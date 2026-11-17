# JIRA TASKBOARD & KẾ HOẠCH CHẠY SONG SONG (8 SPRINT)

**Dự án:** Website đặt bàn và đặt món trước cho nhà hàng tích hợp chatbot AI (`QL09-W`)  
**Jira Project Key:** `SCRUM` | **Phiên bản:** 1.0 | **Ngày lập:** 17/11/2026

> **Mục đích tài liệu:** Bảng công việc chuẩn hóa sẵn sàng nhập lên Jira (khóa `SCRUM`) cho toàn bộ 8 Sprint (Sprint 0 → Sprint 7), chia rõ task cho 4 thành viên (**PM**, **BA**, **Dev Fullstack**, **Dev AI**) theo cơ chế **chạy song song không xung đột Git/code**.

---

## 1. Cấu hình Jira (`SCRUM`) & Quy tắc Chạy Song Song Không Conflict

### 1.1 Thiết lập nhanh Project trên Jira
- **Project Name:** Website đặt bàn và đặt món trước cho nhà hàng tích hợp chatbot AI (`QL09-W`)
- **Project Key:** `SCRUM` *(Toàn bộ Issue Key trên Jira sẽ có dạng `SCRUM-1`, `SCRUM-2`, ...; tên nhánh Git dùng `feature/SCRUM-xx-mo-ta-ngan`)*
- **Template:** Scrum (1 tuần / Sprint, từ `Sprint 0` đến `Sprint 7`)
- **Board Columns:** `To Do` → `In Progress` → `In Review` → `In Testing` → `Done`
- **Priority Mapping:** `Must` = `High` | `Should` = `Medium` | `Could` = `Low`
- **Fix Versions:** `v0.1` (hết S3), `v0.2` (hết S5), `v1.0-rc` (hết S6), `v1.0` (hết S7)
- **Labels gợi ý:** `FE`, `BE`, `DB`, `AI`, `QA`, `PM`, `BA`, `FR-01`..`FR-18`, `US-01`..`US-54`, `Parallel-Safe`

### 1.2 Phân vùng Thư mục Sở hữu (Zero File-Overlap Architecture)

Để **Dev Fullstack** và **Dev AI** code đồng thời mỗi ngày mà **không sửa trùng file**, toàn bộ repository được chia ranh giới cứng như sau:

| Thành viên | Thư mục / File sở hữu tuyệt đối | Tuyệt đối KHÔNG sửa trực tiếp |
| :--- | :--- | :--- |
| **Dev Fullstack** | - `frontend/**` (toàn bộ ReactJS, gồm cả `ChatWidget.tsx`)<br>- `backend/app/core/**` (config, security, database)<br>- `backend/app/api/v1/` (`auth.py`, `menu.py`, `tables.py`, `bookings.py`, `payments.py`, `staff.py`, `kitchen.py`, `reports.py`)<br>- `backend/app/models/core_models.py`<br>- `backend/app/services/` (`availability_service.py`, `booking_service.py`, `payment_service.py`, `mail_service.py`, `scheduler.py`)<br>- `backend/alembic/versions/*_core_*.py`<br>- `backend/tests/core/**`, `docker-compose.yml`, `.github/workflows/**` | - `backend/app/ai/**`<br>- `backend/app/api/v1/chat_router.py`<br>- `backend/app/models/ai_models.py`<br>- `backend/tests/ai/**`, `ai_eval/**`<br>- `ollama/**`, `docker-compose.ollama.yml` |
| **Dev AI** | - `backend/app/ai/**` (`ollama_client.py`, `rag_retriever.py`, `prompts.py`, `datetime_parser.py`, `booking_assistant.py`, `pii_masker.py`, `guardrails.py`, `schemas.py`)<br>- `backend/app/api/v1/chat_router.py`<br>- `backend/app/models/ai_models.py` (`knowledge_docs`, `chat_sessions`, `chat_messages`)<br>- `backend/alembic/versions/*_ai_*.py`<br>- `backend/tests/ai/**`, `ai_eval/**`<br>- `ollama/**`, `docker-compose.ollama.yml` | - `frontend/**`<br>- `backend/app/api/v1/` (các router nghiệp vụ nhà hàng)<br>- `backend/app/models/core_models.py`<br>- `backend/app/services/*` (chỉ import dùng ở chế độ read-only)<br>- `docker-compose.yml` chính |
| **BA & PM** | - Jira (`SCRUM`), MS Project, Figma/Wireframes, Bộ dữ liệu mẫu (`docs/seed_data/`), Kịch bản Test & UAT (`docs/uat/`) | - Không sửa trực tiếp source code `frontend/` và `backend/` |

### 1.3 5 Quy tắc "Khóa biên giới" chống xung đột khi làm việc song song

1. **One-Time Wiring ở Sprint 1 (`SCRUM-18` & `SCRUM-20`):** `backend/app/main.py` mount sẵn `chat_router` tại `/api/v1/chat` và `backend/app/models/__init__.py` import sẵn `ai_models` đúng **1 lần duy nhất** ở Sprint 1. Từ Sprint 2 đến Sprint 7, Dev Fullstack và Dev AI không đụng chung file nào.
2. **Alembic Migration không đụng head:** Toàn bộ 3 bảng AI (`knowledge_docs`, `chat_sessions`, `chat_messages`) nằm trong `ai_models.py` và tạo migration ngay ở Sprint 1 sau khi bảng core xong. Nếu có sửa bảng ở sprint sau, dev phải `git pull --rebase origin develop` trước khi chạy `alembic revision`.
3. **API Contract + Mock JSON trước (`SCRUM-14`):** Schema `POST /api/v1/chat/messages` được chốt ngay ở Sprint 0. Dev Fullstack code giao diện `ChatWidget` bằng mock JSON từ Sprint 3–4 mà không cần đợi Dev AI xong Ollama.
4. **Read-Only Service Bridge (`FR-05` ↔ `FR-14`):** Dev Fullstack định nghĩa sẵn signature `check_table_availability(date, time_slot, guests, area_id)` trong `availability_service.py` từ Sprint 1 (trả mock ở S1–S2, logic DB thật ở S3). Dev AI chỉ `import` gọi ở chế độ **chỉ đọc**, không bao giờ tự ghi vào bảng `bookings`.
5. **PR nhỏ theo Issue Key `SCRUM-xx`:** Mỗi task/story mở nhánh `feature/SCRUM-xx-short-name` từ `develop`, merge nhanh trong 1–2 ngày; PR chạm vào file ngoài vùng sở hữu phải tag PM + dev còn lại review.

---

## 2. Danh sách Epic tạo trước trên Jira (`SCRUM-1` → `SCRUM-10`)

> **Lưu ý đánh mã:** Các mã `SCRUM-1`..`SCRUM-90` dưới đây là **thứ tự đề xuất khi bạn tạo tuần tự trên Jira**. Khi tạo thực tế trên Jira, bạn giữ đúng thứ tự hoặc đối chiếu qua mã `[EP-xx]` / `[US-xx]` đặt ở đầu tiêu đề (Summary).

