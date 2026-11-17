**TÀI LIỆU QUẢN LÝ DỰ ÁN**

# PRODUCT BACKLOG

**Website đặt bàn và đặt món trước cho nhà hàng tích hợp chatbot AI**

| **Thông tin** | **Nội dung** |
| --- | --- |
| Tên dự án | Website đặt bàn và đặt món trước cho nhà hàng tích hợp chatbot AI |
| Mã đề tài | QL09-W |
| Phiên bản | 1.0 – Product Backlog bản đầu (Sprint 0) |
| Ngày lập | 17/11/2026 |
| Đầu vào | Software Requirements Specification (SRS) v1.0; Kế hoạch 8 tuần |
| Công cụ quản lý | Jira (dự án Scrum, khóa SCRUM); GitHub; Microsoft Project |
| Phạm vi kế hoạch | 8 tuần: Sprint 0 (khởi động) đến Sprint 7 (phát hành và kết thúc dự án) |

## 1. Giới thiệu

### 1.1 Mục đích và đầu vào

Tài liệu này là Product Backlog của dự án **Website đặt bàn và đặt món trước cho nhà hàng tích hợp chatbot AI** và là kế hoạch 8 tuần từ Sprint 0 (khởi động) đến Sprint 7 (phát hành và kết thúc dự án). Mỗi tuần là một sprint và toàn bộ backlog, sprint, lỗi, yêu cầu thay đổi được quản lý trên Jira.

Tài liệu được xây dựng từ hai nguồn: (1) Software Requirements Specification (SRS) v1.0, gồm 18 chức năng FR-01 đến FR-18; (2) Kế hoạch 8 tuần, trong đó Tuần 1 (Sprint 0) đã có nội dung chi tiết. Nội dung các Sprint 1 đến Sprint 7 do nhóm đề xuất dựa trên SRS và cần được PO và nhóm xác nhận ở Sprint Planning.

Product Backlog bản này tương ứng sản phẩm D03 “Product Backlog bản đầu” của Sprint 0; PO tiếp tục làm mịn và sắp xếp lại sau mỗi sprint.

### 1.2 Product Goal

Khách đặt bàn theo khung giờ, đặt món và đặt cọc trước mọi lúc trên website, được chatbot AI tư vấn món và hỗ trợ đặt bàn; nhà hàng sắp xếp bàn, chuẩn bị bếp dễ dàng và giảm tình trạng khách đặt nhưng không đến.

Hai giá trị đo được: nhận đặt bàn 24/7; giảm tỷ lệ khách đặt nhưng không đến nhờ đặt cọc.

### 1.3 Nhóm Scrum và vai trò

| **Mã** | **Vị trí trong dự án** | **Vai trò Scrum** | **Phụ trách chính trong backlog** |
| --- | --- | --- | --- |
| PM | Project Manager – Scrum Master (PM/SM) | Scrum Master | Điều phối sprint và sự kiện Scrum, Jira (SCRUM), MS Project, burndown, Earned Value, quản lý thay đổi (CCB), rà soát DoD, báo cáo tiến độ với GV. |
| BA | Product Owner – Business Analyst (PO/BA) | Product Owner | Quản lý Product Backlog trên Jira, viết user story và tiêu chí chấp nhận, thiết kế wireframe luồng chính, dữ liệu thực tế đã ẩn danh, test nghiệp vụ & UAT, hướng dẫn sử dụng. |
| Dev Fullstack | Fullstack Developer (ReactJS + FastAPI + PostgreSQL) | Developer | Kiến trúc, ERD & migration, giao diện ReactJS (bao gồm Chat Widget), REST API FastAPI core, cổng thanh toán, bảo mật, GitHub, CI/CD và triển khai. |
| Dev AI | AI Engineer (Chatbot AI / Ollama) | Developer | Đánh giá và chọn mô hình Ollama có sẵn, FastAPI AI Router, RAG từ PostgreSQL, parser thời gian GMT+7, slot-filling đặt bàn qua chat, Guardrails/che PII và bộ kiểm thử chatbot. |

### 1.4 Quy ước

- **Mã hạng mục:** EP-xx là Epic, US-xx là user story trong tài liệu này. Khi tạo trên Jira, Jira tự cấp mã SCRUM-n; nhóm ghi mã US-xx vào tiêu đề hoặc nhãn để đối chiếu.
- **Story point (SP):** ước lượng tương đối theo dãy 1, 2, 3, 5, 8, 13; nhóm thống nhất bằng Planning Poker ở Sprint Planning.
- **Ưu tiên (MoSCoW):** Must là chức năng bắt buộc của MVP; Should cần có nhưng có thể lùi; Could là mở rộng. Trên Jira: Must = High, Should = Medium, Could = Low.
- **Velocity kế hoạch:** khoảng 27 đến 31 SP mỗi sprint. Sprint 1 dùng để đo velocity thực tế; từ Sprint 2 điều chỉnh theo velocity trung bình của các sprint đã hoàn thành.
- **Dự phòng:** hạng mục chưa xếp sprint, chỉ kéo vào khi velocity thực tế vượt kế hoạch và trước thời điểm đóng băng tính năng (hết Sprint 6).
- **Cột “Người làm chính”:** viết theo vai trò PM, BA, Dev Fullstack, Dev AI; người đứng đầu là người chịu trách nhiệm chính.

## 2. Kế hoạch 8 tuần: Sprint 0 đến Sprint 7

### 2.1 Lộ trình tổng quan

Mỗi tuần là một sprint. Sprint 0 dành cho khởi động; Sprint 1 đến Sprint 6 phát triển sản phẩm; Sprint 7 phát hành và kết thúc dự án, không nhận tính năng mới.

| **Tuần** | **Sprint** | **Sprint Goal** | **Epic chính** | **SP kế hoạch** | **SP tích lũy** | **Mốc** |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | Sprint 0 – Khởi động | Đề tài được duyệt, nhóm và công cụ sẵn sàng. | EP-00 | — | — | M1 |
| 2 | Sprint 1 | Nền tảng kỹ thuật chạy được; người dùng đăng ký, đăng nhập và được phân quyền. | EP-01 | 28 | 28 | M2 |
| 3 | Sprint 2 | Khách xem được thực đơn; Quản lý quản lý được thực đơn và sơ đồ bàn; chốt phương án LLM. | EP-02, EP-03, EP-07 | 29 | 57 |  |
| 4 | Sprint 3 | Khách đặt được bàn theo khung giờ mà không trùng bàn, và đặt món trước. | EP-03, EP-04 | 29 | 86 | M3 |
| 5 | Sprint 4 | Khách đặt cọc qua cổng sandbox, xem và hủy đặt chỗ; bản demo chạy trên staging. | EP-03, EP-04, EP-05, EP-09 | 27 | 113 |  |
| 6 | Sprint 5 | Nhân viên xử lý được đơn đặt chỗ; chatbot tư vấn món và chính sách trên dữ liệu thật. | EP-05, EP-07 | 31 | 144 | M4 |
| 7 | Sprint 6 | Chatbot an toàn và đặt bàn qua chat; có báo cáo, danh sách cho bếp, nhắc lịch; đóng băng tính năng. | EP-05, EP-06, EP-07 | 31 | 175 | M5 |
| 8 | Sprint 7 | Hệ thống qua UAT, phát hành v1.0 và kết thúc dự án. | EP-09 | 27 | 202 | M6 |

Tổng SP kế hoạch từ Sprint 1 đến Sprint 7 là 202 SP cho 45 user story; 9 user story dự phòng (50 SP) chưa xếp sprint.

### 2.2 Mốc kiểm tra và phát hành

Kế hoạch 8 tuần hiện nêu rõ mốc M1 ở Tuần 1 và việc lưu Baseline MS Project tại M2. Thời điểm M3 đến M6 dưới đây là đề xuất, cần đối chiếu với lịch chính thức của GV.

| **Mốc** | **Tuần** | **Nội dung kiểm tra** | **Bản phát hành GitHub** |
| --- | --- | --- | --- |
| M1 | 1 (Sprint 0) | Duyệt đề tài, đăng ký nhóm, tôn chỉ dự án; đã tạo GitHub, Jira, tệp MS Project. | — |
| M2 | 2 (Sprint 1) | Lưu Baseline MS Project (WBS, lịch, tài nguyên). | — |
| M3 | 4 (Sprint 3) | GitHub, Jira, MS Project: tiến độ Sprint 1 đến 3, backlog, burndown. | v0.1 |
| M4 | 6 (Sprint 5) | GitHub, Jira, MS Project: vận hành đặt chỗ, chatbot tư vấn; Earned Value. | v0.2 |
| M5 | 7 (Sprint 6) | Đủ phạm vi MVP, đóng băng tính năng; sẵn sàng UAT. | v1.0-rc |
| M6 | 8 (Sprint 7) | Phát hành, UAT hoàn tất, nộp sản phẩm và hồ sơ dự án. | v1.0 |

