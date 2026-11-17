**TÀI LIỆU ĐẶC TẢ YÊU CẦU PHẦN MỀM**

# SOFTWARE REQUIREMENTS SPECIFICATION (SRS)

**Website đặt bàn và đặt món trước cho nhà hàng tích hợp chatbot AI**

| **Thông tin** | **Nội dung** |
| --- | --- |
| Tên dự án | Website đặt bàn và đặt món trước cho nhà hàng tích hợp chatbot AI |
| Mã đề tài | QL09-W |
| Lĩnh vực | Nhà hàng |
| Nền tảng | Web app (front-end + back-end API) |
| Phiên bản | 1.0 – bản nháp đầu tiên (Sprint 0) |
| Ngày lập | 17/11/2026 |

## 1. Giới thiệu hệ thống

### 1.1 Mục đích tài liệu

Tài liệu Đặc tả yêu cầu phần mềm (SRS) này mô tả yêu cầu của hệ thống **Website đặt bàn và đặt món trước cho nhà hàng tích hợp chatbot AI**. Tài liệu là cơ sở thống nhất giữa Product Owner/Business Analyst, nhóm phát triển, nhóm kiểm thử và đơn vị áp dụng về những gì hệ thống phải thực hiện.

Từ tài liệu này, nhóm xây dựng Product Backlog (epic, user story, tiêu chí chấp nhận), thiết kế kiến trúc – giao diện – cơ sở dữ liệu, test case và kịch bản nghiệm thu (UAT).

Tài liệu gồm hai phần: Phần 1 giới thiệu hệ thống; Phần 2 đặc tả các chức năng của hệ thống.

### 1.2 Phạm vi hệ thống

Hệ thống là ứng dụng web (front-end và back-end API) dành cho nhà hàng, quán ăn có phục vụ tại bàn. Khách hàng đặt bàn theo khung giờ, đặt món và đặt cọc trước, đồng thời được chatbot AI hỗ trợ tư vấn món và đặt bàn. Nhân viên và quản lý dùng hệ thống để sắp xếp bàn, chuẩn bị bếp và theo dõi hoạt động.

**Trong phạm vi:**

- Xem và quản lý thực đơn.
- Quản lý sơ đồ bàn; đặt bàn theo khung giờ và số khách.
- Đặt món trước và đặt cọc trực tuyến; hủy đặt chỗ và hoàn cọc theo chính sách.
- Quản lý đặt chỗ, thông báo và nhắc lịch, tổng hợp món chuẩn bị cho bếp.
- Báo cáo thống kê.
- Chatbot AI tư vấn món, giải đáp thông tin và hỗ trợ đặt bàn qua hội thoại.
- Mở rộng: đặt tiệc/sự kiện, đánh giá nhà hàng, chatbot thực hiện thao tác nghiệp vụ qua API (tool calling).

**Ngoài phạm vi:**

- Thanh toán toàn bộ hóa đơn tại bàn (POS) và xuất hóa đơn VAT.
- Quản lý kho, nguyên liệu, nhân sự và chấm công.
- Giao hàng và mang đi.
- Ứng dụng di động riêng (website được thiết kế responsive để dùng trên điện thoại).
- Huấn luyện hoặc tinh chỉnh mô hình AI; hệ thống chỉ sử dụng mô hình có sẵn.

### 1.3 Đối tượng đọc tài liệu

- **Product Owner/Business Analyst:** xác nhận yêu cầu và xây dựng backlog.
- **Project Manager/Scrum Master:** nắm phạm vi để lập kế hoạch và theo dõi tiến độ.
- **UI/UX Designer và Developer:** thiết kế và lập trình theo yêu cầu.
- **QA/QC:** xây dựng test case và bộ câu hỏi kiểm thử chatbot.
- **Giảng viên và đơn vị áp dụng:** đánh giá và nghiệm thu.

### 1.4 Định nghĩa, thuật ngữ và từ viết tắt

| **Thuật ngữ** | **Giải thích** |
| --- | --- |
| SRS | Software Requirements Specification – Đặc tả yêu cầu phần mềm. |
| MVP | Minimum Viable Product – tập chức năng tối thiểu bắt buộc của sản phẩm. |
| FR | Functional Requirement – yêu cầu chức năng (mã FR-xx). |
| Đơn đặt chỗ | Bản ghi gồm thông tin khách, bàn, khung giờ, số khách, món đặt trước (nếu có) và khoản cọc. |
| Khung giờ | Khoảng thời gian khách dự kiến dùng bàn, chia theo bước cố định. |
| Tiền cọc | Khoản thanh toán trước để giữ chỗ, được trừ vào hóa đơn khi khách dùng bữa. |
| Giữ chỗ tạm thời | Việc giữ bàn trong thời gian ngắn trong lúc khách hoàn tất đặt cọc. |
| Không đến (no-show) | Khách đã đặt chỗ nhưng không đến và không hủy theo quy định. |
| Chatbot AI | Trợ lý hội thoại sử dụng mô hình ngôn ngữ lớn (LLM) để tư vấn và hỗ trợ khách. |
| LLM | Large Language Model – mô hình ngôn ngữ lớn. |
| Ảo giác (hallucination) | Hiện tượng mô hình AI đưa ra thông tin sai hoặc không có căn cứ. |
| Tool calling | Cơ chế để mô hình AI yêu cầu hệ thống gọi các API nghiệp vụ đã được cho phép. |
| API / REST | Giao diện lập trình ứng dụng; kiểu API dùng giao thức HTTP để các thành phần trao đổi dữ liệu. |
| UAT | User Acceptance Test – kiểm thử nghiệm thu bởi đơn vị áp dụng. |

### 1.5 Bối cảnh và bài toán thực tế

Đơn vị áp dụng là các nhà hàng, quán ăn có phục vụ tại bàn. Khách hàng muốn đặt bàn theo khung giờ cụ thể, đặt món và đặt cọc trước; nhà hàng cần sắp xếp bàn hợp lý và chuẩn bị bếp từ trước.

Khi nhận đặt bàn thủ công (điện thoại, tin nhắn), nhà hàng thường gặp các vấn đề:

- Chỉ nhận đặt bàn khi có nhân viên trực, khách khó đặt ngoài giờ.
- Dễ trùng bàn hoặc sai khung giờ do ghi chép thủ công.
- Khách đặt nhưng không đến gây trống bàn và lãng phí chuẩn bị.
- Thực đơn thay đổi (món hết, đổi giá) khiến thông tin khách nhận được không còn đúng.
- Nhân viên phải trả lời lặp đi lặp lại các câu hỏi về món, giờ mở cửa và chính sách.

**Giá trị thực tế (đo được) mà hệ thống hướng tới:**

- Nhận đặt bàn 24/7.
- Giảm tỷ lệ khách đặt nhưng không đến nhờ đặt cọc.

Hai giá trị này được đo bằng các chỉ số trong báo cáo (FR-12): số đơn đặt ngoài giờ làm việc và tỷ lệ không đến trên số đơn đã xác nhận.

### 1.6 Tổng quan hệ thống

#### 1.6.1 Mục tiêu hệ thống

- Cho phép khách đặt bàn theo khung giờ và số khách mọi lúc, không phụ thuộc giờ làm việc của nhân viên.
- Cho phép đặt món và đặt cọc trước để nhà hàng chuẩn bị và giảm tình trạng khách không đến.
- Giúp quản lý sắp xếp bàn, quản lý đặt chỗ và chuẩn bị bếp chính xác, hạn chế trùng bàn.
- Hỗ trợ khách bằng chatbot AI tư vấn món và đặt bàn qua hội thoại, trả lời dựa trên dữ liệu thật của nhà hàng.
- Cung cấp số liệu để quản lý theo dõi hiệu quả vận hành.