| Mã đề xuất | Loại | Tên Epic trên Jira (Summary) | Mã Epic | FR liên quan | Sprint mục tiêu |
| :---: | :--- | :--- | :---: | :--- | :--- |
| `SCRUM-1` | Epic | `[EP-00] Khởi động và quản lý dự án` | EP-00 | — | Sprint 0 |
| `SCRUM-2` | Epic | `[EP-01] Nền tảng kỹ thuật, Tài khoản và Phân quyền` | EP-01 | FR-01 | Sprint 1 |
| `SCRUM-3` | Epic | `[EP-02] Quản lý và Hiển thị Thực đơn` | EP-02 | FR-02, FR-03 | Sprint 2, Dự phòng |
| `SCRUM-4` | Epic | `[EP-03] Sơ đồ bàn và Đặt bàn theo khung giờ` | EP-03 | FR-04, FR-05 | Sprint 2, 3, 4 |
| `SCRUM-5` | Epic | `[EP-04] Đặt món trước và Đặt cọc trực tuyến` | EP-04 | FR-06, FR-07, FR-09 | Sprint 3, 4, Dự phòng |
| `SCRUM-6` | Epic | `[EP-05] Vận hành đặt chỗ, Thông báo và Bếp` | EP-05 | FR-08, FR-10, FR-11 | Sprint 4, 5, 6, Dự phòng |
| `SCRUM-7` | Epic | `[EP-06] Báo cáo thống kê vận hành` | EP-06 | FR-12 | Sprint 6, Dự phòng |
| `SCRUM-8` | Epic | `[EP-07] Chatbot AI (Ollama) tư vấn & đặt bàn` | EP-07 | FR-13, FR-14, FR-15 | Sprint 2, 5, 6, Dự phòng |
| `SCRUM-9` | Epic | `[EP-08] Chức năng mở rộng (Sau MVP)` | EP-08 | FR-16, FR-17, FR-18 | Dự phòng |
| `SCRUM-10` | Epic | `[EP-09] Staging, Kiểm thử, UAT và Phát hành v1.0` | EP-09 | — | Sprint 4, 7 |

---

## 3. Taskboard Chi tiết Theo 8 Sprint (Sẵn sàng đẩy lên Jira)

### Sprint 0 – Tuần 1: Khởi động, Thiết lập Jira `SCRUM` & Khung hạ tầng *(Mốc M1)*

- **Sprint Goal:** Đề tài được duyệt, nhóm 4 người và công cụ sẵn sàng; chốt kiến trúc ReactJS + FastAPI + PostgreSQL + Ollama và hợp đồng API song song.
- **Luồng chạy song song (0% conflict):** PM thiết lập công cụ quản lý | BA khảo sát nhà hàng | Dev Fullstack dựng Repo + skeleton FE/BE/PG | Dev AI cài Ollama local & chốt JSON Contract.

| Mã đề xuất | Loại | Epic / US | Tiêu đề trên Jira (Summary) | Assignee | SP | Ưu tiên | Vùng file / Song song | Blocked By | Tiêu chí Done (AC) |
| :---: | :--- | :--- | :--- | :--- | :---: | :--- | :--- | :--- | :--- |
| `SCRUM-11` | Task | `EP-00` | `[PM] Khởi tạo Jira (SCRUM), MS Project, Kick-off và Bộ tài liệu D01/D14/D17` | PM | — | High | `[Quản lý]` Jira, MS Project | Không | Tạo xong 10 Epic, mời GV vào Jira, chốt RACI & quy ước 4 người. |
| `SCRUM-12` | Task | `EP-00` | `[BA] Khảo sát nhà hàng, hoàn thiện SRS/Backlog và gom dữ liệu thô` | BA | — | High | `[Tài liệu]` `docs/` | Không | Có biên bản khảo sát, chốt `SRS v1.0`, có menu/bàn/chính sách thô. |
| `SCRUM-13` | Task | `EP-00` | `[FS] Khởi tạo Repo GitHub, branch rules và skeleton ReactJS + FastAPI + PG` | Dev Fullstack | — | High | `frontend/`, `backend/app/core/`, `docker-compose.yml` | Không | Repo có nhánh `main`/`develop`, bật PR ≥ 1 review, chạy được PG container. |
| `SCRUM-14` | Task | `EP-00` | `[AI] Cài đặt Ollama local, kéo thử model VN và chốt Chat API JSON Contract` | Dev AI | — | High | `ollama/`, `docs/chat_contract.md` | Không | Ollama chạy ổn định ≥ 2 model; 2 Dev chốt xong JSON response `/api/v1/chat/messages`. |
| `SCRUM-15` | Task | `EP-00` | `[FS] Khảo sát cổng thanh toán Sandbox (VNPay/MoMo/PayOS) & Email/SMS` | Dev Fullstack | — | Medium | `[Nghiên cứu]` Không sửa code | Không | Có bảng so sánh sandbox, phí & phương án gửi mail đề xuất cho PM chốt đầu S1. |

---

### Sprint 1 – Tuần 2: Nền tảng kỹ thuật, ERD, Auth/RBAC & Benchmark Ollama *(Mốc M2 – 28 SP)*

- **Sprint Goal:** Nền tảng ReactJS + FastAPI + PostgreSQL chạy được; người dùng đăng ký, đăng nhập và phân quyền 3 vai trò; hoàn thành benchmark model Ollama.
- **Luồng chạy song song:**
  - **Ngày 1–2:** Dev Fullstack làm `SCRUM-18` (khung + mount `chat_router`) & `SCRUM-19` (ERD core) | BA làm `SCRUM-17` (Wireframe) | Dev AI làm `SCRUM-21` (Benchmark Ollama trên ≥ 10 câu VN, không chờ code BE!) | PM làm `SCRUM-16` (Baseline M2).
  - **Ngày 3–5:** Sau khi `SCRUM-18`, `SCRUM-19` merge vào `develop`, Dev Fullstack code Auth (`SCRUM-22`–`25`) + CI (`SCRUM-26`), đồng thời Dev AI code `ai_models.py` (`SCRUM-20`) & `OllamaClient` trong `backend/app/ai/` — **khác thư mục hoàn toàn, 0 conflict!**