### 2.3 Sự kiện Scrum và phân công chủ trì

Sự kiện Scrum thực hiện trên Jira và được chia cho các thành viên để cùng chủ trì. Time-box dưới đây là đề xuất, quy đổi từ Scrum Guide 2020 cho sprint 1 tuần.

| **Sự kiện** | **Khi nào** | **Time-box** | **Người tham gia** | **Chủ trì** | **Đầu ra** |
| --- | --- | --- | --- | --- | --- |
| Sprint Planning | Đầu tuần | ≤ 2 giờ | Cả nhóm | PM điều phối; BA trình bày mục tiêu và backlog; Dev Fullstack & Dev AI góp ước lượng kỹ thuật | Sprint Goal, Sprint Backlog đã ước lượng SP |
| Daily Scrum | Hằng ngày | ≤ 15 phút | Cả nhóm | PM điều phối cùng BA, Dev Fullstack, Dev AI | Board Jira được cập nhật |
| Backlog Refinement | Giữa tuần | ≤ 1 giờ | PO và nhóm phát triển | BA | Story sẵn sàng cho sprint sau (đạt DoR) |
| Sprint Review | Cuối tuần | ≤ 1 giờ | Cả nhóm; GV hoặc đơn vị khi cần | BA và PM nghiệm thu; Dev Fullstack và Dev AI demo | Increment đã demo; Burndown, Velocity |
| Sprint Retrospective | Sau Review | ≤ 45 phút | Cả nhóm | PM điều phối; BA ghi hành động cải tiến | Hành động cải tiến (task trên Jira) |
| Biên bản, báo cáo tuần | Cuối tuần | — | — | PM / BA | Biên bản họp; báo cáo tuần gửi GV và đơn vị |

### 2.4 Kế hoạch chi tiết từng sprint

Nội dung Sprint 0 lấy từ Kế hoạch 8 tuần; nội dung Sprint 1 đến Sprint 7 là đề xuất. Danh sách hạng mục lấy từ Product Backlog ở Phần 3.

#### Sprint 0 – Khởi động (Tuần 1)

| **Thời gian** | Tuần 1 |
| --- | --- |
| **Sprint Goal** | Đề tài được duyệt, nhóm và công cụ sẵn sàng. |
| **Sự kiện Scrum** | Họp khởi động (kick-off); làm rõ Product Backlog lần đầu. |
| **Hạng mục trên Jira** | - Họp kick-off, thống nhất quy ước nhóm (PM)<br>- Phỏng vấn khảo sát đơn vị áp dụng; xác định nhóm người dùng và use case (BA)<br>- Viết SOW, tôn chỉ, phát biểu phạm vi; phân tích các bên liên quan (BA, PM)<br>- Lập Product Backlog bản đầu (BA)<br>- Tạo repo GitHub (private, main/develop/feature) và mời GV (Dev Fullstack)<br>- Tạo dự án Jira Scrum (khóa SCRUM) và mời GV; tạo tệp MS Project (PM)<br>- Lập tổ chức nhóm, RACI, kế hoạch truyền thông (PM); cài đặt Ollama và chốt Chat API Contract (Dev AI) |
| **Công việc phát triển** | Liên hệ đơn vị áp dụng thực tế, phỏng vấn khảo sát nghiệp vụ; xác định nhóm người dùng, use case; tạo repo GitHub, dự án Jira (Scrum), tệp Microsoft Project, kênh liên lạc, dự án Figma; mời GV vào GitHub và Jira. |
| **Sản phẩm quản lý dự án** | D01 SOW + Tôn chỉ dự án; D02 Phát biểu phạm vi; D03 Product Backlog bản đầu (tài liệu này); D14 Tổ chức nhóm, RACI, quy ước nhóm; D17 Kế hoạch truyền thông; D24 Phân tích các bên liên quan. |
| **Bài liên quan (CĐR)** | Bài 2, 6, 7, 9 |
| **Mốc kiểm tra / đánh giá** | M1 – Duyệt đề tài, đăng ký nhóm, tôn chỉ dự án; kiểm tra đã tạo GitHub, Jira, tệp MS Project. |
| **Chủ trì** | BA (khảo sát, D02–D03, D24); PM (kick-off, D01/D14/D17, Jira SCRUM, MS Project); Dev Fullstack (GitHub, skeleton); Dev AI (Ollama, Chat Contract). |
| **Kết quả cuối sprint** | Nhóm và công cụ sẵn sàng; Product Backlog bản đầu được làm rõ. |

#### Sprint 1 (Tuần 2)

| **Thời gian** | Tuần 2 |
| --- | --- |
| **Sprint Goal** | Nền tảng kỹ thuật chạy được; người dùng đăng ký, đăng nhập và được phân quyền. |
| **Sự kiện Scrum** | Sprint Planning lần đầu; dùng Sprint 1 để đo velocity thực tế và hiệu chỉnh kế hoạch. |
| **Hạng mục Product Backlog** | - US-01 – Khung dự án và kiến trúc (5 SP, Must)<br>- US-02 – ERD và migration CSDL (3 SP, Must)<br>- US-03 – Thiết kế Figma luồng chính (5 SP, Must)<br>- US-04 – Đăng ký tài khoản (3 SP, Must)<br>- US-05 – Đăng nhập và đăng xuất (3 SP, Must)<br>- US-06 – Quên mật khẩu (2 SP, Should)<br>- US-07 – Phân quyền và tài khoản nhân viên (5 SP, Must)<br>- US-08 – CI bằng GitHub Actions (2 SP, Should)<br>**Tổng: 28 SP** (Epic: EP-01) |
| **Công việc phát triển** | Dựng khung front-end, back-end, CSDL; ERD và migration; thiết kế Figma các màn hình chính; đăng ký, đăng nhập, phân quyền; CI. |
| **Sản phẩm quản lý dự án** | Kế hoạch tiến độ: WBS theo sản phẩm, lịch, Resource Sheet, lưu Baseline (PM); kế hoạch chất lượng, Definition of Done và sổ rủi ro bản đầu (PM, BA); chốt cổng thanh toán, hosting, SMS (Dev Fullstack, PM). |
| **Bài liên quan (CĐR)** | Bài 3, 5 (đề xuất) |
| **Mốc kiểm tra / đánh giá** | M2 – Lưu Baseline MS Project (đề xuất). |
| **Chủ trì** | PM điều phối; Dev Fullstack (nền tảng, FE/BE/DB); Dev AI (benchmark Ollama, DB chat); BA (wireframe). |
| **Kết quả cuối sprint** | Chạy được bản đăng ký, đăng nhập, phân quyền trên máy nhóm. |

#### Sprint 2 (Tuần 3)

| **Thời gian** | Tuần 3 |
| --- | --- |
| **Sprint Goal** | Khách xem được thực đơn; Quản lý quản lý được thực đơn và sơ đồ bàn; chốt phương án LLM. |
| **Sự kiện Scrum** | Sprint Review đầu tiên có demo giao diện thực đơn. |
| **Hạng mục Product Backlog** | - US-09 – Xem thực đơn (3 SP, Must)<br>- US-10 – Tìm kiếm và lọc món (3 SP, Should)<br>- US-11 – Quản lý danh mục và món (5 SP, Must)<br>- US-12 – Tình trạng món và lịch sử giá (3 SP, Must)<br>- US-13 – Quản lý khu vực và bàn (5 SP, Must)<br>- US-14 – Sơ đồ bàn trực quan (5 SP, Should)<br>- US-15 – Spike: chọn phương án LLM (3 SP, Must)<br>- US-16 – Dữ liệu mẫu thực tế đã ẩn danh (2 SP, Must)<br>**Tổng: 29 SP** (Epic: EP-02, EP-03, EP-07) |
| **Công việc phát triển** | Xem, tìm kiếm, quản lý thực đơn; tình trạng món và lịch sử giá; quản lý bàn và sơ đồ; nạp dữ liệu mẫu; spike phương án LLM. |
| **Sản phẩm quản lý dự án** | Kế hoạch chi phí và Earned Value lần đầu (PM); phê duyệt mô hình Ollama sử dụng (Dev AI, PM, BA); cập nhật sổ rủi ro (PM). |
| **Bài liên quan (CĐR)** | Bài 4, 9 (đề xuất) |
| **Mốc kiểm tra / đánh giá** | — |
| **Chủ trì** | Dev Fullstack (thực đơn, sơ đồ bàn, seed DB); Dev AI (spike Ollama, lõi RAG); BA (dữ liệu mẫu ẩn danh). |
| **Kết quả cuối sprint** | Thực đơn và sơ đồ bàn dùng được với dữ liệu thực tế đã ẩn danh. |