#### 1.6.2 Tác nhân và người dùng

| **Tác nhân** | **Mô tả** | **Vai trò chính** |
| --- | --- | --- |
| Khách vãng lai | Người truy cập website nhưng chưa đăng nhập. | Xem thực đơn, trò chuyện với chatbot, đăng ký và đăng nhập. |
| Khách hàng | Người dùng đã có tài khoản. | Đặt bàn, đặt món trước, đặt cọc, hủy đặt chỗ, đặt tiệc, đánh giá, đặt bàn qua chatbot. |
| Nhân viên | Lễ tân, phục vụ, nhân viên bếp. | Xử lý đơn đặt chỗ, check-in, xem sơ đồ bàn, xem danh sách món chuẩn bị. |
| Quản lý | Chủ hoặc quản lý nhà hàng. | Quản lý thực đơn, sơ đồ bàn, chính sách cọc/hủy, tài khoản nhân viên, báo cáo và chatbot. |
| Chatbot AI | Thành phần phần mềm trò chuyện với khách. | Tư vấn món, giải đáp thông tin, tra cứu bàn trống, thao tác nghiệp vụ qua API (mở rộng). |
| Hệ thống ngoài | Cổng thanh toán, máy chủ Ollama (LLM có sẵn), dịch vụ SMS/email. | Xử lý thanh toán cọc, sinh câu trả lời cho chatbot, gửi thông báo. |

#### 1.6.3 Kiến trúc tổng quan và công nghệ

Hệ thống theo kiến trúc client–server: front-end giao tiếp với back-end qua REST API; back-end truy cập cơ sở dữ liệu, tích hợp cổng thanh toán, dịch vụ thông báo và máy chủ Ollama cho chatbot. Công nghệ chuẩn hóa của dự án như sau:

| **Thành phần** | **Công nghệ / giải pháp** | **Ghi chú** |
| --- | --- | --- |
| Front-end | ReactJS | Giao diện responsive cho khách, nhân viên, quản lý; có khung chat. |
| Back-end | Python FastAPI | REST API, tài liệu API bằng Swagger; phân quyền theo vai trò. |
| Cơ sở dữ liệu | PostgreSQL | Lưu thực đơn, bàn, đơn đặt chỗ, giao dịch cọc, hội thoại chatbot. |
| Chatbot AI | Mô hình mở chạy sẵn (Ollama) | Không huấn luyện mô hình; sử dụng mô hình Ollama có sẵn và chốt phiên bản model phù hợp ở mục 1.7. |
| Thanh toán | Cổng thanh toán trực tuyến | Dùng cho đặt cọc. |
| Thông báo | Dịch vụ SMS và email | Xác nhận, nhắc lịch. |
| Triển khai | Hosting/cloud, GitHub Actions (CI/CD) | Mã nguồn quản lý trên GitHub. |

### 1.7 Dịch vụ bên ngoài

Hệ thống phụ thuộc vào một số dịch vụ bên ngoài. Với mỗi dịch vụ, nhóm cần quyết định tự làm hay mua/thuê trong quá trình quản lý mua sắm của dự án.

| **Dịch vụ** | **Vai trò trong hệ thống** | **Nội dung cần quyết định** |
| --- | --- | --- |
| Mô hình Ollama cho chatbot | Sinh câu trả lời và hiểu yêu cầu của khách (FR-13, FR-14, FR-18). | Đánh giá và chọn mô hình Ollama có sẵn phù hợp (độ trễ, tài nguyên phần cứng, chất lượng tiếng Việt, bảo mật dữ liệu). |
| Cổng thanh toán | Thu tiền cọc và hoàn cọc (FR-07, FR-09). | Chọn nhà cung cấp, phí giao dịch, môi trường thử nghiệm (sandbox). |
| Hosting | Chạy front-end, back-end và cơ sở dữ liệu. | Chọn nền tảng cloud, cấu hình, chi phí và sao lưu. |
| SMS | Gửi xác nhận và nhắc lịch (FR-10). | Chọn nhà cung cấp và hạn mức tin nhắn. |

### 1.8 Ràng buộc, giả định và phụ thuộc

**Ràng buộc:**

- Sản phẩm là web app; công nghệ theo gợi ý ở mục 1.6.3.
- Chatbot dùng mô hình có sẵn, không huấn luyện mô hình.
- Dữ liệu cá nhân của khách không được gửi lên dịch vụ AI.
- Dự án thực hiện theo Scrum, mỗi sprint 1 tuần, quản lý backlog trên Jira (khóa SCRUM) và mã nguồn trên GitHub; nhóm gồm 4 thành viên (PM, BA, Dev Fullstack, Dev AI).

**Giả định:**

- Nhà hàng vận hành một cơ sở; tiền tệ VND; múi giờ GMT+7; giao diện tiếng Việt.
- Khách truy cập bằng trình duyệt trên điện thoại hoặc máy tính.
- Các tham số nghiệp vụ nêu ở Phần 2 (thời lượng lượt ăn, thời gian giữ chỗ, mức cọc, chính sách hoàn cọc, mốc nhắc lịch) là giá trị đề xuất, cấu hình được và cần được đơn vị áp dụng xác nhận.
- Dữ liệu thử nghiệm lấy từ dữ liệu thực tế của đơn vị và được ẩn danh.

**Phụ thuộc:**

- Dịch vụ LLM, cổng thanh toán, SMS và hosting sẵn sàng trong thời gian phát triển.
- Đơn vị áp dụng cung cấp thực đơn, sơ đồ bàn, giờ mở cửa và chính sách đặt cọc, hủy.

**Yêu cầu chất lượng chung:**

- Hoạt động liên tục 24/7, trừ thời gian bảo trì được thông báo.
- Các thao tác thường dùng (xem thực đơn, kiểm tra bàn trống) phản hồi trong vài giây ở điều kiện bình thường.
- Truyền tải qua HTTPS, mật khẩu lưu dạng băm, phân quyền theo vai trò, không lưu số thẻ thanh toán.

### 1.9 Rủi ro đặc thù và chức năng đáp ứng

Các rủi ro đặc thù của đề tài và chức năng được thiết kế để giảm thiểu:

| **Rủi ro** | **Tác động** | **Chức năng / biện pháp đáp ứng** |
| --- | --- | --- |
| Khách đặt nhưng không đến | Mất doanh thu, bàn trống không bán được, bếp chuẩn bị thừa. | Đặt cọc (FR-07); nhắc lịch (FR-10); chính sách hủy và hoàn cọc (FR-09); đánh dấu không đến và báo cáo tỷ lệ (FR-08, FR-12). |
| Trùng bàn | Hai nhóm khách cùng một bàn và khung giờ. | Kiểm tra bàn trống và giữ chỗ tạm thời có ràng buộc ở cơ sở dữ liệu (FR-05); sơ đồ bàn theo thời gian (FR-04); chặn đổi bàn trùng (FR-08). |
| Thực đơn thay đổi | Món hết hoặc đổi giá sau khi khách đã đặt. | Lưu giá chốt, cảnh báo đơn bị ảnh hưởng, hạn chót sửa món (FR-03, FR-06); chatbot chỉ dùng thực đơn hiện hành (FR-13). |
| Chatbot trả lời sai (ảo giác) hoặc lộ dữ liệu khi gửi lên dịch vụ AI | Khách nhận thông tin sai; rò rỉ dữ liệu cá nhân. | Chỉ trả lời dựa trên dữ liệu truy xuất; che dữ liệu cá nhân trước khi gửi; xác nhận và kiểm tra phía server với thao tác nghiệp vụ; nhật ký và giám sát (FR-13, FR-14, FR-15, FR-18). |

## 2. Chức năng hệ thống