| Mã đề xuất | Loại | Epic / US | Tiêu đề trên Jira (Summary) | Assignee | SP | Ưu tiên | Vùng file / Song song | Blocked By | Tiêu chí Done (AC) |
| :---: | :--- | :--- | :--- | :--- | :---: | :--- | :--- | :--- | :--- |
| `SCRUM-16` | Task | `EP-01` | `[PM] Lưu Baseline MS Project (M2), lập DoR/DoD, Sổ rủi ro và chốt Sandbox` | PM | — | High | `[Quản lý]` MS Project | `SCRUM-11`, `SCRUM-15` | Lưu Baseline M2; ban hành DoR/DoD & Sổ rủi ro v1.0; chốt cổng TT sandbox. |
| `SCRUM-17` | Story | `EP-01` / `US-03` | `[US-03] Thiết kế Wireframe/Prototype luồng chính trên Figma` | BA | 5 | High | `[Thiết kế]` Figma (không sửa code) | `SCRUM-12` | Có prototype mobile + desktop cho toàn bộ luồng chính; PM duyệt trước khi ghép UI. |
| `SCRUM-18` | Story | `EP-01` / `US-01` | `[US-01] Dựng khung ReactJS + FastAPI (/health, Swagger) + PG & mount chat_router` | Dev Fullstack | 5 | High | `frontend/`, `backend/app/main.py`, `services/availability_service.py` | `SCRUM-13` | `/health` trả 200, Swagger chạy, ReactJS gọi được API mẫu; mount sẵn placeholder `chat_router` & `availability_service` interface. |
| `SCRUM-19` | Story | `EP-01` / `US-02` | `[US-02] Thiết kế ERD & Alembic migration core (kèm constraint chống trùng bàn)` | Dev Fullstack | 3 | High | `backend/app/models/core_models.py`, `alembic/` | `SCRUM-18` | ERD lưu repo; migration tạo đủ bảng core trên DB trống + ràng buộc DB chống trùng; ≥ 1 review. |
| `SCRUM-20` | Task | `EP-01` / `US-02` | `[AI] Tạo ai_models.py (knowledge_docs, chat_sessions, chat_messages) & migration AI` | Dev AI | — | High | `backend/app/models/ai_models.py`, `alembic/*_ai_*` | `SCRUM-19` *(lấy head migration mới nhất)* | 3 bảng AI tạo thành công trên PostgreSQL; không sửa `core_models.py`. |
| `SCRUM-21` | Task | `EP-07` / `US-15` *(sớm)* | `[AI] Benchmark ≥ 2 model Ollama trên ≥ 10 câu tiếng Việt & dựng OllamaClient` | Dev AI | — | High | `backend/app/ai/ollama_client.py`, `chat_router.py`, `ollama/` | `SCRUM-14`, `SCRUM-18` *(song song Auth)* | Có bảng đo RAM/latency/tiếng Việt ≥ 10 câu; `/api/v1/chat/health` trả OK khi Ollama sẵn sàng. |
| `SCRUM-22` | Story | `EP-01` / `US-04` | `[US-04] Đăng ký tài khoản (BE FastAPI + FE ReactJS)` | Dev Fullstack | 3 | High | `api/v1/auth.py`, `frontend/src/pages/auth/` | `SCRUM-19` | Check trùng email/SĐT; hash mật khẩu; gửi email xác thực; validate lỗi rõ ràng. |
| `SCRUM-23` | Story | `EP-01` / `US-05` | `[US-05] Đăng nhập, Đăng xuất & Khóa 15p khi sai quá 5 lần` | Dev Fullstack | 3 | High | `api/v1/auth.py`, `frontend/src/pages/auth/` | `SCRUM-22` | JWT có thời hạn; điều hướng theo 3 roles; sai 5 lần khóa 15p; đăng xuất hủy phiên. |
| `SCRUM-24` | Story | `EP-01` / `US-06` | `[US-06] Quên mật khẩu qua email (link dùng 1 lần)` | Dev Fullstack | 2 | Medium | `api/v1/auth.py`, `frontend/src/pages/auth/` | `SCRUM-22` | Gửi link có thời hạn; dùng 1 lần duy nhất; mật khẩu mới lưu dạng băm. |
| `SCRUM-25` | Story | `EP-01` / `US-07` | `[US-07] Phân quyền RBAC 3 vai trò & Quản lý tài khoản Nhân viên` | Dev Fullstack | 5 | High | `core/security.py`, `api/v1/users.py`, `frontend/src/pages/admin/` | `SCRUM-23` | 3 roles (Khách/NV/Quản lý); ngoài quyền trả 403 + ghi audit log; Quản lý tạo/khóa NV. |
| `SCRUM-26` | Story | `EP-01` / `US-08` | `[US-08] Thiết lập CI bằng GitHub Actions (Pytest + React Build)` | Dev Fullstack | 2 | Medium | `.github/workflows/ci.yml` | `SCRUM-18` | Mọi PR tự động chạy pytest & build ReactJS; lỗi build chặn merge vào `main`. |

---

### Sprint 2 – Tuần 3: Thực đơn, Sơ đồ bàn, Dữ liệu mẫu & Lõi RAG trên Ollama *(29 SP)*

- **Sprint Goal:** Khách xem/tìm thực đơn; Quản lý quản lý thực đơn & sơ đồ bàn; nạp dữ liệu mẫu ẩn danh; chốt chính thức model Ollama và dựng xong tầng RAG.
- **Luồng chạy song song (0% conflict):** Dev Fullstack code trong `frontend/src/pages/menu|tables/` + `backend/app/api/v1/menu|tables.py` | Dev AI làm báo cáo `US-15` + code `backend/app/ai/rag_retriever.py` & `prompts.py` | BA chuẩn hóa dữ liệu `US-16` và kiểm thử S1–S2.