#### Sprint 3 (Tuần 4)

| **Thời gian** | Tuần 4 |
| --- | --- |
| **Sprint Goal** | Khách đặt được bàn theo khung giờ mà không trùng bàn, và đặt món trước. |
| **Sự kiện Scrum** | Sprint Review có demo luồng đặt bàn và giỏ món. |
| **Hạng mục Product Backlog** | - US-17 – Kiểm tra bàn trống (5 SP, Must)<br>- US-18 – Giữ chỗ tạm thời, chống trùng bàn (8 SP, Must)<br>- US-19 – Giao diện đặt bàn (5 SP, Must)<br>- US-20 – Cấu hình tham số nghiệp vụ (3 SP, Must)<br>- US-21 – Giỏ món đặt trước (5 SP, Must)<br>- US-22 – Chốt món, giá chốt, tính tiền cọc (3 SP, Must)<br>**Tổng: 29 SP** (Epic: EP-03, EP-04) |
| **Công việc phát triển** | Kiểm tra bàn trống; giữ chỗ tạm thời và chống trùng bàn; giao diện đặt bàn; tham số nghiệp vụ; giỏ món; giá chốt và tiền cọc. |
| **Sản phẩm quản lý dự án** | Cập nhật burndown, % hoàn thành, Earned Value và sổ rủi ro (PM); test case đặt bàn và kiểm thử đồng thời (BA, Dev Fullstack). |
| **Bài liên quan (CĐR)** | Bài 3, 8 (đề xuất) |
| **Mốc kiểm tra / đánh giá** | M3 (đề xuất) – kiểm tra GitHub, Jira, MS Project; gắn tag v0.1. |
| **Chủ trì** | Dev Fullstack (đặt bàn chống trùng, giỏ món, giao diện); Dev AI (parser GMT+7, slot-filling); BA (test case). |
| **Kết quả cuối sprint** | Tag v0.1: đặt bàn và đặt món trước chạy được (chưa cọc). |

#### Sprint 4 (Tuần 5)

| **Thời gian** | Tuần 5 |
| --- | --- |
| **Sprint Goal** | Khách đặt cọc qua cổng sandbox, xem và hủy đặt chỗ; bản demo chạy trên staging. |
| **Sự kiện Scrum** | Sprint Review có demo cọc thành công và hủy đơn trên staging. |
| **Hạng mục Product Backlog** | - US-23 – Xem trạng thái bàn theo ngày, giờ (3 SP, Must)<br>- US-24 – Đặt cọc: tạo giao dịch, chuyển cổng thanh toán (5 SP, Must)<br>- US-25 – Xử lý kết quả thanh toán (callback) (8 SP, Must)<br>- US-26 – Đặt chỗ của tôi, hủy và yêu cầu hoàn cọc (5 SP, Must)<br>- US-27 – Email xác nhận và hủy đặt chỗ (3 SP, Must)<br>- US-28 – Triển khai staging (3 SP, Should)<br>**Tổng: 27 SP** (Epic: EP-03, EP-04, EP-05, EP-09) |
| **Công việc phát triển** | Trạng thái bàn theo ngày, giờ; đặt cọc và xử lý callback; đặt chỗ của tôi, hủy, yêu cầu hoàn cọc; email xác nhận; staging. |
| **Sản phẩm quản lý dự án** | Quản lý thay đổi: ghi nhận CR, họp CCB khi có thay đổi và báo cáo tuần (PM); test case luồng thanh toán (BA). |
| **Bài liên quan (CĐR)** | Bài 7, 9 (đề xuất) |
| **Mốc kiểm tra / đánh giá** | — |
| **Chủ trì** | Dev Fullstack (cọc, callback, staging, giao diện); Dev AI (đưa Ollama & API chat lên staging); BA (kiểm thử thanh toán). |
| **Kết quả cuối sprint** | Luồng đặt bàn, đặt món, đặt cọc hoàn chỉnh trên staging. |

#### Sprint 5 (Tuần 6)

| **Thời gian** | Tuần 6 |
| --- | --- |
| **Sprint Goal** | Nhân viên xử lý được đơn đặt chỗ; chatbot tư vấn món và chính sách trên dữ liệu thật. |
| **Sự kiện Scrum** | Sprint Review có demo giao diện nhân viên và chatbot. |
| **Hạng mục Product Backlog** | - US-29 – Tích hợp LLM và khung chat (8 SP, Must)<br>- US-30 – Chatbot tư vấn món và chính sách (8 SP, Must)<br>- US-31 – Danh sách, lọc và chi tiết đơn đặt chỗ (5 SP, Must)<br>- US-32 – Check-in, hoàn thành, đánh dấu không đến (5 SP, Must)<br>- US-33 – Đổi bàn/giờ, hủy bởi nhân viên, nhật ký (5 SP, Must)<br>**Tổng: 31 SP** (Epic: EP-05, EP-07) |
| **Công việc phát triển** | Danh sách, chi tiết đơn; check-in, hoàn thành, không đến; đổi bàn/giờ, hủy, nhật ký; tích hợp LLM, khung chat; truy xuất thực đơn, chính sách. |
| **Sản phẩm quản lý dự án** | Cập nhật Earned Value và báo cáo CR (PM); kiểm soát chất lượng, báo cáo Bug và nội dung tri thức chatbot (BA). |
| **Bài liên quan (CĐR)** | Bài 4, 5 (đề xuất) |
| **Mốc kiểm tra / đánh giá** | M4 (đề xuất) – kiểm tra GitHub, Jira, MS Project; gắn tag v0.2. |
| **Chủ trì** | Dev AI (chatbot tư vấn trên dữ liệu thật); Dev Fullstack (vận hành đơn, UI khung chat); BA (nội dung & test). |
| **Kết quả cuối sprint** | Tag v0.2: vận hành đặt chỗ và chatbot tư vấn. |

#### Sprint 6 (Tuần 7)

| **Thời gian** | Tuần 7 |
| --- | --- |
| **Sprint Goal** | Chatbot an toàn và đặt bàn qua chat; có báo cáo, danh sách cho bếp, nhắc lịch; đóng băng tính năng. |
| **Sự kiện Scrum** | Sprint Review chốt phạm vi phát hành; cuối sprint đóng băng tính năng (không nhận story mới). |
| **Hạng mục Product Backlog** | - US-34 – Guardrails: chống bịa tin, bảo vệ dữ liệu (5 SP, Must)<br>- US-35 – Đặt bàn qua chat (8 SP, Must)<br>- US-36 – Báo cáo đơn, tỷ lệ hủy/không đến, doanh thu cọc (8 SP, Must)<br>- US-37 – Tổng hợp món chuẩn bị cho bếp (5 SP, Should)<br>- US-38 – Nhắc lịch trước 24 giờ và 2 giờ (5 SP, Should)<br>**Tổng: 31 SP** (Epic: EP-05, EP-06, EP-07) |
| **Công việc phát triển** | Guardrails chatbot; đặt bàn qua chat; báo cáo; tổng hợp món cho bếp; nhắc lịch. Hạng mục dự phòng chỉ kéo vào nếu velocity vượt kế hoạch. |
| **Sản phẩm quản lý dự án** | Cập nhật Earned Value, dự báo hoàn thành và rà soát rủi ro trước phát hành (PM); lập kế hoạch UAT với đơn vị (BA). |
| **Bài liên quan (CĐR)** | Bài 7, 8 (đề xuất) |
| **Mốc kiểm tra / đánh giá** | M5 (đề xuất) – kiểm tra GitHub, Jira, MS Project; gắn tag v1.0-rc. |
| **Chủ trì** | Dev AI (guardrails, đặt bàn qua chat); Dev Fullstack (báo cáo, bếp, nhắc lịch, giao diện); BA (kiểm thử chatbot). |
| **Kết quả cuối sprint** | Tag v1.0-rc: đủ phạm vi MVP. |

#### Sprint 7 – Phát hành và kết thúc dự án (Tuần 8)

