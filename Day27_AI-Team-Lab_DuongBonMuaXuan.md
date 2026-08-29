# Track 1 – Day 27 – AI Team Lab

- Nhóm: **Đường bốn mùa xuân**
- Dự án: **Nera** – trợ lý AI tìm nhà và đặt lịch xem nhà qua hội thoại (nerahome.space)
- Giai đoạn hiện tại: sản phẩm chạy thật ở mức demo, chưa có khách pilot thật. Mục tiêu 1–3 tháng: chạy xong 1 pilot với sàn môi giới ở Hà Nội và có số liệu thật về tỉ lệ chốt lịch, no-show.
- Ngày làm: 29/08/2026

| Thành viên | MSSV | Vai trò hiện tại |
|---|---|---|
| Vũ Thế Lực (trưởng nhóm) | 2A202602008 | AI Product / Founder – use case, value metric, làm việc với sàn |
| Hoàng Tuấn Hưng | 2A202601911 | AI Engineer – graph LangGraph, structured output, tích hợp tool |
| Đỗ Thanh Loan | 2A202601654 | Data / Backend – schema Postgres, crawl dữ liệu, khóa giao dịch |
| Nguyễn Thị Nam Phương | 2A202601720 | Eval / QA / UX – bộ kịch bản, chỉ số chất lượng, luồng người dùng |

---

## Trang 1 – Stakeholder Map & Strategy

### Danh sách stakeholder

| # | Stakeholder | Họ quan tâm điều gì |
|---|---|---|
| 1 | Giảng viên / mentor Track 1 | Nera có giải đúng bài toán không, value metric và unit economics có hợp lý không |
| 2 | Chủ sàn / quản lý kinh doanh của sàn môi giới pilot | Có tiết kiệm công điều phối và tăng số lịch xem nhà không, chi phí bao nhiêu |
| 3 | Nhân viên sale của sàn | AI có làm rối lịch của họ không, có bị thay việc không, thêm hay bớt thao tác |
| 4 | Điều phối viên của sàn | Hàng đợi HITL có xử lý được khi sale vắng không |
| 5 | Người mua nhà (khách cuối dùng chatbot) | Được trả lời nhanh buổi tối, đặt được lịch xem căn mình muốn |
| 6 | Nguồn dữ liệu tin đăng (batdongsan.com.vn và tương đương) | Điều kiện sử dụng dữ liệu, rủi ro pháp lý khi crawl |
| 7 | Nhà cung cấp LLM (OpenAI / OpenRouter) | Quan hệ thương mại, giá token, chính sách nội dung |
| 8 | Google Maps Platform | Chi phí Geocoding / Routes / Places khi bật tính năng lộ trình |

### Ma trận Influence × Interest

| | Interest thấp | Interest cao |
|---|---|---|
| **Influence cao** | **Cần thuyết phục (Blocker):** nhân viên sale · nhà cung cấp LLM · Google Maps | **Ủng hộ chủ chốt (Champion):** chủ sàn pilot · giảng viên / mentor |
| **Influence thấp** | **Ít liên quan (Bystander):** nguồn dữ liệu tin đăng | **Ủng hộ (Supporter):** người mua nhà · điều phối viên |

### Stance thực tế

| Stakeholder | Stance | Ghi chú |
|---|---|---|
| Giảng viên / mentor | Ủng hộ | Đã review qua các lab trước |
| Chủ sàn pilot | Trung lập | Cần thấy số liệu ROI trước khi cho pilot |
| Nhân viên sale | Chưa ủng hộ | Sợ mất kiểm soát lịch và thêm việc; đây là rủi ro cản trở lớn nhất |
| Người mua nhà | Ủng hộ | Với điều kiện phản hồi nhanh và đúng căn |
| Điều phối viên | Trung lập | Chưa rõ hàng đợi HITL có làm nặng việc của họ không |
| Nhà cung cấp LLM / Google Maps | Trung lập | Quan hệ thương mại thuần |

Nhân viên sale nằm ở ô Influence cao nhưng Interest thấp, khác với kỳ vọng "người dùng trực tiếp thì quan tâm cao". Lý do: họ chưa thấy cái lợi cho mình, mới thấy phần rủi ro. Đây là ưu tiên thuyết phục số 1.

### 4 stakeholder ưu tiên và hành động 1–2 tuần

**Đang ủng hộ, cần tận dụng**

1. Giảng viên / mentor. Quan tâm: value metric và bằng chứng. Hành động: Lực gửi link demo kèm report eval 150 kịch bản (containment 75%, pass rate 96.7%) trước thứ Sáu 05/09, xin 15 phút feedback về cách tính giá $0.99/request.
2. Người mua nhà. Quan tâm: trả lời nhanh, đúng căn. Hành động: Phương mời 10 người đang tìm mua nhà ở Hà Nội test chatbot trong tuần này, ghi lại 5 câu hỏi mà bot trả lời tệ nhất và tỉ lệ đặt được lịch.

**Chưa ủng hộ hoặc có rủi ro cản trở, cần thuyết phục**