| Mã đề xuất | Loại | Epic / US | Tiêu đề trên Jira (Summary) | Assignee | SP | Ưu tiên | Vùng file / Song song | Blocked By | Tiêu chí Done (AC) |
| :---: | :--- | :--- | :--- | :--- | :---: | :--- | :--- | :--- | :--- |
| `SCRUM-27` | Task | `EP-02` | `[PM] Review Sprint 1, lập EVM #1, duyệt chốt model Ollama (US-15)` | PM | — | High | `[Quản lý]` MS Project | `SCRUM-21` | Có báo cáo EVM lần 1; ký duyệt phương án Ollama; kiểm soát tiến độ Fullstack. |
| `SCRUM-28` | Story | `EP-02` / `US-16` | `[US-16] Chuẩn hóa bộ dữ liệu mẫu thực tế đã ẩn danh (Menu, Bàn, FAQ)` | BA | 2 | High | `docs/seed_data/` *(JSON/CSV)* | `SCRUM-12` | Tệp thực đơn, sơ đồ bàn, giờ mở cửa, chính sách, FAQ không chứa PII và được nhà hàng xác nhận. |
| `SCRUM-29` | Story | `EP-02` / `US-09` | `[US-09] Xem thực đơn theo danh mục (FE + BE)` | Dev Fullstack | 3 | High | `api/v1/menu.py`, `frontend/src/pages/menu/` | `SCRUM-19` | Chỉ hiển thị món `Đang bán`; món hết hiện `"Tạm hết"` & khóa thêm đơn; giá VND; mobile tốt. |
| `SCRUM-30` | Story | `EP-02` / `US-10` | `[US-10] Tìm kiếm và lọc món ăn theo danh mục, giá, nhãn` | Dev Fullstack | 3 | Medium | `api/v1/menu.py`, `frontend/src/pages/menu/` | `SCRUM-29` | Tìm từ khóa; lọc danh mục, khoảng giá, nhãn (`chay`, `cay`, `hải sản`); thông báo khi rỗng. |
| `SCRUM-31` | Story | `EP-02` / `US-11` | `[US-11] Quản lý danh mục và món ăn (CRUD + xóa mềm)` | Dev Fullstack | 5 | High | `api/v1/menu.py`, `frontend/src/pages/admin/menu/` | `SCRUM-25` | Validate tên không rỗng, giá > 0, ảnh đúng định dạng/dung lượng; xóa mềm; chặn xóa món có đơn. |
| `SCRUM-32` | Story | `EP-02` / `US-12` | `[US-12] Cập nhật tình trạng món & Lưu lịch sử đổi giá` | Dev Fullstack | 3 | High | `api/v1/menu.py`, `frontend/src/pages/staff/` | `SCRUM-31` | 3 trạng thái (`Đang bán`, `Tạm hết`, `Ngừng bán`); lưu `menu_price_history`; chặn đặt món hết. |
| `SCRUM-33` | Story | `EP-03` / `US-13` | `[US-13] Quản lý khu vực và bàn (sức chứa, ghép bàn, ngưng sử dụng)` | Dev Fullstack | 5 | High | `api/v1/tables.py`, `frontend/src/pages/admin/tables/` | `SCRUM-25` | Tạo khu vực, thêm/sửa bàn (mã không trùng, sức chứa ≥ 1, có thể ghép); chặn bàn `Ngưng sử dụng`. |
| `SCRUM-34` | Story | `EP-03` / `US-14` | `[US-14] Sơ đồ bàn trực quan (kéo thả lưu vị trí theo khu vực)` | Dev Fullstack | 5 | Medium | `frontend/src/pages/admin/tables/FloorPlan*` | `SCRUM-33` | Quản lý kéo thả & lưu tọa độ bàn; Nhân viên chỉ xem, không sửa. |
| `SCRUM-35` | Task | `EP-02` / `US-16` | `[FS] Viết Python seed script nạp dữ liệu mẫu từ BA vào PostgreSQL` | Dev Fullstack | — | High | `backend/scripts/seed_core.py` | `SCRUM-28`, `SCRUM-31`, `SCRUM-33` | Nạp sạch thực đơn, khu vực, bàn vào PostgreSQL không lỗi trùng lặp. |
| `SCRUM-36` | Story | `EP-07` / `US-15` | `[US-15] Nghiệm thu Spike: Chốt phương án LLM (Ollama model có sẵn)` | Dev AI | 3 | High | `docs/llm_spike_report.md`, `ollama/Modelfile` | `SCRUM-21` | Báo cáo so sánh chi phí/latency/tiếng Việt/bảo mật giữa Ollama & API trả phí; PM & BA phê duyệt. |
| `SCRUM-37` | Task | `EP-07` / `US-30` *(nền)* | `[AI] Xây dựng RAG Retriever từ PostgreSQL, System Prompt & JSON dish_cards[]` | Dev AI | — | High | `backend/app/ai/rag_retriever.py`, `prompts.py`, `scripts/seed_ai.py` | `SCRUM-20`, `SCRUM-28`, `SCRUM-36` | Truy xuất món `Đang bán` + `knowledge_docs` realtime; trả lời JSON kèm `dish_cards[]` qua Swagger. |

---

### Sprint 3 – Tuần 4: Đặt bàn chống trùng, Đặt món trước & Parser `GMT+7` *(Mốc M3 – Tag `v0.1` – 29 SP)*

- **Sprint Goal:** Khách đặt được bàn theo khung giờ không trùng bàn và đặt món trước; Dev AI hoàn thành parser thời gian `GMT+7`, state machine đặt bàn & lọc PII v1.
- **Luồng chạy song song (0% conflict):** Dev Fullstack code toàn bộ `bookings` & `cart` trong `api/v1/bookings.py` + `services/availability_service.py` + `frontend/src/pages/booking/` | Dev AI code độc lập trong `backend/app/ai/datetime_parser.py`, `booking_assistant.py`, `pii_masker.py`.

| Mã đề xuất | Loại | Epic / US | Tiêu đề trên Jira (Summary) | Assignee | SP | Ưu tiên | Vùng file / Song song | Blocked By | Tiêu chí Done (AC) |
| :---: | :--- | :--- | :--- | :--- | :---: | :--- | :--- | :--- | :--- |
| `SCRUM-38` | Task | `EP-03` | `[PM] Cập nhật Burndown/EVM, kiểm tra mốc M3 & gắn tag GitHub v0.1` | PM | — | High | `[Quản lý]` Jira, MS Project, GitHub | `SCRUM-40`–`45` | Đủ hồ sơ mốc M3; tag `v0.1` trên `develop`/`main`; kiểm tra sẵn sàng payment sandbox. |
| `SCRUM-39` | Task | `EP-03` | `[BA] Test case đặt bàn/đồng thời, chốt tham số mặc định & 15+ mẫu hội thoại AI` | BA | — | High | `docs/testcases/`, `docs/ai_dialogues/` | `SCRUM-28` | Bộ test case đặt bàn + đồng thời; xác nhận tham số `US-20`; 15+ mẫu câu đặt bàn gửi Dev AI. |
| `SCRUM-40` | Story | `EP-03` / `US-20` | `[US-20] Cấu hình tham số nghiệp vụ (lượt 90p, giữ 10p, % cọc, hạn sửa 2h)` | Dev Fullstack | 3 | High | `api/v1/configs.py`, `frontend/src/pages/admin/configs/` | `SCRUM-25`, `SCRUM-39` | Đủ tham số theo SRS; mặc định chuẩn SRS; thay đổi chỉ áp dụng cho đơn mới. |
| `SCRUM-41` | Story | `EP-03` / `US-17` | `[US-17] Service & API Kiểm tra bàn trống (bước 30p, lượt 90p, 2h–30 ngày, GMT+7)` | Dev Fullstack | 5 | High | `services/availability_service.py`, `api/v1/bookings.py` | `SCRUM-33`, `SCRUM-40` | Trả bàn đúng sức chứa & không trùng khung giờ; hết bàn gợi ý giờ/khu vực gần nhất; AI import dùng được. |
| `SCRUM-42` | Story | `EP-03` / `US-18` | `[US-18] Giữ chỗ tạm thời 10p chống trùng bàn + Concurrent Test + Job hết hạn` | Dev Fullstack | 8 | High | `services/booking_service.py`, `scheduler.py`, `tests/core/` | `SCRUM-41` | Tạo đơn `Chờ cọc` giữ 10p; test đồng thời 2 request cùng bàn/giờ chỉ 1 thành công; hết hạn giải phóng bàn. |
| `SCRUM-43` | Story | `EP-03` / `US-19` | `[US-19] Giao diện ReactJS Đặt bàn (kèm đồng hồ đếm ngược giữ chỗ)` | Dev Fullstack | 5 | High | `frontend/src/pages/booking/BookingPage*` | `SCRUM-41` | Nhập ngày, giờ, số khách, khu vực, ghi chú; hiển thị slot trống & countdown 10p; mobile responsive. |
| `SCRUM-44` | Story | `EP-04` / `US-21` | `[US-21] Giỏ món đặt trước gắn đơn đặt bàn (1–20 phần, khóa sửa trước 2h)` | Dev Fullstack | 5 | High | `api/v1/bookings.py`, `frontend/src/pages/booking/PreorderCart*` | `SCRUM-29`, `SCRUM-42` | Thêm/sửa/xóa món (1–20) + ghi chú; chặn món `Tạm hết`; khóa sửa trước giờ hẹn 2h; tạm tính realtime. |
| `SCRUM-45` | Story | `EP-04` / `US-22` | `[US-22] Chốt món, lưu giá chốt (unit_price_locked) và tính tiền cọc` | Dev Fullstack | 3 | High | `services/booking_service.py`, `frontend/src/pages/booking/` | `SCRUM-44` | Lưu giá chốt từng món; tính tiền cọc (kể cả đơn không đặt món); hiển thị chính sách hủy trước cọc. |
| `SCRUM-46` | Task | `EP-07` / `US-35` *(nền)* | `[AI] Parser thời gian tiếng Việt GMT+7 & Slot-filling đặt bàn (read-only FR-05)` | Dev AI | — | High | `backend/app/ai/datetime_parser.py`, `booking_assistant.py`, `tests/ai/` | `SCRUM-37`, `SCRUM-39` | Parse đúng ≥ 15 mẫu câu của BA sang `GMT+7`; validate 2h–30 ngày; sinh URL pre-fill `/booking?...`. |
| `SCRUM-47` | Task | `EP-07` / `US-34` *(nền)* | `[AI] Xây dựng PII Masker v1 (che tên/SĐT/email) & bộ lọc ngoài phạm vi/y tế` | Dev AI | — | High | `backend/app/ai/pii_masker.py`, `guardrails.py`, `tests/ai/` | `SCRUM-37` | SĐT (`0x`/`+84`), email được che sạch trước khi gửi Ollama; phát hiện câu hỏi y tế/dị ứng. |