| **Thời gian** | Tuần 8 |
| --- | --- |
| **Sprint Goal** | Hệ thống qua UAT, phát hành v1.0 và kết thúc dự án. |
| **Sự kiện Scrum** | Sprint Review kết hợp UAT với đơn vị áp dụng; Retrospective tổng kết dự án. |
| **Hạng mục Product Backlog** | - US-39 – Kiểm thử hệ thống và hồi quy (5 SP, Must)<br>- US-40 – Bộ câu hỏi kiểm thử chatbot (3 SP, Must)<br>- US-41 – UAT với đơn vị áp dụng (5 SP, Must)<br>- US-42 – Sửa lỗi UAT và hardening (5 SP, Must)<br>- US-43 – Hướng dẫn sử dụng (3 SP, Must)<br>- US-44 – Phát hành v1.0 (3 SP, Must)<br>- US-45 – Demo và kết thúc dự án (3 SP, Must)<br>**Tổng: 27 SP** (Epic: EP-09) |
| **Công việc phát triển** | Kiểm thử hệ thống, hồi quy và bộ câu hỏi chatbot; UAT; sửa lỗi và hardening; hướng dẫn sử dụng; phát hành v1.0; demo. |
| **Sản phẩm quản lý dự án** | Kết thúc dự án: báo cáo tổng kết, bài học kinh nghiệm, đóng sổ rủi ro, MS Project và Earned Value lần cuối (PM); slide và video demo (PM, BA, 2 Dev). |
| **Bài liên quan (CĐR)** | Bài 9 (đề xuất) |
| **Mốc kiểm tra / đánh giá** | M6 (đề xuất) – kiểm tra GitHub; gắn tag v1.0; nộp sản phẩm và tài liệu. |
| **Chủ trì** | BA (hồi quy, UAT, hướng dẫn); Dev Fullstack (hardening, phát hành v1.0); Dev AI (kiểm thử chatbot, tối ưu Ollama); PM (tổng kết, đóng dự án). |
| **Kết quả cuối sprint** | Tag v1.0 đã qua UAT; hồ sơ dự án đầy đủ. |

### 2.5 Phụ thuộc và quy tắc

- US-18 phụ thuộc US-13 và US-17 (dữ liệu bàn và truy vấn bàn trống).
- US-24 và US-25 cần tài khoản sandbox của cổng thanh toán; nhà cung cấp phải được chốt từ Sprint 1.
- US-29 đến US-35 phụ thuộc quyết định phương án LLM ở US-15 (Sprint 2), dữ liệu mẫu US-16 và tham số US-20.
- US-35 phụ thuộc US-17 và US-19 (kiểm tra bàn trống và form đặt bàn).
- US-37 và US-38 phụ thuộc đơn “Đã xác nhận” (US-25).
- UAT (US-41) cần staging (US-28) và đơn vị áp dụng sắp xếp lịch; PO chốt lịch UAT từ Sprint 5.
- Đóng băng tính năng hết Sprint 6: từ Sprint 7 chỉ sửa lỗi, kiểm thử, tài liệu và phát hành. Thay đổi yêu cầu đi qua quy trình CR (mục 4.2).

## 3. Product Backlog

### 3.1 Danh sách Epic

Epic được tạo trên Jira theo chức năng MVP và chatbot AI; mỗi story thuộc đúng một Epic.

| **Epic** | **Tên** | **FR liên quan** | **Nội dung** | **Số story / SP** | **Sprint** |
| --- | --- | --- | --- | --- | --- |
| EP-00 | Khởi động và quản lý dự án | — | Khảo sát đơn vị áp dụng, thiết lập GitHub, Jira, MS Project, Figma, tài liệu quản lý dự án ban đầu (Sprint 0). | — | S0 |
| EP-01 | Nền tảng và tài khoản | FR-01 | Khung kỹ thuật, ERD, thiết kế giao diện, đăng ký, đăng nhập, phân quyền, CI. | 8 story / 28 SP | S1 |
| EP-02 | Thực đơn | FR-02, FR-03 | Xem, tìm kiếm, quản lý thực đơn và tình trạng món; dữ liệu mẫu thực tế. | 6 story / 19 SP | S2, dự phòng |
| EP-03 | Sơ đồ bàn và đặt bàn | FR-04, FR-05 | Quản lý bàn, sơ đồ, kiểm tra bàn trống, giữ chỗ chống trùng bàn, tham số nghiệp vụ. | 7 story / 34 SP | S2, S3, S4 |
| EP-04 | Đặt món trước và đặt cọc | FR-06, FR-07, FR-09 | Giỏ món, giá chốt, tiền cọc, thanh toán cọc, hủy và hoàn cọc. | 6 story / 34 SP | S3, S4, dự phòng |
| EP-05 | Vận hành đặt chỗ | FR-08, FR-10, FR-11 | Xử lý đơn của nhân viên, thông báo, nhắc lịch, tổng hợp món cho bếp. | 7 story / 31 SP | S4, S5, S6, dự phòng |
| EP-06 | Báo cáo | FR-12 | Báo cáo đơn, tỷ lệ hủy và không đến, doanh thu cọc, thống kê. | 2 story / 13 SP | S6, dự phòng |
| EP-07 | Chatbot AI | FR-13, FR-14, FR-15 | Tích hợp LLM, tư vấn món, an toàn nội dung, đặt bàn qua chat, quản trị chatbot. | 6 story / 37 SP | S2, S5, S6, dự phòng |
| EP-08 | Mở rộng | FR-16, FR-17, FR-18 | Đặt tiệc/sự kiện, đánh giá nhà hàng, chatbot thao tác nghiệp vụ qua API (tool calling). | 4 story / 26 SP | Dự phòng |
| EP-09 | Kiểm thử, triển khai và phát hành | — | Staging, kiểm thử hệ thống, UAT, hướng dẫn sử dụng, phát hành v1.0, kết thúc dự án. | 8 story / 30 SP | S4, S7 |

### 3.2 Danh sách user story

Gồm 45 user story đã xếp Sprint 1 đến Sprint 7 (202 SP) và 9 user story dự phòng (50 SP, tô màu vàng). Sắp xếp theo thứ tự thực hiện.