3. Chủ sàn pilot. Quan tâm: tiết kiệm công, tăng lịch, chi phí. Hành động: Lực đặt lịch gặp 1 chủ sàn trong danh sách 5 sàn Hà Nội, mang bảng một trang so sánh chi phí điều phối thủ công hiện tại với $0.99/request, đề nghị pilot 2 tuần với 1 sale.
4. Nhân viên sale. Quan tâm: không bị rối lịch, không mất việc. Hành động: Lực và Hưng chạy buổi onboarding 30 phút với 2 sale của sàn pilot, chỉ rõ AI chỉ đề xuất khung giờ còn sale bấm duyệt cuối, đo thời gian sale tiết kiệm được mỗi ngày trong tuần đầu.

---

## Trang 2 – Pitch & RACI

### Pitch (nói với chủ sàn / quản lý kinh doanh của sàn pilot)

**Kết luận:** Cho Nera chạy pilot 2 tuần với 1 sale và khoảng 20 căn hộ mẫu, tính phí $0.99 cho mỗi yêu cầu xem nhà hợp lệ.

**Lý do:**

1. Việc hẹn khách xem nhà buổi tối 20:00–23:00 đang điều phối thủ công qua chat, gây trùng lịch và bỏ lỡ khách đúng lúc khách rảnh xem tin.
2. Nera trả lời 24/7, tự kiểm tra lịch trống của sale và trạng thái căn, đề xuất khung giờ, giữ căn tạm 15 phút, sale chỉ cần bấm duyệt.
3. Chi phí mỗi yêu cầu khoảng $0.21, bán $0.99, rẻ hơn một giờ công điều phối viên.

**Bằng chứng:** eval 150 kịch bản với tỉ lệ containment 75%, cao hơn ngưỡng hòa vốn 40,6%; pass rate 96,7%; bản chạy thật tại nerahome.space có thể demo trực tiếp.

**Small ask:** cho mượn 1 sale, danh sách 20 căn và 2 tuần. Chỉ số đánh giá: ít nhất 15 yêu cầu xem nhà hợp lệ, tỉ lệ no-show dưới 20%.

### Phản biện có khả năng xảy ra nhất

"Sale của tôi không tin AI, mà khách quen thì gọi điện chứ không chat."

**Trả lời:** Giữ nguyên kênh gọi điện. Nera chỉ nhận nhánh chat và web vào buổi tối khi sale offline. Có buổi onboarding 30 phút cho sale; tuần 1 sale review toàn bộ đề xuất của AI, tuần 2 chỉ review các ca ngoại lệ. Nếu sau 2 tuần sale thấy tệ hơn cách cũ thì dừng, không mất phí thiết lập.

### RACI Matrix (các việc chính trong 1–2 tháng pilot)

| Công việc | Lực (Product) | Hưng (Engineer) | Loan (Data) | Phương (Eval/QA) | Chủ sàn / Sale |
|---|---|---|---|---|---|
| Chốt phạm vi pilot và KPI | A | C | I | C | C |
| Chuẩn bị dữ liệu 20 căn và lịch sale | C | I | R | I | A |
| Tinh chỉnh agent trước pilot | C | A | R | C | I |
| Xây bộ eval và golden case | I | C | R | A | I |
| Onboarding sale về quy trình HITL | A | R | I | C | C |
| Vận hành pilot 2 tuần và thu số liệu | A | R | R | R | C |
| Review kết quả và quyết định go / no-go | R | C | C | C | A (mentor) |

Mỗi hàng có đúng 1 Accountable. Việc chuẩn bị dữ liệu căn do chủ sàn chịu trách nhiệm cuối vì dữ liệu và nhân sự là của họ. Việc review go/no-go để mentor làm Accountable cho khách quan.

---

## Trang 3 – AI Team Design

### Team Architecture: Centralized

Ở giai đoạn MVP và chuẩn bị pilot đầu tiên, cả 4 thành viên là một nhóm AI lõi phục vụ một sản phẩm. Embedded hoặc Hybrid chỉ cần khi có nhiều sản phẩm hoặc nhiều team dùng chung năng lực AI, chưa phải lúc này.

### Core Roles (đã có trong team)

- AI Product / Founder – Lực: chọn use case, value metric, viết pitch, làm việc với sàn.
- AI Engineer – Hưng: dựng graph LangGraph, structured output, tích hợp tool lịch và inventory.
- Data / Backend – Loan: schema Postgres, crawl dữ liệu tin đăng, khóa giao dịch chống double-booking.
- Eval / QA / UX – Phương: bộ kịch bản, chỉ số containment và no-show, luồng người dùng.

### Extended Roles (khi scale)

Forward Deployed Engineer để onboard từng sàn; MLOps khi traffic tăng; Domain Expert bất động sản; Legal / Compliance cho hợp đồng dữ liệu và bảo vệ thông tin khách.

### Capability gap và Priority Resourcing