---

### Sprint 4 – Tuần 5: Đặt cọc Sandbox, Hủy đơn, Email & Triển khai Staging *(27 SP)*

- **Sprint Goal:** Khách đặt cọc qua cổng sandbox, xem và hủy đặt chỗ, nhận email; bản demo (gồm Ollama và khung chat thử nghiệm) chạy hoàn chỉnh trên Staging.
- **Luồng chạy song song (0% conflict):** Dev Fullstack code `payments.py`, `mail_service.py`, `MyBookings`, `docker-compose.yml` + `ChatWidget` bằng mock/API contract | Dev AI hoàn thiện `chat_router.py` + đóng gói `docker-compose.ollama.yml` lên Staging rồi bật cờ kết nối thật.

| Mã đề xuất | Loại | Epic / US | Tiêu đề trên Jira (Summary) | Assignee | SP | Ưu tiên | Vùng file / Song song | Blocked By | Tiêu chí Done (AC) |
| :---: | :--- | :--- | :--- | :--- | :---: | :--- | :--- | :--- | :--- |
| `SCRUM-48` | Task | `EP-09` | `[PM] Thiết lập quy trình CR/CCB, kiểm soát Staging và chốt lịch UAT Tuần 8` | PM | — | High | `[Quản lý]` Jira CR, Lịch UAT | `SCRUM-38` | Có quy trình CR/CCB; chốt ngày UAT Tuần 8 với nhà hàng; kiểm tra URL Staging. |
| `SCRUM-49` | Task | `EP-04` | `[BA] Test case thanh toán/callback/hoàn cọc & rà soát câu trả lời AI trên Staging` | BA | — | High | `docs/testcases/` | `SCRUM-39` | Kiểm thử đủ 4 kịch bản callback & 3 mốc hoàn cọc; ghi nhận phản hồi chất lượng AI. |
| `SCRUM-50` | Story | `EP-03` / `US-23` | `[US-23] Xem trạng thái từng bàn theo ngày, giờ (5 trạng thái)` | Dev Fullstack | 3 | High | `api/v1/tables.py`, `frontend/src/pages/staff/TableStatus*` | `SCRUM-42` | Hiển thị `Trống`, `Đang giữ`, `Đã đặt`, `Đang phục vụ`, `Ngưng sử dụng` cho Nhân viên & Quản lý. |
| `SCRUM-51` | Story | `EP-04` / `US-24` | `[US-24] Đặt cọc trực tuyến: tạo giao dịch & chuyển hướng cổng Sandbox` | Dev Fullstack | 5 | High | `api/v1/payments.py`, `frontend/src/pages/booking/Checkout*` | `SCRUM-16`, `SCRUM-45` | Hiển thị tóm tắt + chính sách hủy + checkbox đồng ý; lưu mã GD, số tiền, thời điểm; không lưu số thẻ. |
| `SCRUM-52` | Story | `EP-04` / `US-25` | `[US-25] Xử lý Callback/Webhook cổng thanh toán (chữ ký, idempotent, đến muộn)` | Dev Fullstack | 8 | High | `api/v1/payments.py`, `services/payment_service.py`, `tests/core/` | `SCRUM-51` | Verify HMAC; chuyển `Đã xác nhận` + mã đơn; gửi trùng không cộng 2 lần; xử lý trễ/lỗi đúng SRS. |
| `SCRUM-53` | Story | `EP-04` / `US-26` | `[US-26] Trang "Đặt chỗ của tôi": xem, hủy đơn & tạo yêu cầu hoàn cọc` | Dev Fullstack | 5 | High | `api/v1/bookings.py`, `frontend/src/pages/customer/MyBookings*` | `SCRUM-52` | Xem lịch sử đơn; hiển thị % hoàn theo mốc trước khi hủy; giải phóng bàn & tạo yêu cầu hoàn cọc. |
| `SCRUM-54` | Story | `EP-05` / `US-27` | `[US-27] Gửi Email xác nhận đặt cọc và hủy đặt chỗ (retry 3 lần + log)` | Dev Fullstack | 3 | High | `services/mail_service.py` | `SCRUM-52`, `SCRUM-53` | Gửi email đủ mã đơn, giờ, bàn, chính sách; lỗi tự retry tối đa 3 lần và lưu log gửi. |
| `SCRUM-55` | Story | `EP-09` / `US-28` | `[US-28] Triển khai Staging tự động từ nhánh develop + gắn ChatWidget v0` | Dev Fullstack | 3 | Medium | `docker-compose.yml`, `.github/workflows/deploy-staging.yml`, `frontend/src/components/chat/` | `SCRUM-26`, `SCRUM-52` | Truy cập Staging qua URL; auto-deploy từ `develop`; có dữ liệu mẫu & khung Chat Widget gọi API AI. |
| `SCRUM-56` | Task | `EP-07` / `US-29` *(nền)* | `[AI] Đóng gói docker-compose.ollama.yml lên Staging & Fallback khi Ollama lỗi` | Dev AI | — | High | `docker-compose.ollama.yml`, `ollama/`, `backend/app/ai/ollama_client.py` | `SCRUM-36` | Ollama container chạy ổn trên Staging; ngắt thử Ollama trả mã `is_fallback: true` sạch sẽ. |
| `SCRUM-57` | Task | `EP-07` / `US-29` *(nền)* | `[AI] Hoàn thiện REST API /api/v1/chat/* (sessions, messages, feedback) & nối với Staging` | Dev AI | — | High | `backend/app/api/v1/chat_router.py`, `backend/app/ai/` | `SCRUM-46`, `SCRUM-47`, `SCRUM-56` | Toàn bộ endpoint `/api/v1/chat/*` chạy live trên Staging; lưu DB `chat_sessions`/`chat_messages`. |