Phần này đặc tả các chức năng của hệ thống: danh mục chức năng, phân quyền theo tác nhân, vòng đời đơn đặt chỗ và đặc tả chi tiết từng chức năng.

### 2.1 Tổng quan chức năng

#### 2.1.1 Quy ước

- **Mã chức năng:** FR-xx, dùng để truy vết sang epic và user story trên Jira.
- **Mức ưu tiên:** “MVP (bắt buộc)” là chức năng nghiệp vụ bắt buộc của đề tài; “MVP – hỗ trợ” là chức năng cần có để các chức năng MVP vận hành đủ nghiệp vụ; “Mở rộng” là chức năng bổ sung để nâng mức hoàn thiện.
- **Tham số cấu hình:** các con số trong tài liệu (thời lượng lượt ăn, thời gian giữ chỗ, mức cọc, mốc hoàn cọc, mốc nhắc lịch) là giá trị đề xuất, do Quản lý cấu hình và cần được đơn vị áp dụng xác nhận.

#### 2.1.2 Danh mục chức năng

| **Mã** | **Tên chức năng** | **Nhóm** | **Tác nhân chính** | **Ưu tiên** |
| --- | --- | --- | --- | --- |
| FR-01 | Xác thực và phân quyền | Tài khoản | Mọi người dùng | MVP – hỗ trợ |
| FR-02 | Xem và tìm kiếm thực đơn | Thực đơn | Khách | MVP (bắt buộc) |
| FR-03 | Quản lý thực đơn | Thực đơn | Quản lý | MVP (bắt buộc) |
| FR-04 | Quản lý sơ đồ bàn | Bàn | Quản lý | MVP (bắt buộc) |
| FR-05 | Đặt bàn theo khung giờ và số khách | Đặt bàn | Khách hàng | MVP (bắt buộc) |
| FR-06 | Đặt món trước | Đặt món và cọc | Khách hàng | MVP (bắt buộc) |
| FR-07 | Đặt cọc trực tuyến | Đặt món và cọc | Khách hàng | MVP (bắt buộc) |
| FR-08 | Quản lý đặt chỗ | Vận hành | Nhân viên, Quản lý | MVP (bắt buộc) |
| FR-09 | Hủy đặt chỗ và hoàn cọc | Vận hành | Khách hàng | MVP – hỗ trợ |
| FR-10 | Thông báo và nhắc lịch | Vận hành | Hệ thống | MVP – hỗ trợ |
| FR-11 | Tổng hợp món chuẩn bị cho bếp | Vận hành | Nhân viên | MVP – hỗ trợ |
| FR-12 | Báo cáo thống kê | Báo cáo | Quản lý | MVP (bắt buộc) |
| FR-13 | Chatbot tư vấn món và giải đáp thông tin | Chatbot AI | Khách | MVP (bắt buộc) |
| FR-14 | Chatbot hỗ trợ đặt bàn qua hội thoại | Chatbot AI | Khách hàng | MVP (bắt buộc) |
| FR-15 | Quản trị tri thức và giám sát chatbot | Chatbot AI | Quản lý | MVP – hỗ trợ |
| FR-16 | Đặt tiệc / sự kiện | Mở rộng | Khách hàng | Mở rộng |
| FR-17 | Đánh giá nhà hàng | Mở rộng | Khách hàng | Mở rộng |
| FR-18 | Chatbot thao tác nghiệp vụ qua API (tool calling) | Mở rộng | Khách hàng | Mở rộng |

#### 2.1.3 Ma trận chức năng – tác nhân

X: sử dụng đầy đủ; Xem: chỉ xem; Một phần: sử dụng một phần chức năng.

| **Mã** | **Chức năng** | **Khách vãng lai** | **Khách hàng** | **Nhân viên** | **Quản lý** |
| --- | --- | --- | --- | --- | --- |
| FR-01 | Xác thực và phân quyền | X | X | X | X |
| FR-02 | Xem và tìm kiếm thực đơn | X | X |  |  |
| FR-03 | Quản lý thực đơn |  |  | Một phần | X |
| FR-04 | Quản lý sơ đồ bàn |  |  | Xem | X |
| FR-05 | Đặt bàn theo khung giờ và số khách |  | X |  |  |
| FR-06 | Đặt món trước |  | X |  |  |
| FR-07 | Đặt cọc trực tuyến |  | X |  |  |
| FR-08 | Quản lý đặt chỗ |  |  | X | X |
| FR-09 | Hủy đặt chỗ và hoàn cọc |  | X | Một phần | X |
| FR-10 | Thông báo và nhắc lịch |  | Nhận | Nhận | Cấu hình |
| FR-11 | Tổng hợp món chuẩn bị cho bếp |  |  | X | X |
| FR-12 | Báo cáo thống kê |  |  |  | X |
| FR-13 | Chatbot tư vấn món và giải đáp | X | X |  |  |
| FR-14 | Chatbot hỗ trợ đặt bàn |  | X |  |  |
| FR-15 | Quản trị và giám sát chatbot |  |  |  | X |
| FR-16 | Đặt tiệc / sự kiện |  | X | Xem | X |
| FR-17 | Đánh giá nhà hàng |  | X |  | X |
| FR-18 | Chatbot thao tác qua API |  | X |  |  |

#### 2.1.4 Vòng đời đơn đặt chỗ

| **Trạng thái** | **Ý nghĩa** | **Có thể chuyển sang** | **Do ai kích hoạt** |
| --- | --- | --- | --- |
| Chờ cọc | Đơn đã tạo, bàn đang được giữ tạm thời. | Đã xác nhận; Hết hạn; Đã hủy | Hệ thống (cọc thành công, hết hạn); Khách (hủy) |
| Đã xác nhận | Đã cọc thành công, bàn được giữ cho khung giờ. | Đã đến; Đã hủy; Không đến | Nhân viên (check-in); Khách (hủy); Nhân viên (không đến) |
| Đã đến | Khách đã check-in và đang dùng bữa. | Hoàn thành | Nhân viên |
| Hoàn thành | Khách đã dùng bữa; đơn kết thúc, có thể đánh giá. | (trạng thái cuối) | Nhân viên |
| Đã hủy | Đơn bị hủy bởi khách hoặc nhà hàng. | (trạng thái cuối) | Khách; Nhân viên; Quản lý |
| Không đến | Khách không đến quá thời gian chờ. | (trạng thái cuối) | Nhân viên |
| Hết hạn | Hết thời gian giữ chỗ mà chưa cọc. | (trạng thái cuối) | Hệ thống |

### 2.2 Tài khoản và phân quyền

#### FR-01 – Xác thực và phân quyền

| **Mã chức năng** | FR-01 |
| --- | --- |
| **Tên chức năng** | **Xác thực và phân quyền** |
| **Mức ưu tiên** | MVP – hỗ trợ |
| **Tác nhân** | Khách vãng lai, Khách hàng, Nhân viên, Quản lý |
| **Mô tả** | Cho phép đăng ký, đăng nhập, đăng xuất, quản lý hồ sơ cá nhân và phân quyền truy cập chức năng theo vai trò. |
| **Tiền điều kiện** | Người dùng truy cập hệ thống qua trình duyệt. |
| **Luồng sự kiện chính** | 1. Người dùng chọn Đăng ký và nhập họ tên, số điện thoại, email, mật khẩu.<br>2. Hệ thống kiểm tra tính hợp lệ và tính duy nhất của email, số điện thoại.<br>3. Hệ thống tạo tài khoản vai trò Khách hàng và gửi email xác thực.<br>4. Người dùng đăng nhập bằng email hoặc số điện thoại cùng mật khẩu.<br>5. Hệ thống cấp phiên đăng nhập có thời hạn và điều hướng theo vai trò. |
| **Luồng thay thế / ngoại lệ** | - Email hoặc số điện thoại đã tồn tại: hệ thống báo lỗi và gợi ý đăng nhập.<br>- Sai mật khẩu quá 5 lần liên tiếp: khóa đăng nhập tạm thời 15 phút.<br>- Quên mật khẩu: hệ thống gửi liên kết đặt lại mật khẩu qua email.<br>- Truy cập chức năng ngoài quyền: hệ thống từ chối và ghi nhật ký. |
| **Hậu điều kiện** | Phiên đăng nhập hợp lệ được tạo; sự kiện đăng nhập được ghi nhật ký. |
| **Quy tắc nghiệp vụ** | - Có 3 vai trò đăng nhập: Khách hàng, Nhân viên, Quản lý. Tài khoản Nhân viên và Quản lý do Quản lý tạo.<br>- Khách vãng lai được xem thực đơn và trò chuyện với chatbot; đặt bàn và đặt cọc yêu cầu đăng nhập.<br>- Mật khẩu được lưu dưới dạng băm; toàn bộ truyền tải qua HTTPS. |

