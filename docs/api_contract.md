# REST API CONTRACT & INTERNAL BRIDGE SPECIFICATION

**Dự án:** Website đặt bàn và đặt món trước cho nhà hàng tích hợp chatbot AI (`QL09-W`)  
**Jira Key:** `SCRUM` (`SCRUM-14`, `SCRUM-18`) | **Backend:** Python FastAPI | **Frontend:** ReactJS

---

## 1. Quy ước Chung (Global API Conventions)

- **Base URL:** `/api/v1` (Swagger UI tại `/docs`, OpenAPI JSON tại `/openapi.json`)
- **Xác thực & Phân quyền:**
  - Header: `Authorization: Bearer <access_token>` (JWT HS256).
  - 3 vai trò trong JWT payload (`role`): `CUSTOMER`, `STAFF`, `MANAGER`.
  - Truy cập ngoài quyền luôn trả về HTTP `403 Forbidden` và tự động ghi 1 dòng vào bảng `audit_logs` (`US-07`).
- **Chuẩn Thời gian & Tiền tệ:**
  - Mọi trường ngày giờ trao đổi theo chuẩn ISO-8601 có offset `+07:00` (`GMT+7`), ví dụ: `"2026-11-20T19:00:00+07:00"`.
  - Mọi trường tiền tệ là số nguyên `VND` (ví dụ `350000`), không dùng số thực dấu phẩy động.
- **Format Lỗi Thống nhất:**
  ```json
  {
    "error_code": "TABLE_ALREADY_HELD",
    "message": "Bàn này vừa được khách khác giữ chỗ. Vui lòng chọn bàn hoặc khung giờ khác.",
    "details": {
      "suggested_slots": ["18:30", "20:00"],
      "suggested_area_id": 2
    }
  }
  ```

### Bảng Mã lỗi Nghiệp vụ Chuẩn
| HTTP Status | `error_code` | Khi nào trả về |
| :---: | :--- | :--- |
| `400` | `INVALID_BOOKING_WINDOW` | Đặt trước < 2 giờ hoặc > 30 ngày, hoặc không khớp bước 30 phút (`US-17`) |
| `400` | `EDIT_DEADLINE_PASSED` | Sửa/hủy món khi còn < 2 giờ trước giờ hẹn (`US-21`) |
| `401` | `AUTH_REQUIRED` / `ACCOUNT_LOCKED` | Chưa đăng nhập hoặc tài khoản đang bị khóa 15 phút do sai mật khẩu 5 lần (`US-05`) |
| `403` | `FORBIDDEN_ROLE` | Truy cập API ngoài quyền vai trò (`US-07`) |
| `409` | `TABLE_ALREADY_HELD` | Bàn đã bị giữ/đặt trong khung giờ yêu cầu (`US-18`, `US-33`) |
| `409` | `ITEM_HAS_EXISTING_ORDERS` | Cố xóa món ăn đã từng phát sinh đơn hoặc xóa bàn có đơn tương lai (`US-11`, `US-13`) |
| `422` | `MENU_ITEM_UNAVAILABLE` | Thêm món đang ở trạng thái `OUT_OF_STOCK` hoặc `DISCONTINUED` vào giỏ (`US-12`, `US-21`) |
| `429` | `CHAT_RATE_LIMITED` | Gửi tin nhắn chatbot vượt quá giới hạn tần suất (`US-34`) |
| `503` | `OLLAMA_FALLBACK` | Máy chủ Ollama quá thời gian chờ hoặc bảo trì (`US-29`) |

---

## 2. Internal Python Bridge (`FR-05` ↔ `FR-14`)

Để **Dev Fullstack** (Form Đặt bàn `US-17`) và **Dev AI** (Chatbot Đặt bàn `US-35`) dùng chung 1 nguồn kiểm tra bàn trống duy nhất mà không sửa code của nhau, `backend/app/services/availability_service.py` khóa cứng interface sau từ **Sprint 1 (`SCRUM-18`)**:

```python
# File: backend/app/services/availability_service.py
from datetime import date, time
from pydantic import BaseModel
from sqlalchemy.ext.asyncio import AsyncSession

class TableCandidate(BaseModel):
    table_id: int
    table_code: str
    area_id: int
    area_name: str
    capacity: int
    is_combinable: bool

class AvailabilityResult(BaseModel):
    is_available: bool
    requested_date: date
    requested_time: str          # "HH:MM" (GMT+7)
    guest_count: int
    matched_tables: list[TableCandidate]
    suggested_slots: list[str]   # ["18:00", "18:30", "20:00"] nếu hết bàn ở giờ chọn
    suggested_areas: list[dict]  # [{"area_id": 2, "area_name": "Sân vườn"}]
    reason_code: str | None = None  # None | "OUT_OF_ADVANCE_WINDOW" | "NO_TABLE_IN_SLOT" | "NEEDS_COMBINATION"

async def check_table_availability(
    db: AsyncSession,
    booking_date: date,
    start_time: time,
    guest_count: int,
    area_id: int | None = None,
) -> AvailabilityResult:
    """
    Hàm read-only kiểm tra bàn trống theo bước 30p, lượt phục vụ 90p (lấy từ business_configs).
    - Sprint 1-2: Trả dữ liệu mock đúng kiểu AvailabilityResult để Dev AI test sớm.
    - Sprint 3+: Truy vấn thực tế trên bảng dining_tables & bookings (US-17).
    """
```

---

## 3. Hợp đồng API Chatbot AI (`backend/app/api/v1/chat_router.py` – Dev AI sở hữu)

> **Mount point cố định trong `main.py`:** `app.include_router(chat_router, prefix="/api/v1/chat", tags=["Chatbot AI"])`

### 3.1 `GET /api/v1/chat/health`
Kiểm tra trạng thái kết nối tới máy chủ Ollama và model đã nạp.
- **Response `200 OK`:**
  ```json
  {
    "status": "ok",
    "ollama_connected": true,
    "active_model": "qwen2.5:7b-instruct",
    "fallback_mode": false
  }
  ```

### 3.2 `POST /api/v1/chat/sessions`
Khởi tạo phiên chat mới hoặc đồng bộ phiên chat hiện có của khách vãng lai sau khi khách đăng nhập (`US-29`, `US-35`).
- **Auth:** Optional Bearer Token (nếu có token thì gắn `user_id` vào session).
- **Request Body:**
  ```json
  {
    "existing_session_id": "550e8400-e29b-41d4-a716-446655440000"
  }
  ```
- **Response `200 OK`:**
  ```json
  {
    "session_id": "550e8400-e29b-41d4-a716-446655440000",
    "greeting_message": "Xin chào! Tôi là Trợ lý AI của nhà hàng. Tôi có thể tư vấn thực đơn theo ngân sách/sở thích hoặc hỗ trợ bạn tìm bàn trống.",
    "slot_state": {
      "date": null,
      "time": null,
      "guests": null,
      "area_id": null
    },
    "booking_prefill_url": null
  }
  ```

### 3.3 `GET /api/v1/chat/sessions/{session_id}`
Lấy lịch sử tin nhắn của phiên hội thoại để khôi phục giao diện `ChatWidget` khi khách chuyển trang.
- **Response `200 OK`:**
  ```json
  {
    "session_id": "550e8400-e29b-41d4-a716-446655440000",
    "slot_state": { "date": "2026-11-20", "time": "19:00", "guests": 4, "area_id": 1 },
    "messages": [
      {
        "id": 101,
        "role": "assistant",
        "reply_text": "Xin chào! Tôi là Trợ lý AI...",
        "dish_cards": [],
        "booking_prefill_url": null,
        "feedback": null,
        "created_at": "2026-11-17T19:30:00+07:00"
      }
    ]
  }
  ```

### 3.4 `POST /api/v1/chat/messages` *(Contract chính giữa ReactJS `ChatWidget` & Dev AI)*
Gửi tin nhắn của khách và nhận câu trả lời đã qua RAG + Guardrails từ Ollama (`US-29`, `US-30`, `US-34`, `US-35`).
- **Auth:** Optional Bearer Token.
- **Request Body:**
  ```json
  {
    "session_id": "550e8400-e29b-41d4-a716-446655440000",
    "message": "Gợi ý giúp mình combo 4 người khoảng 800k có món hải sản, tối thứ 6 tuần này 19h còn bàn không?",
    "client_context": {
      "active_booking_id": null
    }
  }
  ```