---

### Sprint 5 – Tuần 6: Vận hành Nhân viên & Chatbot AI Tư vấn trên dữ liệu thật *(Mốc M4 – Tag `v0.2` – 31 SP)*

- **Sprint Goal:** Nhân viên xử lý trọn vẹn đơn đặt chỗ; Chatbot AI chính thức tư vấn món và chính sách trên dữ liệu thật ngay trên website.
- **Luồng chạy song song (0% conflict):** Dev Fullstack code màn hình Nhân viên (`staff/`, `api/v1/staff.py`) & hoàn thiện UI thẻ món trong `ChatWidget.tsx` | Dev AI tối ưu prompt/RAG cho `US-29` & `US-30` trong `backend/app/ai/` và đẩy bản preview đặt bàn chat (`US-35`) lên Staging.

| Mã đề xuất | Loại | Epic / US | Tiêu đề trên Jira (Summary) | Assignee | SP | Ưu tiên | Vùng file / Song song | Blocked By | Tiêu chí Done (AC) |
| :---: | :--- | :--- | :--- | :--- | :---: | :--- | :--- | :--- | :--- |
| `SCRUM-58` | Task | `EP-05` | `[PM] Cập nhật EVM #2, kiểm tra mốc M4, gắn tag v0.2 & chốt phạm vi S6` | PM | — | High | `[Quản lý]` Jira, MS Project, GitHub | `SCRUM-60`–`65` | Báo cáo EVM M4; tag `v0.2`; quyết định giữ/lùi `US-37`/`US-38` theo Velocity thực tế. |
| `SCRUM-59` | Task | `EP-07` | `[BA] Duyệt tri thức Chatbot, test nghiệp vụ Nhân viên & soạn bộ 30+ câu test AI` | BA | — | High | `docs/testcases/`, `docs/ai_eval/` | `SCRUM-49` | Kiểm duyệt FAQ/chính sách; log Bug luồng Nhân viên; có ≥ 30 câu hỏi nháp cho `US-40`. |
| `SCRUM-60` | Story | `EP-05` / `US-31` | `[US-31] Danh sách, bộ lọc và chi tiết đơn đặt chỗ cho Nhân viên/Quản lý` | Dev Fullstack | 5 | High | `api/v1/staff.py`, `frontend/src/pages/staff/BookingList*` | `SCRUM-52` | Lọc theo ngày, trạng thái, khu vực, mã đơn, SĐT; xem đủ khách/bàn/món/cọc/lịch sử; khóa Khách hàng. |
| `SCRUM-61` | Story | `EP-05` / `US-32` | `[US-32] Check-in, Hoàn thành, nhắc quá 15p & đánh dấu Không đến` | Dev Fullstack | 5 | High | `api/v1/staff.py`, `scheduler.py`, `frontend/src/pages/staff/` | `SCRUM-60` | Chuyển `Đã đến`/`Hoàn thành`; cảnh báo quá giờ 15p chưa check-in; đánh dấu `Không đến`; chặn sửa trạng thái cuối. |
| `SCRUM-62` | Story | `EP-05` / `US-33` | `[US-33] Đổi bàn/đổi giờ, hủy bởi Nhân viên (ngoại lệ Quản lý) & nhật ký` | Dev Fullstack | 5 | High | `api/v1/staff.py`, `services/booking_service.py`, `frontend/src/pages/staff/` | `SCRUM-60` | Chặn đổi sang bàn/giờ trùng; hủy ngoại lệ chỉ Quản lý + lý do; lưu log người/giờ/cũ-mới & email khách. |
| `SCRUM-63` | Task | `EP-07` / `US-29,30` *(FE)* | `[FS] Hoàn thiện ReactJS ChatWidget (thẻ món "Thêm vào đơn", feedback, fallback UI)` | Dev Fullstack | — | High | `frontend/src/components/chat/` | `SCRUM-55`, `SCRUM-57` | Nêu rõ là trợ lý AI; bấm *"Thêm vào đơn"* trên thẻ món thêm thẳng vào giỏ `US-21`; có nút Hữu ích/Không hữu ích. |
| `SCRUM-64` | Story | `EP-07` / `US-29` | `[US-29] Nghiệm thu tích hợp Ollama, lưu hội thoại multi-turn & fallback` | Dev AI | 8 | High | `backend/app/ai/`, `chat_router.py` | `SCRUM-57`, `SCRUM-63` | Hội thoại lưu đủ vào PostgreSQL; gọi Ollama ổn định; ngắt Ollama hiển thị hướng dẫn dùng menu/form đặt bàn. |
| `SCRUM-65` | Story | `EP-07` / `US-30` | `[US-30] Nghiệm thu Chatbot tư vấn món theo ngân sách/số người/nhãn & chính sách thật` | Dev AI | 8 | High | `backend/app/ai/rag_retriever.py`, `prompts.py` | `SCRUM-37`, `SCRUM-64` | Giá/trạng thái khớp DB 100% tại thời điểm hỏi; không có dữ liệu thì nói chưa biết & chỉ cách liên hệ NV. |

---

### Sprint 6 – Tuần 7: Guardrails, Đặt bàn qua Chat, Báo cáo, Bếp & Nhắc lịch *(Mốc M5 – Feature Freeze – Tag `v1.0-rc` – 31 SP)*

- **Sprint Goal:** Chatbot an toàn (chống bịa tin, bảo vệ PII) và đặt bàn qua chat; hoàn thiện báo cáo thống kê, danh sách bếp và nhắc lịch 24h/2h; **Đóng băng tính năng cuối sprint**.
- **Luồng chạy song song (0% conflict):** Dev Fullstack code `reports.py`, `kitchen.py`, `scheduler.py` (nhắc 24h/2h) & nối pre-fill URL params ở `BookingPage.tsx` | Dev AI hoàn thiện `guardrails.py` (`US-34`), `booking_assistant.py` (`US-35`) & test với BA.

