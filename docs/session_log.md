# Session Log – Dự án `QL09-W` (`SCRUM`)

Tệp ghi nhận nhật ký các phiên làm việc (quyết định đã chốt, thay đổi tài liệu/code, việc đang dở và bước tiếp theo) để **PM**, **BA**, **Dev Fullstack**, **Dev AI** và AI Agent không bị mất ngữ cảnh giữa các phiên.

---

## Quy ước ghi nhận mỗi phiên
```markdown
## [YYYY-MM-DD HH:mm] – Phiên #<n> | Người tham gia / Vai trò: <PM / BA / Dev Fullstack / Dev AI / Agent>
- **Sprint / Jira Keys:** <Sprint x | SCRUM-xx>
- **Đã hoàn thành:**
  - ...
- **Quyết định kỹ thuật / Nghiệp vụ đã chốt:**
  - ...
- **Vấn đề / Blocker (nếu có):**
  - ...
- **Bước tiếp theo (Next Actions):**
  - ...
```

---

## [2026-11-17 19:47] – Phiên #1 | Vai trò: PM + AI Assistant
- **Sprint / Jira Keys:** `Sprint 0` | Khởi tạo toàn bộ khung tài liệu `SCRUM-1` → `SCRUM-90`
- **Đã hoàn thành:**
  - Phân tích `Software Requirements Specification (SRS)` v1.0 (`FR-01`→`FR-18`) và `Product Backlog` v1.0 (`EP-00`→`EP-09`, `US-01`→`US-54`, 202 SP MVP + 50 SP Dự phòng).
  - Chuyển đổi toàn bộ tài liệu sang định dạng Markdown thuần trong thư mục `docs/` và dọn bỏ các bản `.docx`.
  - Xây dựng `docs/roadmap.md` điều chỉnh theo đội hình 4 người (**PM**, **BA**, **Dev Fullstack**, **Dev AI**) và Tech Stack chuẩn (**ReactJS**, **Python FastAPI**, **PostgreSQL**, **Ollama**).
  - Xây dựng `docs/taskboard.md` chuẩn hóa 90 issue Jira (`SCRUM-1`→`SCRUM-90`) kèm phân vùng thư mục chạy song song 0% conflict.
  - Tạo `AGENTS.md`, `docs/session_log.md` và cập nhật `README.md`.
  - Hoàn thành Brainstorming (Phương án B) và bổ sung 3 tài liệu kỹ thuật cốt lõi:
    1. `docs/database_schema.md` (11 bảng Core + 3 bảng AI, `btree_gist` exclusion constraint chống trùng bàn).
    2. `docs/api_contract.md` (Hợp đồng REST API Core & `/api/v1/chat/*`, mã lỗi HTTP/nghiệp vụ, bridge `check_table_availability()`).
    3. `docs/ai_ollama_spec.md` (Khung chọn model Ollama `qwen2.5:7b`, pipeline 5 lớp Guardrails/PII/RAG/GMT+7 và bộ 35 câu test chuẩn `TC-AI-01`→`TC-AI-35`).
- **Quyết định kỹ thuật / Nghiệp vụ đã chốt:**
  - Jira Project Key chính thức: **`SCRUM`** (giữ nguyên mã đề tài `QL09-W`).
  - Tech Stack cố định: **ReactJS** (FE) · **Python FastAPI** (BE Core & AI Router) · **PostgreSQL** (DB) · **Ollama** (mô hình có sẵn, không fine-tune).
  - Phân tách cứng vùng sở hữu code giữa **Dev Fullstack** và **Dev AI** (khóa mount router & import model từ Sprint 1; chốt API Contract `/api/v1/chat/messages` từ Sprint 0).
- **Vấn đề / Blocker (nếu có):**
  - Chưa có source code ứng dụng; đang ở giai đoạn khởi động Sprint 0 (`M1`).
- **Bước tiếp theo (Next Actions):**
  - Đẩy Epics (`SCRUM-1`→`SCRUM-10`) và Sprint 0–1 từ `docs/taskboard.md` lên Jira.
  - Khởi tạo cấu trúc monorepo (`frontend/`, `backend/`, `ollama/`) ở Sprint 0 (`SCRUM-13`, `SCRUM-14`) bám sát `docs/database_schema.md`, `docs/api_contract.md` và `docs/ai_ollama_spec.md`.