| **ID** | **Epic** | **User story** | **Tiêu chí chấp nhận** | **Ưu tiên** | **SP** | **Sprint** | **FR** | **Người làm chính** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| US-01 | EP-01 | **Khung dự án và kiến trúc**<br>Với tư cách nhóm phát triển, tôi muốn có khung front-end, back-end và CSDL chạy được trên máy từng thành viên, để các story sau phát triển trên cùng một nền tảng. | - Repo có cấu trúc thư mục và README hướng dẫn cài đặt, chạy<br>- API /health trả về 200 và Swagger hiển thị<br>- Back-end kết nối được PostgreSQL; front-end gọi được API mẫu | Must | 5 | Sprint 1 | — | Dev Fullstack |
| US-02 | EP-01 | **ERD và migration CSDL**<br>Với tư cách Tech Lead, tôi muốn có ERD và migration cho các thực thể chính (người dùng, món, bàn, đơn đặt chỗ, giao dịch cọc), để dữ liệu nhất quán giữa các chức năng. | - ERD lưu trong repo<br>- Migration tạo đủ bảng trên CSDL trống<br>- Có ràng buộc ở CSDL hỗ trợ chống trùng bàn<br>- Có ít nhất 1 người review | Must | 3 | Sprint 1 | — | Dev Fullstack, Dev AI |
| US-03 | EP-01 | **Thiết kế Figma luồng chính**<br>Với tư cách UI/UX Designer, tôi muốn thiết kế trên Figma các màn hình chính, để cả nhóm và PO thống nhất giao diện trước khi lập trình. | - Có prototype: trang chủ, thực đơn, đặt bàn, đặt món, đặt cọc, khung chat, trang quản lý<br>- Giao diện dùng được trên điện thoại<br>- PO duyệt trước khi lập trình | Must | 5 | Sprint 1 | — | BA, PM |
| US-04 | EP-01 | **Đăng ký tài khoản**<br>Với tư cách khách vãng lai, tôi muốn đăng ký tài khoản bằng họ tên, số điện thoại, email và mật khẩu, để có thể đặt bàn. | - Email và số điện thoại không được trùng<br>- Mật khẩu lưu dạng băm<br>- Hệ thống gửi email xác thực<br>- Báo lỗi rõ khi nhập sai | Must | 3 | Sprint 1 | FR-01 | Dev Fullstack |
| US-05 | EP-01 | **Đăng nhập và đăng xuất**<br>Với tư cách người dùng, tôi muốn đăng nhập bằng email hoặc số điện thoại, để sử dụng chức năng đúng vai trò của mình. | - Đăng nhập đúng được điều hướng theo vai trò<br>- Sai mật khẩu quá 5 lần bị khóa 15 phút<br>- Phiên đăng nhập có thời hạn; đăng xuất hủy phiên | Must | 3 | Sprint 1 | FR-01 | Dev Fullstack |
| US-06 | EP-01 | **Quên mật khẩu**<br>Với tư cách người dùng, tôi muốn đặt lại mật khẩu qua email, để lấy lại quyền truy cập khi quên mật khẩu. | - Gửi liên kết đặt lại có thời hạn<br>- Liên kết chỉ dùng được một lần<br>- Mật khẩu mới được lưu dạng băm | Should | 2 | Sprint 1 | FR-01 | Dev Fullstack |
| US-07 | EP-01 | **Phân quyền và tài khoản nhân viên**<br>Với tư cách Quản lý, tôi muốn tạo tài khoản Nhân viên và phân quyền theo vai trò, để mỗi người chỉ dùng đúng chức năng của mình. | - Có 3 vai trò: Khách hàng, Nhân viên, Quản lý<br>- Truy cập ngoài quyền trả về 403 và ghi nhật ký<br>- Quản lý tạo, khóa, mở khóa tài khoản Nhân viên | Must | 5 | Sprint 1 | FR-01 | Dev Fullstack |
| US-08 | EP-01 | **CI bằng GitHub Actions**<br>Với tư cách Tech Lead, tôi muốn GitHub Actions tự động build và chạy test khi có pull request, để phát hiện lỗi sớm. | - Pull request kích hoạt build và test<br>- Kết quả hiển thị trên pull request<br>- Build lỗi thì không được merge vào main | Should | 2 | Sprint 1 | — | Dev Fullstack |
| US-09 | EP-02 | **Xem thực đơn**<br>Với tư cách khách, tôi muốn xem thực đơn theo danh mục kèm hình ảnh, mô tả và giá, để chọn món. | - Chỉ hiển thị món đang bán; món hết hiển thị “Tạm hết” và không thêm được vào đơn<br>- Giá hiển thị bằng VND<br>- Khách vãng lai xem được; hiển thị tốt trên điện thoại | Must | 3 | Sprint 2 | FR-02 | Dev Fullstack |
| US-10 | EP-02 | **Tìm kiếm và lọc món**<br>Với tư cách khách, tôi muốn tìm và lọc món, để nhanh chóng chọn được món phù hợp. | - Tìm theo từ khóa<br>- Lọc theo danh mục, khoảng giá, nhãn món<br>- Không có kết quả thì thông báo và gợi ý bỏ bớt bộ lọc | Should | 3 | Sprint 2 | FR-02 | Dev Fullstack |
| US-11 | EP-02 | **Quản lý danh mục và món**<br>Với tư cách Quản lý, tôi muốn thêm, sửa, ẩn danh mục và món, để thực đơn luôn đúng với thực tế. | - Kiểm tra tên không rỗng, giá lớn hơn 0, hình ảnh đúng định dạng và dung lượng<br>- Xóa mềm; không xóa món đã phát sinh đơn<br>- Thay đổi hiển thị ngay cho khách | Must | 5 | Sprint 2 | FR-03 | Dev Fullstack |
| US-12 | EP-02 | **Tình trạng món và lịch sử giá**<br>Với tư cách Quản lý hoặc Nhân viên, tôi muốn cập nhật món còn, hết, ngừng bán, để khách không đặt món không còn. | - Trạng thái: Đang bán, Tạm hết, Ngừng bán<br>- Lưu lịch sử đổi giá (người sửa, thời điểm, giá cũ, giá mới)<br>- Món Tạm hết hoặc Ngừng bán không thêm được vào đơn mới | Must | 3 | Sprint 2 | FR-03 | Dev Fullstack |
| US-13 | EP-03 | **Quản lý khu vực và bàn**<br>Với tư cách Quản lý, tôi muốn khai báo khu vực và bàn với sức chứa, để hệ thống biết nhà hàng có những bàn nào. | - Tạo khu vực; thêm, sửa bàn (mã, số chỗ, khu vực, có thể ghép)<br>- Mã bàn không trùng<br>- Bàn “Ngưng sử dụng” không nhận đơn mới<br>- Chặn xóa hoặc ngưng bàn có đơn trong tương lai (kiểm tra lại khi có đơn ở Sprint 3) | Must | 5 | Sprint 2 | FR-04 | Dev Fullstack |
| US-14 | EP-03 | **Sơ đồ bàn trực quan**<br>Với tư cách Quản lý, tôi muốn sắp xếp bàn lên sơ đồ bằng kéo thả, để nhân viên nhìn thấy bố trí thực tế. | - Kéo thả bàn trên sơ đồ theo khu vực và lưu vị trí<br>- Nhân viên chỉ xem, không sửa | Should | 5 | Sprint 2 | FR-04 | Dev Fullstack |
| US-15 | EP-07 | **Spike: chọn phương án LLM**<br>Với tư cách Dev AI, tôi muốn đánh giá các mô hình Ollama có sẵn, để chọn mô hình tối ưu về độ trễ, tài nguyên và tiếng Việt cho chatbot. | - Báo cáo so sánh ít nhất 2 mô hình Ollama có sẵn: tài nguyên RAM/VRAM, độ trễ, chất lượng tiếng Việt, bảo mật dữ liệu<br>- Chạy thử với ít nhất 10 câu hỏi mẫu<br>- BA và PM duyệt quyết định; chốt cấu hình model sử dụng | Must | 3 | Sprint 2 | FR-13 | Dev AI, PM, BA |
| US-16 | EP-02 | **Dữ liệu mẫu thực tế đã ẩn danh**<br>Với tư cách PO/BA, tôi muốn chuẩn bị dữ liệu thực đơn, sơ đồ bàn, chính sách từ đơn vị áp dụng, để thử nghiệm sát thực tế. | - Có tệp dữ liệu thực đơn và sơ đồ bàn được đơn vị xác nhận<br>- Có script nạp dữ liệu mẫu vào CSDL<br>- Không chứa dữ liệu cá nhân thật | Must | 2 | Sprint 2 | — | BA, Dev Fullstack |
| US-17 | EP-03 | **Kiểm tra bàn trống**<br>Với tư cách khách hàng, tôi muốn chọn ngày, giờ, số khách và xem khung giờ còn bàn, để đặt đúng nhu cầu. | - Trả về bàn có sức chứa phù hợp và không trùng khung giờ<br>- Khung giờ bước 30 phút, lượt phục vụ 90 phút<br>- Chỉ cho đặt trước từ 2 giờ đến 30 ngày<br>- Hết bàn thì gợi ý khung giờ hoặc khu vực gần nhất | Must | 5 | Sprint 3 | FR-05 | Dev Fullstack |
| US-18 | EP-03 | **Giữ chỗ tạm thời, chống trùng bàn**<br>Với tư cách khách hàng, tôi muốn hệ thống giữ bàn tạm thời sau khi chọn khung giờ, để không bị người khác đặt trùng. | - Tạo đơn “Chờ cọc” và giữ bàn 10 phút<br>- Hai yêu cầu đồng thời cùng bàn và khung giờ chỉ một yêu cầu thành công (có test đồng thời)<br>- Hết hạn tự chuyển “Hết hạn” và giải phóng bàn | Must | 8 | Sprint 3 | FR-05 | Dev Fullstack |
| US-19 | EP-03 | **Giao diện đặt bàn**<br>Với tư cách khách hàng, tôi muốn có form đặt bàn dễ dùng, để hoàn tất đặt bàn nhanh. | - Nhập ngày, giờ, số khách, khu vực, ghi chú<br>- Hiển thị khung giờ còn bàn và thông báo lỗi rõ ràng<br>- Hiển thị đếm ngược thời gian giữ chỗ; dùng được trên điện thoại | Must | 5 | Sprint 3 | FR-05 | Dev Fullstack |
| US-20 | EP-03 | **Cấu hình tham số nghiệp vụ**<br>Với tư cách Quản lý, tôi muốn cấu hình các tham số nghiệp vụ, để điều chỉnh chính sách mà không phải sửa mã nguồn. | - Cấu hình thời lượng lượt, thời gian giữ chỗ, mức cọc, mốc hoàn cọc, mốc nhắc lịch, hạn chót sửa món<br>- Giá trị mặc định theo SRS<br>- Thay đổi chỉ áp dụng cho đơn mới | Must | 3 | Sprint 3 | FR-05, FR-07, FR-09 | Dev Fullstack, BA |
| US-21 | EP-04 | **Giỏ món đặt trước**<br>Với tư cách khách hàng, tôi muốn chọn món và số lượng gắn với đơn đặt bàn, để nhà hàng chuẩn bị trước. | - Thêm, sửa, xóa món; số lượng từ 1 đến 20; ghi chú từng món<br>- Không thêm được món Tạm hết<br>- Chỉ sửa trước hạn chót 2 giờ so với giờ hẹn<br>- Tạm tính cập nhật ngay | Must | 5 | Sprint 3 | FR-06 | Dev Fullstack |
| US-22 | EP-04 | **Chốt món, giá chốt, tính tiền cọc**<br>Với tư cách khách hàng, tôi muốn xem tổng tiền và số tiền cọc trước khi thanh toán, để biết rõ khoản phải trả. | - Lưu giá chốt từng món tại thời điểm xác nhận<br>- Tính tổng và tiền cọc theo tham số cấu hình<br>- Không đặt món vẫn tạo được đơn, cọc theo quy tắc không có món<br>- Hiển thị chính sách hủy trước khi cọc | Must | 3 | Sprint 3 | FR-06 | Dev Fullstack |
| US-23 | EP-03 | **Xem trạng thái bàn theo ngày, giờ**<br>Với tư cách Nhân viên, tôi muốn xem trạng thái từng bàn theo ngày và giờ, để sắp xếp chỗ cho khách. | - Hiển thị: Trống, Đang giữ, Đã đặt, Đang phục vụ, Ngưng sử dụng<br>- Dữ liệu cập nhật theo đơn đặt chỗ<br>- Nhân viên và Quản lý xem được | Must | 3 | Sprint 4 | FR-04 | Dev Fullstack |
| US-24 | EP-04 | **Đặt cọc: tạo giao dịch, chuyển cổng thanh toán**<br>Với tư cách khách hàng, tôi muốn thanh toán tiền cọc trực tuyến, để xác nhận đơn đặt chỗ. | - Hiển thị tóm tắt đơn và chính sách hủy; khách đồng ý điều khoản<br>- Giao dịch lưu mã, số tiền, thời điểm, trạng thái<br>- Không lưu số thẻ; chuyển sang cổng sandbox | Must | 5 | Sprint 4 | FR-07 | Dev Fullstack |
| US-25 | EP-04 | **Xử lý kết quả thanh toán (callback)**<br>Với tư cách hệ thống, tôi muốn nhận và xử lý kết quả từ cổng thanh toán, để đơn được xác nhận chính xác. | - Xác thực chữ ký; thành công thì đơn thành “Đã xác nhận” và sinh mã đặt chỗ<br>- Kết quả gửi trùng không ghi nhận cọc hai lần<br>- Kết quả đến muộn sau khi hết hạn: xác nhận nếu bàn còn trống, nếu không thì đánh dấu hoàn cọc<br>- Thanh toán thất bại: đơn giữ “Chờ cọc”, khách thử lại được | Must | 8 | Sprint 4 | FR-07 | Dev Fullstack |
| US-26 | EP-04 | **Đặt chỗ của tôi, hủy và yêu cầu hoàn cọc**<br>Với tư cách khách hàng, tôi muốn xem và hủy đơn của mình, để chủ động khi thay đổi kế hoạch. | - Xem danh sách và chi tiết đơn của mình<br>- Hiển thị mức hoàn cọc theo chính sách trước khi xác nhận hủy<br>- Đơn chuyển “Đã hủy”, bàn được giải phóng<br>- Tạo yêu cầu hoàn cọc để Quản lý xử lý | Must | 5 | Sprint 4 | FR-09 | Dev Fullstack |
| US-27 | EP-05 | **Email xác nhận và hủy đặt chỗ**<br>Với tư cách khách hàng, tôi muốn nhận email khi đơn được xác nhận hoặc bị hủy, để có thông tin đặt chỗ. | - Gửi email khi cọc thành công và khi hủy<br>- Nội dung có mã đặt chỗ, giờ, bàn, chính sách hủy<br>- Gửi lỗi thì thử lại tối đa 3 lần và ghi trạng thái gửi | Must | 3 | Sprint 4 | FR-10 | Dev Fullstack |
| US-28 | EP-09 | **Triển khai staging**<br>Với tư cách nhóm, tôi muốn có môi trường staging, để demo và kiểm thử trên bản chạy thật. | - Truy cập được qua URL<br>- Tự động deploy từ nhánh develop<br>- Có sẵn dữ liệu mẫu; GV và PO truy cập được | Should | 3 | Sprint 4 | — | Dev Fullstack, Dev AI |
| US-29 | EP-07 | **Tích hợp LLM và khung chat**<br>Với tư cách khách, tôi muốn trò chuyện với chatbot ngay trên website, để được hỗ trợ nhanh. | - Khung chat có trên các trang chính; chatbot nêu rõ là trợ lý AI<br>- Gọi mô hình Ollama theo cấu hình đã chốt ở US-15<br>- Lưu hội thoại<br>- LLM lỗi hoặc quá thời gian: thông báo và hướng dẫn dùng thực đơn, form đặt bàn | Must | 8 | Sprint 5 | FR-13 | Dev AI, Dev Fullstack |
| US-30 | EP-07 | **Chatbot tư vấn món và chính sách**<br>Với tư cách khách, tôi muốn hỏi chatbot về món, giá, giờ mở cửa, chính sách cọc và hủy, để quyết định trước khi đặt. | - Trả lời dựa trên dữ liệu hệ thống tại thời điểm hỏi<br>- Gợi ý món theo ngân sách, số người, nhãn món; thẻ món có tên, giá, ảnh, nút Thêm vào đơn<br>- Không có dữ liệu thì nói chưa có thông tin và hướng dẫn liên hệ nhân viên | Must | 8 | Sprint 5 | FR-13 | Dev AI, Dev Fullstack, BA |
| US-31 | EP-05 | **Danh sách, lọc và chi tiết đơn đặt chỗ**<br>Với tư cách Nhân viên, tôi muốn xem và lọc các đơn đặt chỗ, để nắm tình hình từng ngày. | - Lọc theo ngày, trạng thái, khu vực, mã đơn, số điện thoại<br>- Chi tiết gồm khách, bàn, món, khoản cọc, lịch sử thay đổi<br>- Chỉ Nhân viên và Quản lý truy cập | Must | 5 | Sprint 5 | FR-08 | Dev Fullstack |
| US-32 | EP-05 | **Check-in, hoàn thành, đánh dấu không đến**<br>Với tư cách Nhân viên, tôi muốn cập nhật trạng thái đơn khi khách đến, dùng xong hoặc không đến, để dữ liệu phản ánh đúng thực tế. | - Check-in chuyển “Đã đến”; hoàn thành chuyển “Hoàn thành”<br>- Quá giờ hẹn 15 phút chưa check-in thì hệ thống nhắc<br>- Đánh dấu “Không đến”; không thao tác được trên đơn ở trạng thái cuối | Must | 5 | Sprint 5 | FR-08 | Dev Fullstack |
| US-33 | EP-05 | **Đổi bàn/giờ, hủy bởi nhân viên, nhật ký**<br>Với tư cách Nhân viên, tôi muốn đổi bàn, đổi giờ hoặc hủy đơn khi cần, để xử lý tình huống phát sinh. | - Chặn đổi sang bàn hoặc giờ trùng<br>- Hủy đơn đã cọc theo chính sách; ngoại lệ chỉ Quản lý, kèm lý do<br>- Ghi nhật ký người, thời điểm, giá trị cũ và mới; thông báo khách khi đổi | Must | 5 | Sprint 5 | FR-08 | Dev Fullstack |
| US-34 | EP-07 | **Guardrails: chống bịa tin, bảo vệ dữ liệu**<br>Với tư cách Quản lý, tôi muốn chatbot không bịa thông tin và không làm lộ dữ liệu cá nhân, để khách tin cậy. | - Họ tên, số điện thoại, email được loại bỏ hoặc che trước khi gửi lên LLM<br>- Câu hỏi ngoài phạm vi bị từ chối lịch sự; câu hỏi dị ứng, y tế hướng khách liên hệ nhân viên<br>- Giới hạn tần suất và độ dài tin nhắn<br>- QA kiểm thử ít nhất 20 câu hỏi, không có thông tin bịa | Must | 5 | Sprint 6 | FR-13 | Dev AI, BA |
| US-35 | EP-07 | **Đặt bàn qua chat**<br>Với tư cách khách hàng, tôi muốn đặt bàn qua trò chuyện với chatbot, để không phải tự điền form. | - Chatbot hỏi lần lượt ngày, giờ, số khách; tra cứu bàn trống (chỉ đọc) và đề xuất khung giờ<br>- Tạo liên kết đặt bàn điền sẵn thông tin; chưa đăng nhập thì yêu cầu đăng nhập và giữ thông tin<br>- Không báo “đã đặt” khi hệ thống chưa xác nhận; ngày giờ chuẩn hóa theo GMT+7 | Must | 8 | Sprint 6 | FR-14 | Dev AI, Dev Fullstack |
| US-36 | EP-06 | **Báo cáo đơn, tỷ lệ hủy/không đến, doanh thu cọc**<br>Với tư cách Quản lý, tôi muốn xem báo cáo theo khoảng thời gian, để đánh giá hiệu quả đặt cọc và vận hành. | - Số đơn theo trạng thái; tỷ lệ hủy; tỷ lệ không đến<br>- Doanh thu cọc, hoàn cọc; số đơn đặt ngoài giờ làm việc<br>- Hiển thị bảng và biểu đồ; chỉ Quản lý truy cập | Must | 8 | Sprint 6 | FR-12 | Dev Fullstack |
| US-37 | EP-05 | **Tổng hợp món chuẩn bị cho bếp**<br>Với tư cách Nhân viên bếp, tôi muốn xem tổng hợp món đặt trước theo ngày và khung giờ, để chuẩn bị nguyên liệu. | - Tổng hợp món, số lượng, ghi chú từ các đơn “Đã xác nhận”<br>- Cập nhật khi đơn hủy hoặc sửa món<br>- In hoặc xuất PDF | Should | 5 | Sprint 6 | FR-11 | Dev Fullstack |
| US-38 | EP-05 | **Nhắc lịch trước 24 giờ và 2 giờ**<br>Với tư cách Quản lý, tôi muốn hệ thống tự nhắc khách trước giờ hẹn, để giảm tình trạng khách không đến. | - Gửi nhắc trước 24 giờ và 2 giờ qua email (SMS cho nhắc 2 giờ nếu bật)<br>- Không gửi cho đơn đã hủy<br>- Ghi trạng thái gửi; gửi lỗi thử lại tối đa 3 lần | Should | 5 | Sprint 6 | FR-10 | Dev Fullstack |
| US-39 | EP-09 | **Kiểm thử hệ thống và hồi quy**<br>Với tư cách QA/QC, tôi muốn kiểm thử toàn bộ chức năng MVP trên staging, để đảm bảo chất lượng trước khi phát hành. | - Test case cho mọi story MVP được chạy; kiểm thử API bằng Postman<br>- Kiểm thử hồi quy sau mỗi lần sửa lỗi<br>- Không còn Bug mức Cao hoặc Nghiêm trọng đang mở | Must | 5 | Sprint 7 | — | BA, PM |
| US-40 | EP-09 | **Bộ câu hỏi kiểm thử chatbot**<br>Với tư cách QA/QC, tôi muốn có bộ câu hỏi kiểm thử chatbot, để đo độ chính xác và độ an toàn của câu trả lời. | - Ít nhất 30 câu hỏi: món, giá, chính sách, đặt bàn, ngoài phạm vi, cố tình dẫn dụ<br>- Đo tỷ lệ trả lời đúng; câu sai ghi thành Bug<br>- PO xác nhận kết quả | Must | 3 | Sprint 7 | FR-13, FR-14 | Dev AI, BA |
| US-41 | EP-09 | **UAT với đơn vị áp dụng**<br>Với tư cách PO/BA, tôi muốn đơn vị áp dụng chạy UAT theo kịch bản, để xác nhận hệ thống đáp ứng nghiệp vụ. | - Kịch bản UAT bám theo tiêu chí chấp nhận<br>- Đơn vị thực hiện và xác nhận kết quả<br>- Lỗi UAT ghi trên Jira; có báo cáo tổng hợp | Must | 5 | Sprint 7 | — | BA, PM |
| US-42 | EP-09 | **Sửa lỗi UAT và hardening**<br>Với tư cách Tech Lead, tôi muốn sửa lỗi từ UAT và rà soát bảo mật, hiệu năng, để hệ thống ổn định khi phát hành. | - Sửa hết Bug Cao và Nghiêm trọng<br>- Rà soát HTTPS, phân quyền, không lưu số thẻ<br>- Thao tác thường dùng phản hồi trong vài giây trên staging<br>- Kiểm tra sao lưu và khôi phục CSDL | Must | 5 | Sprint 7 | — | Dev Fullstack, Dev AI |
| US-43 | EP-09 | **Hướng dẫn sử dụng**<br>Với tư cách PO/BA, tôi muốn có hướng dẫn sử dụng, để khách, nhân viên và quản lý tự dùng được hệ thống. | - Hướng dẫn riêng cho Khách, Nhân viên, Quản lý, kèm ảnh chụp màn hình<br>- Có phần hướng dẫn dùng chatbot<br>- Đơn vị áp dụng xem và phản hồi | Must | 3 | Sprint 7 | — | BA |
| US-44 | EP-09 | **Phát hành v1.0**<br>Với tư cách Tech Lead, tôi muốn phát hành phiên bản v1.0, để bàn giao sản phẩm hoàn chỉnh. | - Triển khai production sau khi sao lưu<br>- Tag và release v1.0 trên GitHub kèm ghi chú phát hành<br>- README đủ hướng dẫn cài đặt và chạy demo; CI xanh | Must | 3 | Sprint 7 | — | Dev Fullstack, Dev AI |
| US-45 | EP-09 | **Demo và kết thúc dự án**<br>Với tư cách nhóm, tôi muốn hoàn tất demo và đóng dự án, để bàn giao đầy đủ cho GV và đơn vị áp dụng. | - Có slide và video demo<br>- Có báo cáo tổng kết, bài học kinh nghiệm; đóng sổ rủi ro<br>- MS Project và Earned Value cập nhật lần cuối; bàn giao tài liệu | Must | 3 | Sprint 7 | — | PM, BA |
| US-46 | EP-07 | **Quản trị tri thức và giám sát chatbot**<br>Với tư cách Quản lý, tôi muốn quản lý nội dung chatbot dùng và xem lịch sử hội thoại, để cải thiện chất lượng trả lời. | - Thêm, sửa câu hỏi thường gặp, chính sách, thông tin nhà hàng; chatbot dùng ngay<br>- Xem hội thoại, lọc “Không hữu ích”, đánh dấu câu trả lời sai<br>- Che dữ liệu cá nhân khi hiển thị; lưu tối đa 90 ngày | Should | 5 | Dự phòng | FR-15 | Dev AI, Dev Fullstack |
| US-47 | EP-06 | **Báo cáo mở rộng và xuất tệp**<br>Với tư cách Quản lý, tôi muốn xem thêm món đặt nhiều, công suất bàn, thống kê chatbot và xuất tệp, để phân tích sâu hơn. | - Món đặt trước nhiều nhất; công suất sử dụng bàn theo khung giờ; thống kê chatbot<br>- Xuất Excel hoặc PDF<br>- Khoảng thời gian trên 1 năm yêu cầu thu hẹp | Should | 5 | Dự phòng | FR-12 | Dev Fullstack, Dev AI |
| US-48 | EP-04 | **Hoàn cọc tự động và xử lý lỗi hoàn**<br>Với tư cách Quản lý, tôi muốn hệ thống hoàn cọc qua cổng theo chính sách, để giảm xử lý thủ công. | - Gọi cổng hoàn cọc theo mức chính sách; ghi giao dịch hoàn<br>- Lỗi hoàn chuyển “Chờ xử lý thủ công” để Quản lý xử lý<br>- Thông báo khách khi hoàn cọc | Should | 8 | Dự phòng | FR-09 | Dev Fullstack |
| US-49 | EP-02 | **Cảnh báo đơn bị ảnh hưởng khi món hết**<br>Với tư cách Nhân viên, tôi muốn được cảnh báo khi món đã đặt trước chuyển hết hoặc ngừng bán, để liên hệ khách đổi món. | - Đánh dấu các đơn “Đã xác nhận” có món bị ảnh hưởng<br>- Nhân viên thấy cảnh báo trong chi tiết đơn<br>- Ghi lịch sử việc đổi món | Should | 3 | Dự phòng | FR-03 | Dev Fullstack |
| US-50 | EP-05 | **Tạo đơn thay khách (đặt qua điện thoại)**<br>Với tư cách Nhân viên, tôi muốn tạo đơn thay khách gọi điện, để không bỏ sót khách đặt bàn qua điện thoại. | - Tạo đơn theo luồng đặt bàn, nhập thông tin khách<br>- Gửi liên kết đặt cọc cho khách<br>- Ghi nguồn đặt chỗ là điện thoại | Should | 3 | Dự phòng | FR-08 | Dev Fullstack |
| US-51 | EP-08 | **Đánh giá nhà hàng**<br>Với tư cách khách hàng, tôi muốn đánh giá sau khi dùng bữa, để chia sẻ trải nghiệm. | - Chấm 1 đến 5 sao và nhận xét cho đơn “Hoàn thành”; mỗi đơn một đánh giá<br>- Sửa được trong 7 ngày<br>- Quản lý phản hồi hoặc ẩn đánh giá vi phạm; hiển thị điểm trung bình | Could | 5 | Dự phòng | FR-17 | Dev Fullstack |
| US-52 | EP-08 | **Chatbot tool calling: tra cứu**<br>Với tư cách khách hàng, tôi muốn chatbot tra cứu thông tin đơn của tôi, để không phải mở nhiều trang. | - Chatbot gọi các công cụ chỉ đọc trong danh sách cho phép (đơn của chính khách, bàn trống, chính sách hoàn cọc)<br>- Back-end kiểm tra quyền và tham số<br>- Ghi nhật ký mọi lần gọi | Could | 8 | Dự phòng | FR-18 | Dev AI |
| US-53 | EP-08 | **Chatbot tool calling: thao tác có xác nhận**<br>Với tư cách khách hàng, tôi muốn chatbot tạo đơn giữ chỗ hoặc hủy đơn thay tôi sau khi tôi xác nhận, để hoàn tất ngay trong khung chat. | - Hiển thị tóm tắt và yêu cầu khách xác nhận rõ ràng trước khi thực hiện<br>- Thanh toán cọc vẫn qua cổng thanh toán, chatbot chỉ gửi liên kết<br>- Giới hạn số lần gọi; chặn và ghi nhật ký khi bất thường | Could | 5 | Dự phòng | FR-18 | Dev AI, Dev Fullstack |
| US-54 | EP-08 | **Đặt tiệc / sự kiện**<br>Với tư cách khách hàng, tôi muốn gửi yêu cầu đặt tiệc cho nhóm đông, để nhà hàng sắp xếp và báo giá. | - Form nhập loại sự kiện, ngày giờ, số khách, ngân sách, yêu cầu<br>- Quản lý duyệt, báo giá, đặt mức cọc; khách xác nhận và cọc<br>- Dùng chung cơ chế giữ bàn; báo giá quá hạn tự hết hạn | Could | 8 | Dự phòng | FR-16 | Dev Fullstack |