- **Response `200 OK`:**
  ```json
  {
    "message_id": 102,
    "session_id": "550e8400-e29b-41d4-a716-446655440000",
    "reply_text": "Với ngân sách khoảng 800.000đ cho 4 người, nhà hàng gợi ý 2 món hải sản đang bán bên dưới. Về lịch đặt bàn: Thứ Sáu (20/11/2026) lúc 19:00 (GMT+7) cho 4 khách hiện vẫn còn bàn trống tại Tầng 1 và Sân vườn. Bạn nhấn vào nút bên dưới để xác nhận đặt bàn và đặt cọc nhé!",
    "intent": "booking_assist",
    "dish_cards": [
      {
        "id": 12,
        "name": "Lẩu Thái Hải Sản",
        "price_vnd": 350000,
        "image_url": "/static/menu/lau-thai.jpg",
        "tags": ["cay", "hai_san"],
        "status": "AVAILABLE"
      },
      {
        "id": 18,
        "name": "Mực Hấp Hành Gừng",
        "price_vnd": 220000,
        "image_url": "/static/menu/muc-hap.jpg",
        "tags": ["hai_san"],
        "status": "AVAILABLE"
      }
    ],
    "slot_state": {
      "date": "2026-11-20",
      "time": "19:00",
      "guests": 4,
      "area_id": null,
      "available_slots": ["19:00", "19:30"]
    },
    "booking_prefill_url": "/booking?date=2026-11-20&time=19:00&guests=4",
    "requires_login_for_booking": true,
    "is_fallback": false,
    "fallback_Links": null
  }
  ```
- **Response khi Ollama lỗi hoặc quá thời gian chờ (`200 OK` với `is_fallback: true` để UI hiển thị mượt mà theo `US-29`):**
  ```json
  {
    "message_id": 103,
    "session_id": "550e8400-e29b-41d4-a716-446655440000",
    "reply_text": "Trợ lý AI hiện đang tạm ngưng để bảo trì hoặc quá tải. Bạn vui lòng xem trực tiếp Thực đơn hoặc sử dụng Biểu mẫu Đặt bàn bên dưới nhé!",
    "intent": "fallback",
    "dish_cards": [],
    "slot_state": {},
    "booking_prefill_url": null,
    "requires_login_for_booking": false,
    "is_fallback": true,
    "fallback_links": {
      "menu_url": "/menu",
      "booking_url": "/booking",
      "hotline": "1900-xxxx"
    }
  }
  ```

### 3.5 `POST /api/v1/chat/messages/{message_id}/feedback`
Khách đánh giá câu trả lời của Chatbot là `HELPFUL` (Hữu ích) hoặc `NOT_HELPFUL` (Không hữu ích) (`FR-13`, `US-30`).
- **Request Body:** `{ "feedback": "HELPFUL" }` (hoặc `"NOT_HELPFUL"`)
- **Response `200 OK`:** `{ "message_id": 102, "feedback": "HELPFUL" }`

---

## 4. Hợp đồng API Core Nghiệp vụ (Dev Fullstack sở hữu)

### 4.1 Xác thực & Tài khoản (`/api/v1/auth/*` & `/api/v1/users/*` – `FR-01`)
| Method & Path | Quyền | Request chính | Response chính |
| :--- | :---: | :--- | :--- |
| `POST /api/v1/auth/register` | Public | `{ full_name, phone, email, password }` | `201 Created` `{ id, email, phone, role: "CUSTOMER" }` |
| `POST /api/v1/auth/login` | Public | `{ identifier (email/phone), password }` | `200 OK` `{ access_token, expires_in, user: { id, full_name, role } }` |
| `POST /api/v1/auth/logout` | Đã login | Header JWT | `200 OK` (Hủy phiên hiện tại) |
| `POST /api/v1/auth/forgot-password` | Public | `{ email }` | `200 OK` (Gửi link 1 lần qua email `US-06`) |
| `POST /api/v1/auth/reset-password` | Public | `{ token, new_password }` | `200 OK` |
| `GET /api/v1/users/staff` | `MANAGER` | Query `?is_active=true` | `200 OK` Danh sách nhân viên (`US-07`) |
| `POST /api/v1/users/staff` | `MANAGER` | `{ full_name, phone, email, password, role: "STAFF" }` | `201 Created` |
| `PATCH /api/v1/users/staff/{id}/status` | `MANAGER` | `{ is_active: false }` | `200 OK` (Khóa/mở khóa tài khoản NV) |