### 2.3 Thực đơn

#### FR-02 – Xem và tìm kiếm thực đơn

| **Mã chức năng** | FR-02 |
| --- | --- |
| **Tên chức năng** | **Xem và tìm kiếm thực đơn** |
| **Mức ưu tiên** | MVP (bắt buộc) |
| **Tác nhân** | Khách vãng lai, Khách hàng |
| **Mô tả** | Hiển thị thực đơn theo danh mục kèm hình ảnh, mô tả, giá và tình trạng; hỗ trợ tìm kiếm và lọc món. |
| **Tiền điều kiện** | Không yêu cầu đăng nhập. |
| **Luồng sự kiện chính** | 1. Người dùng chọn mục Thực đơn.<br>2. Hệ thống hiển thị danh mục và các món đang bán.<br>3. Người dùng tìm theo từ khóa hoặc lọc theo danh mục, khoảng giá, nhãn món (chay, cay, hải sản…).<br>4. Người dùng chọn một món để xem chi tiết.<br>5. Khách hàng đã có đơn đặt bàn có thể thêm món vào danh sách đặt trước (FR-06). |
| **Luồng thay thế / ngoại lệ** | - Không có kết quả phù hợp: hệ thống thông báo và gợi ý bỏ bớt bộ lọc.<br>- Món đang tạm hết: hiển thị nhãn “Tạm hết” và không cho thêm vào đơn. |
| **Hậu điều kiện** | Không thay đổi dữ liệu. |
| **Quy tắc nghiệp vụ** | - Chỉ hiển thị món ở trạng thái “Đang bán”; giá hiển thị bằng VND.<br>- Dữ liệu thực đơn là nguồn dữ liệu duy nhất để chatbot tư vấn (FR-13). |

#### FR-03 – Quản lý thực đơn

| **Mã chức năng** | FR-03 |
| --- | --- |
| **Tên chức năng** | **Quản lý thực đơn** |
| **Mức ưu tiên** | MVP (bắt buộc) |
| **Tác nhân** | Quản lý (Nhân viên chỉ cập nhật tình trạng còn/hết) |
| **Mô tả** | Thêm, sửa, ẩn danh mục và món ăn: tên, mô tả, giá, hình ảnh, nhãn món, trạng thái bán; cập nhật tình trạng còn/hết. |
| **Tiền điều kiện** | Người dùng đã đăng nhập với vai trò Quản lý. |
| **Luồng sự kiện chính** | 1. Quản lý vào mục Quản lý thực đơn.<br>2. Chọn Thêm món, Sửa món hoặc Ngừng bán.<br>3. Nhập hoặc chỉnh sửa thông tin món.<br>4. Hệ thống kiểm tra dữ liệu hợp lệ (tên không rỗng, giá lớn hơn 0, hình ảnh đúng định dạng).<br>5. Hệ thống lưu thay đổi, ghi lịch sử thay đổi giá và cập nhật thực đơn hiển thị cho khách và dữ liệu chatbot sử dụng. |
| **Luồng thay thế / ngoại lệ** | - Món đang nằm trong đơn đặt chỗ chưa hoàn thành: không cho xóa, chỉ cho chuyển sang “Ngừng bán”.<br>- Hình ảnh sai định dạng hoặc quá dung lượng: hệ thống báo lỗi, không lưu. |
| **Hậu điều kiện** | Thực đơn được cập nhật và có hiệu lực ngay cho các lượt đặt mới. |
| **Quy tắc nghiệp vụ** | - Xóa mềm; không xóa món đã từng phát sinh đơn.<br>- Đơn đã đặt giữ nguyên giá tại thời điểm khách xác nhận món (lưu giá chốt trong chi tiết đơn).<br>- Khi món chuyển sang “Hết” hoặc “Ngừng bán” mà đã có đơn đặt trước, hệ thống đánh dấu các đơn bị ảnh hưởng và cảnh báo nhân viên để liên hệ khách đổi món. |

### 2.4 Sơ đồ bàn và đặt bàn

#### FR-04 – Quản lý sơ đồ bàn

| **Mã chức năng** | FR-04 |
| --- | --- |
| **Tên chức năng** | **Quản lý sơ đồ bàn** |
| **Mức ưu tiên** | MVP (bắt buộc) |
| **Tác nhân** | Quản lý (Nhân viên chỉ xem) |
| **Mô tả** | Khai báo khu vực, bàn, sức chứa và vị trí bàn trên sơ đồ trực quan; xem trạng thái từng bàn theo ngày và khung giờ. |
| **Tiền điều kiện** | Người dùng đã đăng nhập với vai trò Quản lý hoặc Nhân viên. |
| **Luồng sự kiện chính** | 1. Quản lý vào mục Sơ đồ bàn.<br>2. Tạo khu vực (ví dụ: tầng 1, sân vườn, phòng riêng).<br>3. Thêm bàn: mã bàn, số chỗ, khu vực, vị trí trên sơ đồ (kéo thả).<br>4. Hệ thống kiểm tra và lưu sơ đồ.<br>5. Người dùng chọn ngày và giờ để xem trạng thái từng bàn: Trống, Đang giữ, Đã đặt, Đang phục vụ, Ngưng sử dụng. |
| **Luồng thay thế / ngoại lệ** | - Mã bàn trùng trong cùng cơ sở: hệ thống báo lỗi.<br>- Xóa hoặc ngưng bàn đang có đơn đặt trong tương lai: hệ thống chặn và yêu cầu chuyển bàn cho các đơn đó trước. |
| **Hậu điều kiện** | Sơ đồ bàn được lưu và được dùng làm cơ sở tính bàn trống cho FR-05. |
| **Quy tắc nghiệp vụ** | - Sức chứa của bàn tối thiểu là 1.<br>- Bàn ở trạng thái “Ngưng sử dụng” không nhận đơn đặt mới.<br>- Cho phép đánh dấu bàn có thể ghép để phục vụ nhóm đông khách. |

#### FR-05 – Đặt bàn theo khung giờ và số khách