### 3.3 Ma trận truy vết FR – user story

Đối chiếu từng chức năng trong SRS với user story để bảo đảm backlog phủ hết yêu cầu.

| **FR** | **Chức năng** | **Ưu tiên SRS** | **User story** | **Sprint** | **Mức phủ** |
| --- | --- | --- | --- | --- | --- |
| FR-01 | Xác thực và phân quyền | MVP – hỗ trợ | US-04, US-05, US-06, US-07 | S1 | Đủ |
| FR-02 | Xem và tìm kiếm thực đơn | MVP | US-09, US-10 | S2 | Đủ |
| FR-03 | Quản lý thực đơn | MVP | US-11, US-12, US-49 | S2 | Một phần (phần còn lại dự phòng) |
| FR-04 | Quản lý sơ đồ bàn | MVP | US-13, US-14, US-23 | S2, S4 | Đủ |
| FR-05 | Đặt bàn theo khung giờ và số khách | MVP | US-17, US-18, US-19, US-20 | S3 | Đủ |
| FR-06 | Đặt món trước | MVP | US-21, US-22 | S3 | Đủ |
| FR-07 | Đặt cọc trực tuyến | MVP | US-20, US-24, US-25 | S3, S4 | Đủ |
| FR-08 | Quản lý đặt chỗ | MVP | US-31, US-32, US-33, US-50 | S5 | Một phần (phần còn lại dự phòng) |
| FR-09 | Hủy đặt chỗ và hoàn cọc | MVP – hỗ trợ | US-20, US-26, US-48 | S3, S4 | Một phần (phần còn lại dự phòng) |
| FR-10 | Thông báo và nhắc lịch | MVP – hỗ trợ | US-27, US-38 | S4, S6 | Đủ |
| FR-11 | Tổng hợp món chuẩn bị cho bếp | MVP – hỗ trợ | US-37 | S6 | Đủ |
| FR-12 | Báo cáo thống kê | MVP | US-36, US-47 | S6 | Một phần (phần còn lại dự phòng) |
| FR-13 | Chatbot tư vấn món và giải đáp thông tin | MVP | US-15, US-29, US-30, US-34, US-40 | S2, S5, S6, S7 | Đủ |
| FR-14 | Chatbot hỗ trợ đặt bàn qua hội thoại | MVP | US-35, US-40 | S6, S7 | Đủ |
| FR-15 | Quản trị tri thức và giám sát chatbot | MVP – hỗ trợ | US-46 | — | Dự phòng |
| FR-16 | Đặt tiệc / sự kiện | Mở rộng | US-54 | — | Dự phòng |
| FR-17 | Đánh giá nhà hàng | Mở rộng | US-51 | — | Dự phòng |
| FR-18 | Chatbot thao tác nghiệp vụ qua API | Mở rộng | US-52, US-53 | — | Dự phòng |