| Mã đề xuất | Loại | Epic / US | Tiêu đề trên Jira (Summary) | Assignee | SP | Ưu tiên | Vùng file / Song song | Blocked By | Tiêu chí Done (AC) |
| :---: | :--- | :--- | :--- | :--- | :---: | :--- | :--- | :--- | :--- |
| `SCRUM-66` | Task | `EP-06` | `[PM] Cập nhật EVM & dự báo, rà soát rủi ro, tuyên bố Feature Freeze (M5 – v1.0-rc)` | PM | — | High | `[Quản lý]` Jira, MS Project, GitHub | `SCRUM-68`–`73` | Hoàn thành mốc M5; gắn tag `v1.0-rc`; đóng cổng nhận tính năng mới từ Sprint 7. |
| `SCRUM-67` | Task | `EP-09` | `[BA] Lập kịch bản UAT (US-41) & test ≥ 20 câu Guardrails (US-34) + Báo cáo/Bếp` | BA | — | High | `docs/uat/`, `docs/testcases/` | `SCRUM-59` | Có kịch bản UAT gửi nhà hàng; xác nhận 20+ câu Guardrails không bịa/lộ PII; test số liệu báo cáo. |
| `SCRUM-68` | Story | `EP-06` / `US-36` | `[US-36] Báo cáo thống kê (đơn, tỷ lệ hủy/no-show, doanh thu cọc, đặt ngoài giờ)` | Dev Fullstack | 8 | High | `api/v1/reports.py`, `frontend/src/pages/admin/reports/` | `SCRUM-61` | Đủ bảng + biểu đồ; tính đúng tỷ lệ không đến & số đơn đặt ngoài giờ; chỉ Quản lý truy cập. |
| `SCRUM-69` | Story | `EP-05` / `US-37` | `[US-37] Tổng hợp món chuẩn bị cho bếp theo ngày/khung giờ (In / Xuất PDF)` | Dev Fullstack | 5 | Medium | `api/v1/kitchen.py`, `frontend/src/pages/staff/Kitchen*` | `SCRUM-52` | Tổng hợp từ đơn `Đã xác nhận` kèm ghi chú; tự cập nhật khi hủy/sửa món; xuất PDF hoặc in. |
| `SCRUM-70` | Story | `EP-05` / `US-38` | `[US-38] Tự động nhắc lịch trước 24 giờ và 2 giờ qua Email (SMS nếu bật)` | Dev Fullstack | 5 | Medium | `services/scheduler.py`, `services/mail_service.py` | `SCRUM-54` | Bỏ qua đơn đã hủy; nhắc đúng mốc 24h & 2h; retry tối đa 3 lần & lưu trạng thái gửi. |
| `SCRUM-71` | Task | `EP-07` / `US-35` *(FE)* | `[FS] Hỗ trợ pre-fill URL params từ Chat (/booking?date=...) & giữ state qua Login` | Dev Fullstack | — | High | `frontend/src/pages/booking/BookingPage*`, `pages/auth/` | `SCRUM-43`, `SCRUM-23` | Mở link từ chat tự điền ngày/giờ/số khách/khu vực; khách chưa đăng nhập sau khi login quay lại giữ nguyên thông tin. |
| `SCRUM-72` | Story | `EP-07` / `US-34` | `[US-34] Guardrails Chatbot: Che PII, chống bịa tin, từ chối ngoài phạm vi/y tế & rate limit` | Dev AI | 5 | High | `backend/app/ai/pii_masker.py`, `guardrails.py`, `tests/ai/` | `SCRUM-47`, `SCRUM-65` | Tên/SĐT/email bị che trước Ollama; cross-check giá/món DB; pass ≥ 20 câu adversarial cùng BA. |
| `SCRUM-73` | Story | `EP-07` / `US-35` | `[US-35] Đặt bàn qua chat E2E (hỏi từng bước, GMT+7, link điền sẵn, không báo đặt nhầm)` | Dev AI | 8 | High | `backend/app/ai/booking_assistant.py`, `datetime_parser.py` | `SCRUM-41`, `SCRUM-46`, `SCRUM-71` | Chuẩn hóa GMT+7 xác nhận lại với khách; tra bàn trống read-only; tạo link pre-fill; không báo "đã đặt" trước xác nhận. |

---

### Sprint 7 – Tuần 8: Kiểm thử, UAT, Hardening & Phát hành `v1.0` *(Mốc M6 – 27 SP – Không nhận tính năng mới)*

- **Sprint Goal:** Hệ thống vượt qua kiểm thử hồi quy, bộ ≥ 30 câu test Chatbot và UAT với nhà hàng; không còn Bug Cao/Nghiêm trọng; phát hành Production `v1.0` và đóng dự án.
- **Luồng chạy song song (0% conflict):** BA + PM chạy kiểm thử hồi quy & UAT | Dev Fullstack fix Bug Core + Hardening + Release Production | Dev AI chạy auto-eval ≥ 30 câu (`ai_eval/`) + fix Bug AI + tối ưu latency Ollama.

| Mã đề xuất | Loại | Epic / US | Tiêu đề trên Jira (Summary) | Assignee | SP | Ưu tiên | Vùng file / Song song | Blocked By | Tiêu chí Done (AC) |
| :---: | :--- | :--- | :--- | :--- | :---: | :--- | :--- | :--- | :--- |
| `SCRUM-74` | Story | `EP-09` / `US-39` | `[US-39] Kiểm thử toàn bộ hệ thống MVP & hồi quy trên Staging (Postman + Test case)` | BA *(phối hợp PM)* | 5 | High | `docs/testcases/`, `[Staging]` | `SCRUM-66` | Chạy hết test case MVP & Postman; chạy lại hồi quy sau mỗi bản sửa; 0 Bug Cao/Nghiêm trọng mở. |
| `SCRUM-75` | Story | `EP-09` / `US-40` | `[US-40] Chạy & nghiệm thu bộ kiểm thử Chatbot ≥ 30 câu hỏi chuẩn & dẫn dụ` | Dev AI + BA | 3 | High | `ai_eval/`, `backend/app/ai/` | `SCRUM-72`, `SCRUM-73` | ≥ 30 câu (món, giá, chính sách, đặt bàn, ngoài phạm vi, dẫn dụ); câu sai log Bug và sửa đạt chuẩn; BA/PM duyệt. |
| `SCRUM-76` | Story | `EP-09` / `US-41` | `[US-41] Chạy UAT với đơn vị nhà hàng áp dụng theo kịch bản & tổng hợp kết quả` | BA *(PM điều phối)* | 5 | High | `docs/uat/` | `SCRUM-67`, `SCRUM-74` | Nhà hàng chạy thử đủ kịch bản; toàn bộ lỗi UAT ghi lên Jira; có biên bản báo cáo tổng hợp UAT. |
| `SCRUM-77` | Story | `EP-09` / `US-42` | `[US-42] Sửa lỗi UAT (Core/FE/DB) & Hardening bảo mật, hiệu năng, backup/restore` | Dev Fullstack | 5 | High | `frontend/`, `backend/app/core|api|services/` | `SCRUM-74`, `SCRUM-76` | Sửa hết Bug Cao/Nghiêm trọng; verify HTTPS, RBAC, không lưu số thẻ; index DB tối ưu; test backup & restore PG. |
| `SCRUM-78` | Task | `EP-09` / `US-42` *(AI)* | `[AI] Sửa Bug UAT phần Chatbot & tối ưu latency Ollama Production (keep_alive, log rotation)` | Dev AI | — | High | `backend/app/ai/`, `ollama/` | `SCRUM-75`, `SCRUM-76` | Sửa dứt điểm Bug AI từ UAT; giữ model trên RAM ổn định; log `chat_messages` dọn theo chính sách 90 ngày. |
| `SCRUM-79` | Story | `EP-09` / `US-43` | `[US-43] Viết Hướng dẫn sử dụng (Khách, Nhân viên, Quản lý + Chatbot AI)` | BA | 3 | High | `docs/user_guide/` | `SCRUM-66` | Hướng dẫn riêng cho 3 vai trò kèm ảnh màn hình & phần dùng Chatbot; nhà hàng xem và phản hồi. |
| `SCRUM-80` | Story | `EP-09` / `US-44` | `[US-44] Phát hành Production v1.0, GitHub Release & README/AI Runbook` | Dev Fullstack *(AI góp docs)* | 3 | High | `README.md`, `ollama/README.md`, `.github/` | `SCRUM-77`, `SCRUM-78` | Backup DB trước deploy; tag & release `v1.0` trên GitHub; README đủ hướng dẫn cài đặt & vận hành Ollama; CI xanh. |
| `SCRUM-81` | Story | `EP-09` / `US-45` | `[US-45] Demo, Báo cáo tổng kết, EVM cuối & Kết thúc dự án (Mốc M6)` | PM + BA *(2 Dev quay video)* | 3 | High | `docs/final_report/`, Slide & Video | `SCRUM-80` | Đủ slide + video demo, báo cáo tổng kết, bài học kinh nghiệm, đóng sổ rủi ro, EVM MS Project cuối; hoàn tất M6. |