### 4.2 Thực đơn (`/api/v1/menu/*` – `FR-02`, `FR-03`)
| Method & Path | Quyền | Request / Query Params | Response chính |
| :--- | :---: | :--- | :--- |
| `GET /api/v1/menu/categories` | Public | — | Danh sách danh mục đang active |
| `GET /api/v1/menu/items` | Public | `?q=&category_id=&min_price=&max_price=&tag=` | Danh sách món (`AVAILABLE` & `OUT_OF_STOCK`; ẩn `DISCONTINUED`) (`US-09`, `US-10`) |
| `POST /api/v1/menu/items` | `MANAGER` | `{ category_id, name, description, price_vnd, image_url, tags }` | `201 Created` (`US-11`) |
| `PUT /api/v1/menu/items/{id}` | `MANAGER` | Cập nhật thông tin/giá món | `200 OK` (Tự động ghi `menu_price_history` nếu `price_vnd` đổi `US-12`) |
| `PATCH /api/v1/menu/items/{id}/status` | `STAFF`, `MANAGER` | `{ status: "AVAILABLE \| OUT_OF_STOCK \| DISCONTINUED" }` | `200 OK` (`US-12`) |
| `DELETE /api/v1/menu/items/{id}` | `MANAGER` | — | `200 OK` Xóa mềm (`409` nếu món đã có đơn `US-11`) |
| `GET /api/v1/menu/items/{id}/price-history` | `MANAGER` | — | Lịch sử đổi giá của món (`US-12`) |

### 4.3 Khu vực & Sơ đồ bàn (`/api/v1/areas/*`, `/api/v1/tables/*` – `FR-04`)
| Method & Path | Quyền | Request / Query Params | Response chính |
| :--- | :---: | :--- | :--- |
| `GET /api/v1/areas` | Public | — | Danh sách khu vực |
| `GET /api/v1/tables/floor-plan` | `STAFF`, `MANAGER` | `?date=YYYY-MM-DD&time=HH:MM` | Danh sách bàn kèm tọa độ `pos_x, pos_y` và trạng thái tại thời điểm chọn: `EMPTY \| HELD \| BOOKED \| SERVING \| INACTIVE` (`US-14`, `US-23`) |
| `POST /api/v1/tables` | `MANAGER` | `{ area_id, table_code, capacity, is_combinable, pos_x, pos_y }` | `201 Created` (`US-13`) |
| `PUT /api/v1/tables/positions` | `MANAGER` | `{ positions: [{ table_id, pos_x, pos_y }] }` | `200 OK` Lưu sơ đồ kéo thả (`US-14`) |