## 4. Quản lý Product Backlog trên Jira

### 4.1 Cấu hình dự án

| **Hạng mục** | **Cấu hình** |
| --- | --- |
| Loại dự án | Scrum (gói Free, tối đa 10 người dùng: nhóm 4 thành viên và GV); khóa dự án SCRUM. |
| Loại issue | Epic, Story, Task, Sub-task, Bug; yêu cầu thay đổi (CR) dùng issue type riêng hoặc Task gắn nhãn CR. |
| Epic | EP-00 đến EP-09 theo mục 3.1. |
| Trường dữ liệu | Story Points (1, 2, 3, 5, 8, 13); Priority (Must = High, Should = Medium, Could = Low); Labels (FR-xx, FE, BE, DB, AI, QA, PM, BA); Fix version (v0.1, v0.2, v1.0-rc, v1.0). Mô tả story gồm câu user story và tiêu chí chấp nhận. |
| Sprint | Sprint 0 đến Sprint 7, mỗi sprint 1 tuần, có Sprint Goal ở mục 2.4. |
| Board | To Do, In Progress, In Review, In Testing, Done; cập nhật hằng ngày. |
| Báo cáo | Burndown, Velocity, Sprint Report; xuất ảnh hoặc tệp sau mỗi sprint để nộp. |
| Tích hợp | Kết nối GitHub để liên kết nhánh, commit, pull request với issue. |
| Phân quyền thao tác | BA quản lý backlog; PM quản lý sprint; BA và PM phối hợp tạo và theo dõi Bug. |