---

## 4. Backlog Dự phòng trên Jira (`SCRUM-82` → `SCRUM-90` – 50 SP)

> **Quy tắc:** Tạo sẵn trên Jira ở trạng thái **Backlog** (chưa gán vào Sprint nào). Chỉ kéo vào Sprint khi Velocity thực tế vượt kế hoạch và **trước mốc Feature Freeze (hết Sprint 6)**.

| Mã đề xuất | Loại | Epic / US | Tiêu đề trên Jira (Summary) | Assignee đề xuất | SP | Ưu tiên | FR | Vùng file / Lưu ý conflict |
| :---: | :--- | :--- | :--- | :--- | :---: | :--- | :--- | :--- |
| `SCRUM-82` | Story | `EP-07` / `US-46` | `[US-46] Quản trị tri thức (FAQ/chính sách) & giám sát lịch sử hội thoại Chatbot` | Dev AI (API) + Dev Fullstack (UI) | 5 | Medium | FR-15 | `ai/admin_router.py` & `pages/admin/chatbot/` — ưu tiên kéo #1 nếu Dev AI xong sớm S6 |
| `SCRUM-83` | Story | `EP-06` / `US-47` | `[US-47] Báo cáo mở rộng (món đặt nhiều, công suất bàn, thống kê AI) & xuất Excel/PDF` | Dev Fullstack + Dev AI | 5 | Medium | FR-12 | `api/v1/reports.py` + hàm đọc `chat_messages` từ `ai/` |
| `SCRUM-84` | Story | `EP-04` / `US-48` | `[US-48] Hoàn cọc tự động qua cổng thanh toán & xử lý lỗi hoàn thủ công` | Dev Fullstack | 8 | Medium | FR-09 | `services/payment_service.py` |
| `SCRUM-85` | Story | `EP-02` / `US-49` | `[US-49] Cảnh báo đơn đã xác nhận bị ảnh hưởng khi món hết hoặc ngừng bán` | Dev Fullstack | 3 | Medium | FR-03 | `api/v1/menu.py`, `api/v1/staff.py` |
| `SCRUM-86` | Story | `EP-05` / `US-50` | `[US-50] Nhân viên tạo đơn đặt bàn thay khách gọi điện & gửi link cọc` | Dev Fullstack | 3 | Medium | FR-08 | `api/v1/staff.py`, `pages/staff/` |
| `SCRUM-87` | Story | `EP-08` / `US-51` | `[US-51] Đánh giá nhà hàng (1–5 sao, nhận xét, sửa trong 7 ngày, kiểm duyệt)` | Dev Fullstack | 5 | Low | FR-17 | `api/v1/reviews.py`, `pages/customer/` |
| `SCRUM-88` | Story | `EP-08` / `US-52` | `[US-52] Chatbot Tool Calling: Tra cứu đơn của khách, bàn trống, chính sách hoàn cọc` | Dev AI | 8 | Low | FR-18 | `backend/app/ai/tools.py` (chỉ đọc qua API có JWT) |
| `SCRUM-89` | Story | `EP-08` / `US-53` | `[US-53] Chatbot Tool Calling: Tạo giữ chỗ / Hủy đơn có xác nhận rõ ràng` | Dev AI + Dev Fullstack | 5 | Low | FR-18 | Yêu cầu review chéo bảo mật giữa 2 Dev trước khi merge |
| `SCRUM-90` | Story | `EP-08` / `US-54` | `[US-54] Đặt tiệc / sự kiện cho nhóm đông, báo giá & đặt cọc riêng` | Dev Fullstack | 8 | Low | FR-16 | `api/v1/events.py`, `pages/events/` |

---

## 5. Checklist Nhanh cho PM Khi Đẩy Backlog Lên Jira (`SCRUM`)

1. **Tạo Project:** Chọn template **Scrum** → đặt Key là **`SCRUM`** → cấu hình cột Board (`To Do`, `In Progress`, `In Review`, `In Testing`, `Done`).
2. **Tạo 10 Epics trước:** Tạo `SCRUM-1` → `SCRUM-10` theo Mục 2.
3. **Tạo 8 Sprint rỗng:** Đặt tên `Sprint 0 – Khởi động` đến `Sprint 7 – Phát hành v1.0` (mỗi sprint 1 tuần) và tạo 4 `Fix Version` (`v0.1`, `v0.2`, `v1.0-rc`, `v1.0`).
4. **Nhập Story & Task vào từng Sprint:** Copy từng dòng ở Mục 3 (gắn đúng `Epic Link`, `Sprint`, `Story Points`, `Priority`, `Assignee`, `Blocked By`, `Acceptance Criteria` và **Phạm vi file** vào phần Description để Dev biết ranh giới không đụng hàng).
5. **Bật tích hợp GitHub ↔ Jira:** Yêu cầu cả 2 Dev đặt tên nhánh `feature/SCRUM-xx-...` và commit bắt đầu bằng `SCRUM-xx: ...` để Jira tự động liên kết PR và trạng thái code.