| **Mã chức năng** | FR-05 |
| --- | --- |
| **Tên chức năng** | **Đặt bàn theo khung giờ và số khách** |
| **Mức ưu tiên** | MVP (bắt buộc) |
| **Tác nhân** | Khách hàng |
| **Mô tả** | Cho phép khách chọn ngày, giờ, số khách; hệ thống kiểm tra bàn trống phù hợp, giữ chỗ tạm thời và tạo đơn đặt chỗ. |
| **Tiền điều kiện** | Khách hàng đã đăng nhập; có ít nhất một bàn đang hoạt động. |
| **Luồng sự kiện chính** | 1. Khách chọn Đặt bàn.<br>2. Khách nhập ngày, giờ, số khách; có thể chọn thêm khu vực ưa thích và ghi chú (ví dụ: sinh nhật, ghế trẻ em).<br>3. Hệ thống tìm bàn trống có sức chứa phù hợp và không trùng khung giờ với đơn khác.<br>4. Hệ thống hiển thị các khung giờ còn bàn và bàn gợi ý.<br>5. Khách chọn khung giờ (và bàn, nếu nhà hàng cho phép chọn bàn).<br>6. Hệ thống tạo đơn ở trạng thái “Chờ cọc” và giữ bàn tạm thời trong thời gian giữ chỗ.<br>7. Khách tiếp tục đặt món trước (FR-06, tùy chọn) và đặt cọc (FR-07). |
| **Luồng thay thế / ngoại lệ** | - Không còn bàn phù hợp: hệ thống gợi ý khung giờ gần nhất hoặc khu vực khác.<br>- Hết thời gian giữ chỗ mà chưa cọc: đơn chuyển “Hết hạn”, bàn được giải phóng.<br>- Hai khách cùng chọn một bàn trong cùng khung giờ: khách giữ chỗ thành công trước được giữ, khách sau nhận thông báo và gợi ý lựa chọn khác.<br>- Số khách vượt sức chứa lớn nhất của một bàn: hệ thống đề xuất ghép bàn hoặc chuyển sang đặt tiệc/sự kiện (FR-16), nếu có. |
| **Hậu điều kiện** | Đơn đặt chỗ “Chờ cọc” được tạo kèm bàn đang giữ. |
| **Quy tắc nghiệp vụ** | - Khung giờ chia theo bước 30 phút; thời lượng mỗi lượt phục vụ mặc định 90 phút.<br>- Đặt trước tối thiểu 2 giờ và tối đa 30 ngày so với giờ hẹn.<br>- Thời gian giữ chỗ chờ cọc mặc định 10 phút.<br>- Tại mỗi thời điểm, một bàn chỉ thuộc về một đơn: việc kiểm tra và giữ bàn phải thực hiện trong một giao dịch và có ràng buộc ở cơ sở dữ liệu để chống trùng bàn.<br>- Mọi thời gian tính theo múi giờ Việt Nam (GMT+7). |

### 2.5 Đặt món trước và đặt cọc

#### FR-06 – Đặt món trước

| **Mã chức năng** | FR-06 |
| --- | --- |
| **Tên chức năng** | **Đặt món trước** |
| **Mức ưu tiên** | MVP (bắt buộc) |
| **Tác nhân** | Khách hàng |
| **Mô tả** | Cho phép khách chọn món và số lượng để nhà hàng chuẩn bị trước, gắn với đơn đặt bàn. |
| **Tiền điều kiện** | Khách hàng đã có đơn đặt chỗ ở trạng thái “Chờ cọc” hoặc “Đã xác nhận” còn trong hạn chỉnh sửa món. |
| **Luồng sự kiện chính** | 1. Khách chọn Đặt món trước trong đơn.<br>2. Khách duyệt thực đơn, thêm món, nhập số lượng và ghi chú cho từng món.<br>3. Hệ thống tính tạm tính theo giá hiện hành.<br>4. Khách có thể sửa số lượng hoặc xóa món.<br>5. Khách xác nhận danh sách món; hệ thống lưu giá chốt từng món, cập nhật tổng tiền và số tiền cọc yêu cầu. |
| **Luồng thay thế / ngoại lệ** | - Món hết trong lúc đặt: hệ thống thông báo và loại món khỏi danh sách.<br>- Khách không đặt món: đơn vẫn hợp lệ, tiền cọc tính theo quy tắc cọc khi không có món.<br>- Sửa món sau khi đã cọc: cho phép đến hạn chốt; nếu tổng tiền thay đổi, hệ thống tính lại mức cọc và xử lý phần chênh lệch (yêu cầu cọc bổ sung hoặc ghi nhận theo chính sách). |
| **Hậu điều kiện** | Đơn lưu danh sách món, giá chốt và tổng tiền; số tiền cọc yêu cầu được cập nhật. |
| **Quy tắc nghiệp vụ** | - Hạn chót chỉnh sửa hoặc hủy món mặc định là 2 giờ trước giờ hẹn.<br>- Số lượng mỗi món từ 1 đến 20 (có thể cấu hình).<br>- Chỉ cho đặt món đang bán; giá lấy từ thực đơn tại thời điểm xác nhận. |

#### FR-07 – Đặt cọc trực tuyến

| **Mã chức năng** | FR-07 |
| --- | --- |
| **Tên chức năng** | **Đặt cọc trực tuyến** |
| **Mức ưu tiên** | MVP (bắt buộc) |
| **Tác nhân** | Khách hàng; Cổng thanh toán (hệ thống ngoài) |
| **Mô tả** | Cho phép khách thanh toán tiền cọc qua cổng thanh toán để xác nhận đơn đặt chỗ. |
| **Tiền điều kiện** | Đơn ở trạng thái “Chờ cọc” và còn trong thời gian giữ chỗ. |
| **Luồng sự kiện chính** | 1. Hệ thống hiển thị tóm tắt đơn: bàn, giờ, số khách, món đặt trước, số tiền cọc, chính sách hủy và hoàn cọc.<br>2. Khách đồng ý điều khoản và chọn Thanh toán.<br>3. Hệ thống tạo giao dịch và chuyển khách sang cổng thanh toán.<br>4. Khách hoàn tất thanh toán trên cổng.<br>5. Cổng thanh toán gửi kết quả về hệ thống (callback/webhook); hệ thống xác thực chữ ký của kết quả.<br>6. Nếu thành công, đơn chuyển sang “Đã xác nhận”, hệ thống sinh mã đặt chỗ và gửi xác nhận cho khách (FR-10). |
| **Luồng thay thế / ngoại lệ** | - Thanh toán thất bại hoặc khách hủy: đơn giữ nguyên “Chờ cọc” đến hết thời gian giữ chỗ; khách có thể thử lại.<br>- Kết quả thanh toán thành công đến muộn sau khi đơn đã hết hạn: nếu bàn còn trống, hệ thống xác nhận đơn; nếu không, tự động hoàn cọc và thông báo cho khách.<br>- Kết quả thanh toán gửi trùng lặp: hệ thống xử lý một lần duy nhất (không ghi nhận cọc hai lần).<br>- Cổng thanh toán không phản hồi: hệ thống thông báo lỗi và không ghi nhận cọc. |
| **Hậu điều kiện** | Giao dịch cọc được lưu; đơn ở trạng thái “Đã xác nhận” khi cọc thành công. |
| **Quy tắc nghiệp vụ** | - Mức cọc do Quản lý cấu hình; đề xuất mặc định 30% giá trị món đặt trước, tối thiểu một mức cố định theo số khách khi không đặt món.<br>- Chỉ lưu mã giao dịch, số tiền, thời điểm, trạng thái; không lưu số thẻ hay thông tin thanh toán nhạy cảm.<br>- Khoản cọc được hiển thị trong chi tiết đơn để nhân viên trừ vào hóa đơn khi khách dùng bữa (hệ thống không xử lý thanh toán hóa đơn tại bàn). |

### 2.6 Vận hành đặt chỗ

#### FR-08 – Quản lý đặt chỗ