### 4.2 Quy trình xử lý issue

- **Story:** To Do → In Progress (tạo nhánh feature từ develop) → In Review (mở pull request, có ít nhất 1 người review) → In Testing (QA chạy test case trên staging) → Done (đạt Definition of Done và PO chấp nhận ở Sprint Review).
- **Bug:** QA tạo issue loại Bug, liên kết story gốc, ghi mức độ và bước tái hiện; Dev sửa; QA kiểm tra lại rồi đóng.
- **Yêu cầu thay đổi (CR):** tạo issue CR, PO và PM đánh giá tác động đến phạm vi, tiến độ, chi phí; CCB (PO, PM, Tech Lead) quyết định; nếu chấp nhận thì thêm vào backlog và xếp sprint ở Planning kế tiếp.
- Không đổi Sprint Goal giữa sprint trừ khi PO quyết định; story chưa Done được ước lượng lại và đưa vào sprint sau.

### 4.3 Quy tắc làm việc với GitHub

- Repo private, mời GV; các nhánh main, develop, feature; mọi thay đổi vào main qua pull request có ít nhất 1 người review.
- Tên nhánh và commit ghi mã issue Jira, ví dụ feature/SCRUM-12-dat-ban và “SCRUM-12: kiểm tra bàn trống”.
- Gắn tag v0.1, v0.2, v1.0-rc, v1.0 theo mốc ở mục 2.2; README có hướng dẫn cài đặt và chạy demo.
- GitHub Actions chạy build và test cho mỗi pull request.

### 4.4 Definition of Ready và Definition of Done

**Definition of Ready** (story được đưa vào sprint khi):

- Có câu user story đủ vai trò, mục tiêu, lợi ích và tiêu chí chấp nhận kiểm thử được.
- Đã ước lượng SP; phụ thuộc đã xác định; giao diện (nếu có) đã có trên Figma.
- PO đã duyệt và story nằm trong khả năng của một sprint.

**Definition of Done** (story hoàn thành khi):

- Mã đã qua pull request có review và merge vào develop; build và test thành công.
- Test case của story đã chạy đạt trên staging; không còn Bug mức Cao hoặc Nghiêm trọng liên quan.
- API được ghi trên Swagger; dữ liệu và phân quyền đúng theo SRS.
- Demo được ở Sprint Review và PO chấp nhận; trạng thái Jira là Done.

### 4.5 Theo dõi tiến độ

- Cuối mỗi sprint, PM xuất Burndown và Velocity, cập nhật % hoàn thành và Earned Value trong MS Project.
- BA tổng hợp danh sách Bug và kiểm thử; PM tổng hợp CR và gửi báo cáo tuần cho GV và đơn vị áp dụng.
- PO dùng velocity thực tế để quyết định có kéo hạng mục dự phòng vào sprint hay không.
- Nếu velocity thực tế thấp hơn kế hoạch, ưu tiên giữ các story Must; story Should lùi sang sprint sau hoặc dự phòng.