### 4.4 Đặt bàn, Giỏ món đặt trước & Đặt cọc (`/api/v1/bookings/*`, `/api/v1/payments/*` – `FR-05`, `FR-06`, `FR-07`, `FR-09`)
| Method & Path | Quyền | Mô tả & Xử lý cốt lõi |
| :--- | :---: | :--- |
| `GET /api/v1/bookings/availability` | Public / `CUSTOMER` | Query `?date=2026-11-20&time=19:00&guests=4&area_id=1` → gọi `check_table_availability()` trả slot trống & gợi ý (`US-17`). |
| `POST /api/v1/bookings/hold` | `CUSTOMER` | Body `{ date, time, guest_count, table_id?, area_id?, customer_note?, source? }`. Tạo đơn `PENDING_DEPOSIT`, giữ bàn 10 phút (`hold_expires_at`). Nếu trùng bàn trả `409 TABLE_ALREADY_HELD` (`US-18`). |
| `PUT /api/v1/bookings/{id}/items` | `CUSTOMER` | Body `{ items: [{ menu_item_id, quantity, item_note }] }`. Kiểm tra hạn chót 2h trước giờ hẹn, kiểm tra món `AVAILABLE`, lưu `unit_price_locked`, tính lại `food_subtotal_vnd` và `deposit_required_vnd` (`US-21`, `US-22`). |
| `POST /api/v1/payments/create-deposit-url` | `CUSTOMER` | Body `{ booking_id, accepted_policy: true }`. Tạo bản ghi `payment_transactions` (`INITIATED`) và trả `{ payment_url, txn_ref }` chuyển sang cổng sandbox (`US-24`). |
| `POST /api/v1/payments/webhook/sandbox` | Webhook (HMAC) | Xác thực chữ ký; xử lý idempotent theo `provider_txn_id`. Nếu đơn còn `PENDING_DEPOSIT` → chuyển `CONFIRMED` + gửi email xác nhận. Nếu đơn đã `EXPIRED` nhưng bàn vẫn trống → chuyển `CONFIRMED`; nếu bàn đã bị người khác đặt → đánh dấu hoàn cọc (`US-25`). |
| `GET /api/v1/bookings/my` | `CUSTOMER` | Danh sách & chi tiết đơn của chính khách đang đăng nhập (`US-26`). |
| `POST /api/v1/bookings/{id}/cancel-preview` | `CUSTOMER` | Trả mức % hoàn cọc và số tiền hoàn dự kiến theo mốc `>=24h` (100%), `2..24h` (50%), `<2h` (0%) trước khi khách bấm xác nhận hủy (`US-26`). |
| `POST /api/v1/bookings/{id}/cancel` | `CUSTOMER` | Chuyển đơn sang `CANCELLED`, giải phóng bàn ngay lập tức, tạo `refund_requests` (nếu có tiền hoàn) và gửi email hủy (`US-26`, `US-27`). |

### 4.5 Vận hành Nhân viên, Bếp, Báo cáo & Tham số (`/api/v1/staff/*`, `/api/v1/kitchen/*`, `/api/v1/reports/*`, `/api/v1/configs` – `FR-08`, `FR-11`, `FR-12`)
| Method & Path | Quyền | Mô tả |
| :--- | :---: | :--- |
| `GET /api/v1/staff/bookings` | `STAFF`, `MANAGER` | Lọc theo `date`, `status`, `area_id`, `booking_code`, `phone`; trả kèm cờ `is_late_checkin` (quá hẹn > 15p chưa check-in) (`US-31`, `US-32`). |
| `POST /api/v1/staff/bookings/{id}/check-in` | `STAFF`, `MANAGER` | Chuyển `CONFIRMED` → `CHECKED_IN` (`US-32`). |
| `POST /api/v1/staff/bookings/{id}/complete` | `STAFF`, `MANAGER` | Chuyển `CHECKED_IN` → `COMPLETED` (`US-32`). |
| `POST /api/v1/staff/bookings/{id}/no-show` | `STAFF`, `MANAGER` | Chuyển `CONFIRMED` → `NO_SHOW` (`US-32`). |
| `PUT /api/v1/staff/bookings/{id}/reschedule` | `STAFF`, `MANAGER` | Đổi bàn hoặc giờ hẹn (kiểm tra chống trùng bàn, ghi `audit_logs`, gửi email báo khách `US-33`). |
| `POST /api/v1/staff/bookings/{id}/cancel` | `STAFF`, `MANAGER` | Hủy đơn bởi nhà hàng; nếu chọn ngoại lệ hoàn cọc (`override_refund_percent`) thì bắt buộc role `MANAGER` và có `reason` (`US-33`). |
| `GET /api/v1/kitchen/prep-summary` | `STAFF`, `MANAGER` | Query `?date=YYYY-MM-DD&slot_from=18:00&slot_to=21:00` → Tổng hợp số lượng từng món & ghi chú từ các đơn `CONFIRMED` (hỗ trợ `?export=pdf` `US-37`). |
| `GET /api/v1/reports/overview` | `MANAGER` | Query `?from_date=...&to_date=...` (chặn khoảng > 365 ngày) → Số đơn theo trạng thái, tỷ lệ hủy, tỷ lệ `NO_SHOW`, doanh thu cọc, hoàn cọc, **số đơn đặt ngoài giờ làm việc** (`US-36`). |
| `GET` / `PUT /api/v1/configs` | `MANAGER` | Xem và cập nhật tham số nghiệp vụ trong `business_configs` (`US-20`). |