| **Mã chức năng** | FR-08 |
| --- | --- |
| **Tên chức năng** | **Quản lý đặt chỗ** |
| **Mức ưu tiên** | MVP (bắt buộc) |
| **Tác nhân** | Nhân viên, Quản lý |
| **Mô tả** | Theo dõi và xử lý các đơn đặt chỗ: xem danh sách, xác nhận, đổi bàn hoặc giờ, hủy, check-in, hoàn thành, đánh dấu không đến; tạo đơn thay khách khi khách đặt qua điện thoại. |
| **Tiền điều kiện** | Người dùng đã đăng nhập với vai trò Nhân viên hoặc Quản lý. |
| **Luồng sự kiện chính** | 1. Nhân viên vào mục Quản lý đặt chỗ.<br>2. Lọc đơn theo ngày, trạng thái, khu vực, mã đặt chỗ hoặc số điện thoại khách.<br>3. Chọn một đơn để xem chi tiết: khách, bàn, món đặt trước, khoản cọc, lịch sử thay đổi.<br>4. Thực hiện thao tác: đổi bàn/giờ, ghi chú, hủy, check-in khi khách đến, hoàn thành, đánh dấu không đến.<br>5. Hệ thống cập nhật trạng thái đơn, đồng bộ sơ đồ bàn và gửi thông báo cho khách khi cần (FR-10). |
| **Luồng thay thế / ngoại lệ** | - Đổi sang bàn hoặc giờ đã có đơn khác: hệ thống chặn thao tác.<br>- Hủy đơn đã cọc: áp dụng chính sách hoàn cọc (FR-09); chỉ Quản lý được xử lý ngoại lệ và phải ghi lý do.<br>- Thao tác trên đơn đã ở trạng thái cuối (Hoàn thành, Đã hủy, Không đến, Hết hạn): hệ thống không cho phép.<br>- Khách đặt qua điện thoại: nhân viên tạo đơn thay khách theo luồng FR-05 và gửi liên kết đặt cọc cho khách. |
| **Hậu điều kiện** | Trạng thái đơn và sơ đồ bàn được cập nhật; thao tác được ghi vào lịch sử. |
| **Quy tắc nghiệp vụ** | - Tuân theo vòng đời đơn ở mục 2.1.4.<br>- Mọi thay đổi ghi nhật ký: người thực hiện, thời điểm, giá trị cũ và mới.<br>- Quá giờ hẹn 15 phút mà chưa check-in: hệ thống nhắc nhân viên xử lý; nhân viên đánh dấu “Không đến” và đơn chuyển trạng thái tương ứng. |

#### FR-09 – Hủy đặt chỗ và hoàn cọc

| **Mã chức năng** | FR-09 |
| --- | --- |
| **Tên chức năng** | **Hủy đặt chỗ và hoàn cọc** |
| **Mức ưu tiên** | MVP – hỗ trợ |
| **Tác nhân** | Khách hàng; Nhân viên, Quản lý (xử lý ngoại lệ) |
| **Mô tả** | Cho phép khách hủy đơn và hoàn cọc theo chính sách hủy của nhà hàng. |
| **Tiền điều kiện** | Đơn ở trạng thái “Chờ cọc” hoặc “Đã xác nhận”, chưa đến giờ hẹn. |
| **Luồng sự kiện chính** | 1. Khách mở mục Đặt chỗ của tôi, chọn đơn và nhấn Hủy.<br>2. Hệ thống hiển thị mức hoàn cọc theo chính sách tại thời điểm hủy.<br>3. Khách xác nhận hủy.<br>4. Hệ thống chuyển đơn sang “Đã hủy”, giải phóng bàn và tạo yêu cầu hoàn cọc (nếu có).<br>5. Hệ thống gửi thông báo kết quả cho khách (FR-10). |
| **Luồng thay thế / ngoại lệ** | - Hủy trong khoảng thời gian không được hoàn cọc: hệ thống cảnh báo mất cọc trước khi khách xác nhận.<br>- Hoàn cọc qua cổng thanh toán lỗi: yêu cầu hoàn được đánh dấu “Chờ xử lý thủ công” để Quản lý xử lý. |
| **Hậu điều kiện** | Đơn “Đã hủy”; bàn được giải phóng; khoản hoàn cọc được ghi nhận. |
| **Quy tắc nghiệp vụ** | - Chính sách đề xuất: hủy trước giờ hẹn từ 24 giờ trở lên hoàn 100%; từ 2 đến dưới 24 giờ hoàn 50%; dưới 2 giờ không hoàn. Khách không đến không được hoàn cọc.<br>- Các mốc thời gian và tỷ lệ hoàn do Quản lý cấu hình và phải hiển thị cho khách trước khi cọc. |

#### FR-10 – Thông báo và nhắc lịch

| **Mã chức năng** | FR-10 |
| --- | --- |
| **Tên chức năng** | **Thông báo và nhắc lịch** |
| **Mức ưu tiên** | MVP – hỗ trợ |
| **Tác nhân** | Hệ thống; Khách hàng, Nhân viên (nhận thông báo); Quản lý (cấu hình mẫu) |
| **Mô tả** | Gửi xác nhận, nhắc lịch và thông báo thay đổi/hủy qua email và SMS; thông báo đơn mới cho nhân viên trong hệ thống. |
| **Tiền điều kiện** | Có sự kiện kích hoạt (đơn xác nhận, thay đổi, hủy, đến mốc nhắc lịch). |
| **Luồng sự kiện chính** | 1. Hệ thống phát hiện sự kiện cần thông báo.<br>2. Hệ thống chọn mẫu nội dung và kênh gửi phù hợp.<br>3. Hệ thống gửi qua dịch vụ email hoặc SMS.<br>4. Hệ thống ghi nhận kết quả gửi vào chi tiết đơn. |
| **Luồng thay thế / ngoại lệ** | - Gửi thất bại: thử lại tối đa 3 lần; nếu vẫn lỗi thì ghi nhật ký và hiển thị cảnh báo trong chi tiết đơn.<br>- Số điện thoại không hợp lệ: gửi bằng email. |
| **Hậu điều kiện** | Thông báo được gửi và trạng thái gửi được lưu. |
| **Quy tắc nghiệp vụ** | - Nhắc lịch mặc định trước 24 giờ và trước 2 giờ so với giờ hẹn (cấu hình được) nhằm giảm tình trạng khách không đến.<br>- SMS chỉ dùng cho sự kiện quan trọng (xác nhận, nhắc lịch, hủy) để kiểm soát chi phí.<br>- Nội dung gồm mã đặt chỗ, giờ, bàn, chính sách hủy và liên kết quản lý đơn. |

#### FR-11 – Tổng hợp món chuẩn bị cho bếp

| **Mã chức năng** | FR-11 |
| --- | --- |
| **Tên chức năng** | **Tổng hợp món chuẩn bị cho bếp** |
| **Mức ưu tiên** | MVP – hỗ trợ |
| **Tác nhân** | Nhân viên, Quản lý |
| **Mô tả** | Tổng hợp các món đặt trước theo ngày và khung giờ để bếp chuẩn bị. |
| **Tiền điều kiện** | Người dùng đã đăng nhập với vai trò Nhân viên hoặc Quản lý. |
| **Luồng sự kiện chính** | 1. Nhân viên chọn ngày và ca hoặc khung giờ cần xem.<br>2. Hệ thống tổng hợp các món và số lượng từ các đơn “Đã xác nhận”, kèm ghi chú của khách.<br>3. Nhân viên xem hoặc in danh sách.<br>4. Bếp có thể đánh dấu món đã chuẩn bị (tùy chọn). |
| **Luồng thay thế / ngoại lệ** | - Đơn bị hủy hoặc sửa món sau khi đã in: danh sách trên hệ thống cập nhật và nêu rõ thay đổi.<br>- Không có đơn trong khoảng đã chọn: hệ thống hiển thị danh sách trống. |
| **Hậu điều kiện** | Không thay đổi dữ liệu đơn (trừ trạng thái chuẩn bị món nếu có dùng). |
| **Quy tắc nghiệp vụ** | - Chỉ tính các đơn đã xác nhận (đã cọc).<br>- Danh sách được cập nhật theo dữ liệu mới nhất và có thể xuất PDF hoặc in. |

### 2.7 Báo cáo