| Capability gap | Cách bổ sung | Vì sao | Khi nào cần |
|---|---|---|---|
| Hiểu quy trình môi giới bất động sản Hà Nội thật (cách giữ căn, hoa hồng, thói quen sale) | **Partner** với 1 sale kỳ cựu ở sàn pilot, đổi feedback lấy quyền dùng miễn phí trong pilot | Không cần tuyển full-time ở giai đoạn MVP, chỉ cần người sửa giả định | Trước pilot đầu tiên |
| Eval có khách xác nhận (hiện có 150 kịch bản tự viết, chưa có golden set khách ký, chưa chạy tự động mỗi release) | **Không tuyển thêm.** Phương và Loan xây bộ 30 golden case và gắn vào CI trong 30 ngày. Thuê ngoài QA part-time chỉ khi khối lượng tăng sau pilot | Team đã có người phụ trách eval, chỉ thiếu dữ liệu thật từ khách | Ngay, trước pilot |
| Bán hàng B2B và onboarding đối tác (chưa ai trong team từng ký pilot với sàn) | **Partner** với mentor hoặc một người có sẵn mạng lưới sàn ở Hà Nội để giới thiệu. Thuê freelancer sales chỉ sau khi có case pilot đầu tiên | Cần 1 case thành công làm bằng chứng trước khi bỏ tiền thuê | Sau pilot đầu tiên |

### Squad Goal

Squad của chúng tôi sở hữu **trợ lý đặt lịch xem nhà Nera** và chịu trách nhiệm đưa **tỉ lệ yêu cầu xem nhà hợp lệ được chốt thành lịch** từ **hiện trạng demo chưa có khách thật** đến **ít nhất 15 lịch hợp lệ với no-show dưới 20% qua một pilot sàn trong 30 ngày**.

---

## Trang 4 – Team Health & Growth Plan

### Tự chấm Team Health (thang 1–5, mỗi thành viên tự cho điểm)

| Khía cạnh | Lực | Hưng | Loan | Phương | Trung bình |
|---|---|---|---|---|---|
| Chất lượng AI | 4 | 4 | 3 | 3 | 3.5 |
| Tiến độ | 4 | 3 | 2 | 3 | 3.0 |
| Tinh thần team | 4 | 4 | 3 | 4 | 3.75 |
| Tốc độ ra sản phẩm | 4 | 3 | 3 | 4 | 3.5 |

### So sánh điểm và chọn vấn đề

Khía cạnh thấp nhất là **Tiến độ** (trung bình 3.0). Chênh lệch lớn nhất giữa các thành viên cũng ở Tiến độ: Lực chấm 4, Loan chấm 2.

Lý do chênh lệch: Lực nhìn ở góc sản phẩm nên thấy demo đã chạy thật và coi là "đang đúng hướng". Loan làm mảng dữ liệu nên thấy rõ backlog: crawl chưa phủ đủ căn, schema còn phải bổ sung trước khi pilot, nên chấm thấp. Hai người đang đo "tiến độ" theo hai thước đo khác nhau.

Một vấn đề nếu không xử lý sẽ ảnh hưởng trực tiếp đến milestone tiếp theo: chưa có pilot khách thật, nên mọi giả định về giá, containment và no-show vẫn là ước lượng. Không đủ bằng chứng cho các mốc sau như demo day hoặc gọi vốn.

### Competency cần nâng

- Vai trò: Data / Backend (Loan)
- Level hiện tại: gần L2 – AI Practitioner (dựng được pipeline dữ liệu và schema cho agent).
- Competency cần nâng tiếp: vững L2 ở mảng dữ liệu phục vụ eval, tức biến log hội thoại thành golden case tái lập được.
- Action 30 ngày: cùng Phương dựng bộ 30 golden case từ log pilot và gắn vào CI chạy mỗi release.

### Growth Plan 30 ngày

| Vấn đề | Hành động 30 ngày | Owner | Deadline | Dấu hiệu hoàn thành |
|---|---|---|---|---|
| Chưa có khách pilot thật | Đặt lịch gặp 3 chủ sàn ở Hà Nội, chốt 1 pilot 2 tuần với 1 sale | Lực | 12/09/2026 | Có 1 biên bản pilot ký, ghi rõ sàn, phạm vi và KPI |
| Eval chưa có khách xác nhận | Xây bộ 30 golden case từ hội thoại pilot, gắn vào CI chạy mỗi release | Phương (cùng Loan) | 26/09/2026 | `scripts/run_chat_agent_eval.py` chạy đủ 30 case, report lưu lại mỗi release |
| Hai thành viên đo tiến độ khác nhau, backlog dữ liệu chưa rõ | Loan lập backlog dữ liệu có ước lượng, cả team review tiến độ 20 phút mỗi thứ Sáu | Hưng | 19/09/2026 | Backlog trong repo, có log 3 buổi review thứ Sáu |

### Kiểm tra tính nhất quán giữa 4 trang

- Chủ sàn pilot và sale sàn là stakeholder ưu tiên ở Trang 1, đồng thời là đối tượng của Pitch và có vai trong RACI ở Trang 2.
- Capability gap ở Trang 3 (domain bất động sản, eval có khách ký) đúng là vấn đề Team Health ở Trang 4.
- Owner Growth Plan khớp RACI: Lực chịu trách nhiệm phần khách và pilot, Phương và Loan chịu phần eval và dữ liệu.