#### FR-12 – Báo cáo thống kê

| **Mã chức năng** | FR-12 |
| --- | --- |
| **Tên chức năng** | **Báo cáo thống kê** |
| **Mức ưu tiên** | MVP (bắt buộc) |
| **Tác nhân** | Quản lý |
| **Mô tả** | Cung cấp báo cáo theo khoảng thời gian: số đơn theo trạng thái, tỷ lệ hủy và không đến, doanh thu cọc và hoàn cọc, món được đặt trước nhiều nhất, công suất sử dụng bàn theo khung giờ, số đơn đặt ngoài giờ làm việc và thống kê hoạt động chatbot. |
| **Tiền điều kiện** | Người dùng đã đăng nhập với vai trò Quản lý. |
| **Luồng sự kiện chính** | 1. Quản lý vào mục Báo cáo.<br>2. Chọn loại báo cáo, khoảng thời gian và bộ lọc (khu vực, trạng thái).<br>3. Hệ thống tổng hợp dữ liệu và hiển thị dưới dạng bảng và biểu đồ.<br>4. Quản lý xuất báo cáo ra Excel hoặc PDF. |
| **Luồng thay thế / ngoại lệ** | - Không có dữ liệu trong khoảng thời gian: hệ thống thông báo.<br>- Khoảng thời gian quá dài (trên 1 năm): hệ thống yêu cầu thu hẹp khoảng thời gian. |
| **Hậu điều kiện** | Không thay đổi dữ liệu. |
| **Quy tắc nghiệp vụ** | - Tỷ lệ không đến = số đơn không đến / số đơn đã xác nhận đã qua giờ hẹn.<br>- Số liệu lấy từ dữ liệu đơn và giao dịch thực tế; thời gian lọc theo GMT+7.<br>- Thống kê chatbot gồm số hội thoại, tỷ lệ chuyển cho nhân viên và tỷ lệ phản hồi “Không hữu ích”. |

### 2.8 Chatbot AI

#### FR-13 – Chatbot tư vấn món và giải đáp thông tin

| **Mã chức năng** | FR-13 |
| --- | --- |
| **Tên chức năng** | **Chatbot tư vấn món và giải đáp thông tin** |
| **Mức ưu tiên** | MVP (bắt buộc) |
| **Tác nhân** | Khách vãng lai, Khách hàng; Dịch vụ LLM (hệ thống ngoài) |
| **Mô tả** | Chatbot AI trả lời bằng tiếng Việt về món ăn, giờ mở cửa, địa chỉ, chính sách đặt cọc và hủy; gợi ý món theo ngân sách, số người, sở thích. |
| **Tiền điều kiện** | Khung chat có trên website; dịch vụ LLM hoạt động. |
| **Luồng sự kiện chính** | 1. Khách mở khung chat; chatbot chào và nêu rõ đây là trợ lý AI.<br>2. Khách đặt câu hỏi (ví dụ: gợi ý món cho 4 người, món chay, giờ mở cửa, chính sách hủy).<br>3. Hệ thống xác định yêu cầu và truy xuất dữ liệu liên quan từ thực đơn, chính sách và thông tin nhà hàng.<br>4. Chatbot soạn câu trả lời dựa trên dữ liệu truy xuất, kèm thẻ món (tên, giá, hình ảnh, nút Thêm vào đơn đặt trước).<br>5. Hệ thống lưu hội thoại và cho phép khách đánh giá câu trả lời là Hữu ích hoặc Không hữu ích. |
| **Luồng thay thế / ngoại lệ** | - Câu hỏi ngoài phạm vi nhà hàng: chatbot từ chối lịch sự và hướng khách về chủ đề phù hợp.<br>- Không có thông tin để trả lời: chatbot nói rõ chưa có thông tin và cung cấp cách liên hệ nhân viên.<br>- Dịch vụ LLM lỗi hoặc quá thời gian chờ: hệ thống thông báo chatbot tạm ngưng và hướng dẫn dùng thực đơn, biểu mẫu đặt bàn. |
| **Hậu điều kiện** | Hội thoại được lưu phục vụ giám sát (FR-15). |
| **Quy tắc nghiệp vụ** | - Tên món, giá, tình trạng và chính sách chỉ được lấy từ dữ liệu hệ thống tại thời điểm hỏi; chatbot không được tự suy đoán hay bịa thông tin (hạn chế ảo giác).<br>- Không gửi họ tên, số điện thoại, email của khách lên dịch vụ LLM; dữ liệu cá nhân được loại bỏ hoặc che trước khi gửi.<br>- Không đưa ra cam kết về dị ứng hoặc y tế; với câu hỏi này, chatbot hướng khách liên hệ nhân viên.<br>- Có giới hạn tần suất và độ dài tin nhắn để tránh lạm dụng. |

#### FR-14 – Chatbot hỗ trợ đặt bàn qua hội thoại

| **Mã chức năng** | FR-14 |
| --- | --- |
| **Tên chức năng** | **Chatbot hỗ trợ đặt bàn qua hội thoại** |
| **Mức ưu tiên** | MVP (bắt buộc) |
| **Tác nhân** | Khách hàng |
| **Mô tả** | Chatbot dẫn dắt khách cung cấp thông tin đặt bàn, tra cứu bàn trống và chuyển khách sang bước xác nhận và đặt cọc với thông tin đã điền sẵn. |
| **Tiền điều kiện** | Khung chat hoạt động; khách có thể đăng nhập trong quá trình hội thoại. |
| **Luồng sự kiện chính** | 1. Khách nêu nhu cầu đặt bàn trong khung chat.<br>2. Chatbot hỏi lần lượt các thông tin còn thiếu: ngày, giờ, số khách, khu vực (nếu có).<br>3. Chatbot tra cứu bàn trống qua chức năng kiểm tra bàn trống của FR-05 (chỉ đọc) và đề xuất khung giờ.<br>4. Khách chọn khung giờ.<br>5. Chatbot tạo liên kết đến biểu mẫu đặt bàn đã điền sẵn thông tin.<br>6. Khách xác nhận, chọn món (tùy chọn) và đặt cọc theo FR-05, FR-06, FR-07. |
| **Luồng thay thế / ngoại lệ** | - Khách chưa đăng nhập: chatbot yêu cầu đăng nhập và giữ lại thông tin đã nhập.<br>- Hết bàn ở khung giờ khách chọn: chatbot đề xuất khung giờ hoặc khu vực khác.<br>- Thông tin không hợp lệ (ngày đã qua, quá giới hạn đặt trước): chatbot nêu lý do và hỏi lại. |
| **Hậu điều kiện** | Khách nhận liên kết đặt bàn với thông tin điền sẵn; đơn chỉ được tạo khi khách xác nhận trên hệ thống. |
| **Quy tắc nghiệp vụ** | - Chatbot không được thông báo “đã đặt thành công” khi chưa có xác nhận từ hệ thống.<br>- Ngày giờ do khách nhập bằng ngôn ngữ tự nhiên được chuẩn hóa theo GMT+7 và được hiển thị lại để khách xác nhận.<br>- Dữ liệu bàn trống lấy từ cùng dịch vụ với FR-05 để đảm bảo nhất quán. |

#### FR-15 – Quản trị tri thức và giám sát chatbot

| **Mã chức năng** | FR-15 |
| --- | --- |
| **Tên chức năng** | **Quản trị tri thức và giám sát chatbot** |
| **Mức ưu tiên** | MVP – hỗ trợ |
| **Tác nhân** | Quản lý |
| **Mô tả** | Quản lý nội dung nghiệp vụ chatbot sử dụng (thông tin nhà hàng, chính sách, câu hỏi thường gặp), xem lịch sử hội thoại và theo dõi chất lượng trả lời. |
| **Tiền điều kiện** | Người dùng đã đăng nhập với vai trò Quản lý. |
| **Luồng sự kiện chính** | 1. Quản lý vào mục Quản trị chatbot.<br>2. Thêm hoặc sửa mục thông tin: giờ mở cửa, địa chỉ, chính sách đặt cọc và hủy, câu hỏi thường gặp.<br>3. Hệ thống lưu và chatbot sử dụng nội dung mới ngay.<br>4. Quản lý xem lịch sử hội thoại, lọc theo phản hồi “Không hữu ích” hoặc hội thoại cần chuyển nhân viên.<br>5. Quản lý đánh dấu câu trả lời sai và ghi chú để chỉnh lại nội dung hoặc kịch bản chatbot. |
| **Luồng thay thế / ngoại lệ** | - Nội dung để trống hoặc vượt giới hạn độ dài: hệ thống báo lỗi.<br>- Xóa mục đang được dùng: hệ thống yêu cầu xác nhận. |
| **Hậu điều kiện** | Tri thức chatbot và đánh dấu chất lượng được cập nhật. |
| **Quy tắc nghiệp vụ** | - Dữ liệu cá nhân trong lịch sử hội thoại được che khi hiển thị.<br>- Lịch sử hội thoại lưu tối đa 90 ngày (cấu hình được).<br>- Chỉ Quản lý được xem lịch sử hội thoại. |

### 2.9 Chức năng mở rộng

#### FR-16 – Đặt tiệc / sự kiện

| **Mã chức năng** | FR-16 |
| --- | --- |
| **Tên chức năng** | **Đặt tiệc / sự kiện** |
| **Mức ưu tiên** | Mở rộng |
| **Tác nhân** | Khách hàng; Quản lý (duyệt); Nhân viên (xem) |
| **Mô tả** | Cho phép khách gửi yêu cầu đặt tiệc hoặc sự kiện cho nhóm đông, nhiều bàn hoặc phòng riêng; Quản lý duyệt, báo giá và yêu cầu cọc. |
| **Tiền điều kiện** | Khách hàng đã đăng nhập. |
| **Luồng sự kiện chính** | 1. Khách chọn Đặt tiệc/sự kiện.<br>2. Khách nhập loại sự kiện, ngày giờ, số khách, ngân sách dự kiến và yêu cầu đặc biệt.<br>3. Hệ thống kiểm tra khả năng sắp xếp (ghép bàn hoặc phòng riêng) và tạo yêu cầu “Chờ duyệt”.<br>4. Quản lý xem yêu cầu, điều chỉnh, báo giá và đặt mức cọc.<br>5. Khách xác nhận báo giá và đặt cọc theo FR-07.<br>6. Hệ thống xác nhận đơn tiệc và giữ bàn tương ứng. |
| **Luồng thay thế / ngoại lệ** | - Không đủ chỗ: Quản lý từ chối hoặc đề xuất thời điểm khác.<br>- Khách không phản hồi báo giá trong thời hạn quy định: yêu cầu chuyển “Hết hạn”. |
| **Hậu điều kiện** | Đơn tiệc được xác nhận hoặc đóng với lý do rõ ràng. |
| **Quy tắc nghiệp vụ** | - Đơn tiệc dùng chung cơ chế giữ bàn và chống trùng bàn với FR-05.<br>- Mức cọc, thời hạn đặt tối thiểu và thời hạn báo giá do Quản lý cấu hình riêng cho đơn tiệc. |

#### FR-17 – Đánh giá nhà hàng

| **Mã chức năng** | FR-17 |
| --- | --- |
| **Tên chức năng** | **Đánh giá nhà hàng** |
| **Mức ưu tiên** | Mở rộng |
| **Tác nhân** | Khách hàng; Quản lý |
| **Mô tả** | Cho phép khách đánh giá trải nghiệm sau khi dùng bữa; Quản lý xem, phản hồi và kiểm duyệt. |
| **Tiền điều kiện** | Đơn của khách ở trạng thái “Hoàn thành”. |
| **Luồng sự kiện chính** | 1. Hệ thống mời khách đánh giá sau khi đơn hoàn thành.<br>2. Khách chấm điểm từ 1 đến 5 sao và nhập nhận xét.<br>3. Hệ thống lưu đánh giá (chờ duyệt nếu bật kiểm duyệt).<br>4. Đánh giá hợp lệ được hiển thị trên website; Quản lý có thể phản hồi hoặc ẩn đánh giá vi phạm. |
| **Luồng thay thế / ngoại lệ** | - Khách chưa có đơn hoàn thành: hệ thống không cho đánh giá.<br>- Khách đã đánh giá đơn này: chỉ cho phép sửa trong 7 ngày. |
| **Hậu điều kiện** | Đánh giá được lưu; điểm trung bình được cập nhật. |
| **Quy tắc nghiệp vụ** | - Mỗi đơn hoàn thành chỉ có một đánh giá.<br>- Nhận xét chứa từ ngữ không phù hợp bị chặn hoặc chuyển chờ duyệt. |

#### FR-18 – Chatbot thực hiện thao tác nghiệp vụ qua API (tool calling)

| **Mã chức năng** | FR-18 |
| --- | --- |
| **Tên chức năng** | **Chatbot thực hiện thao tác nghiệp vụ qua API (tool calling)** |
| **Mức ưu tiên** | Mở rộng |
| **Tác nhân** | Khách hàng; Chatbot AI |
| **Mô tả** | Chatbot gọi trực tiếp các API nghiệp vụ được cho phép thay cho việc chuyển khách sang biểu mẫu: tra cứu đơn của khách, tạo đơn giữ chỗ, thêm món, hủy đơn, tra cứu mức hoàn cọc. |
| **Tiền điều kiện** | Khách đã đăng nhập; danh mục công cụ (tool) của chatbot được cấu hình. |
| **Luồng sự kiện chính** | 1. Khách nêu yêu cầu thao tác bằng ngôn ngữ tự nhiên (ví dụ: “đặt bàn 4 người 19h thứ Sáu”, “hủy đơn của tôi”).<br>2. Mô hình xác định công cụ và tham số cần gọi.<br>3. Back-end kiểm tra quyền của khách và tính hợp lệ của tham số trước khi thực thi.<br>4. Với thao tác làm thay đổi dữ liệu, chatbot hiển thị tóm tắt và yêu cầu khách xác nhận rõ ràng.<br>5. Sau khi khách xác nhận, hệ thống gọi API nghiệp vụ (cùng quy tắc với FR-05 đến FR-09).<br>6. Chatbot thông báo kết quả thực tế do API trả về. |
| **Luồng thay thế / ngoại lệ** | - Thiếu tham số: chatbot hỏi thêm.<br>- API từ chối (hết bàn, ngoài quyền, vi phạm chính sách): chatbot nêu nguyên nhân.<br>- Khách không xác nhận: thao tác bị hủy, không thay đổi dữ liệu.<br>- Phát hiện gọi bất thường hoặc vượt giới hạn: hệ thống chặn và ghi nhật ký. |
| **Hậu điều kiện** | Dữ liệu chỉ thay đổi khi API thực thi thành công sau xác nhận của khách; mọi lần gọi được ghi nhật ký. |
| **Quy tắc nghiệp vụ** | - Chỉ cho gọi các công cụ nằm trong danh sách cho phép (whitelist) và chỉ trên dữ liệu của chính khách đang đăng nhập.<br>- Back-end không tin tham số do mô hình sinh ra mà luôn kiểm tra lại quyền và dữ liệu.<br>- Việc thanh toán cọc vẫn thực hiện trên trang của cổng thanh toán; chatbot chỉ gửi liên kết thanh toán.<br>- Nhật ký gồm người dùng, công cụ, tham số, kết quả và thời điểm. |
