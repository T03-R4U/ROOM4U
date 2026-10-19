# Room4U

Website đặt và quản lý phòng học nhóm trong thư viện, thuộc học phần **Introduction to Software Engineering** tại Trường Đại học Khoa học Tự nhiên, ĐHQG-HCM (HCMUS), do nhóm **T03 – R4U** thực hiện.

> README tạm thời, tóm tắt theo tài liệu **Project Proposal & Plan (PA1)** do nhóm cung cấp. Các tính năng, công nghệ và mốc triển khai dưới đây là kế hoạch; sẽ được cập nhật khi yêu cầu, thiết kế và mã nguồn được hoàn thiện.

## Tổng quan

Room4U cung cấp một nơi để sinh viên tìm phòng phù hợp, xem lịch, gửi yêu cầu đặt phòng và theo dõi kết quả. Thủ thư xét duyệt yêu cầu, quản lý thông tin phòng và chặn lịch bảo trì.

Dự án là website mô phỏng độc lập với dữ liệu thử nghiệm, giả định một thư viện có **8 phòng**, hai vai trò **Student** và **Library Administrator**, cùng các tài khoản được tạo sẵn. Bối cảnh nghiệp vụ là giả định phục vụ đồ án, không phải kết quả khảo sát quy trình thực tế tại HCMUS. Hệ thống không kết nối cơ sở dữ liệu sinh viên, SSO hay thiết bị thư viện của trường.

## Trạng thái hiện tại

Repo hiện chứa tài liệu ban đầu và các file giữ chỗ cho cấu trúc dự án. Chưa có ứng dụng có thể chạy, cấu hình cài đặt, bộ kiểm thử hoặc CI. Hướng dẫn cài đặt, biến môi trường, chạy và kiểm thử sẽ được bổ sung khi có mã nguồn triển khai.

## Phạm vi dự kiến

| Mã | Tính năng cốt lõi | Nội dung |
| --- | --- | --- |
| F01 | Đăng nhập và phân quyền | Đăng nhập/đăng xuất bằng tài khoản có sẵn; API kiểm tra vai trò và quyền sở hữu dữ liệu. |
| F02 | Tìm phòng và xem lịch | Xem ảnh, sức chứa, thiết bị; lọc theo số người và ngày; xem các khung giờ đã duyệt hoặc bị chặn. |
| F03 | Quản lý yêu cầu cá nhân | Gửi yêu cầu, xem trạng thái và lịch sử; hủy yêu cầu hoặc lịch đặt của mình trước giờ bắt đầu. |
| F04 | Xử lý yêu cầu đặt phòng | Thủ thư duyệt/từ chối yêu cầu; hủy lịch đã duyệt kèm lý do; kiểm tra lại xung đột khi duyệt. |
| F05 | Quản lý phòng và lịch khả dụng | Thêm/sửa thông tin phòng, ngừng nhận yêu cầu mới và chặn các khoảng bảo trì. |

Cả năm tính năng đều thuộc phạm vi **Must-have** theo proposal.

Luồng yêu cầu dự kiến:

```text
Gửi yêu cầu → Pending → Approved hoặc Rejected
Sinh viên có thể hủy Pending/Approved của mình trước giờ bắt đầu → Cancelled
Thủ thư có thể hủy Approved kèm lý do → Cancelled
```

**Pending chưa giữ chỗ.** Chỉ yêu cầu được duyệt thành công mới trở thành lịch đặt xác nhận.

### Quy tắc nghiệp vụ sơ bộ

- Giờ mở cửa: **08:00–20:00**, múi giờ `Asia/Ho_Chi_Minh`.
- Thời lượng đặt phòng: **30–120 phút**, đặt trước tối đa **7 ngày**, số người không vượt sức chứa phòng.
- Giả định PA1: giờ bắt đầu theo bước 30 phút; cần xác nhận chi tiết ở PA2.
- Backend kiểm tra quy tắc và lịch khả dụng; việc duyệt đồng thời phải ngăn hai lịch `Approved` trùng phòng và thời gian.
- Các khoảng thời gian liền kề được phép, ví dụ 09:00–10:00 và 10:00–11:00.
- Phải xử lý các lịch `Approved` bị ảnh hưởng trước khi tạo khoảng bảo trì. Phòng ngừng hoạt động không nhận yêu cầu mới.
- Không sửa trực tiếp phòng hoặc giờ của lịch `Approved`; cần hủy kèm lý do và gửi yêu cầu mới. Lịch sử được giữ lại.

Các quy tắc và ngoại lệ sẽ được làm rõ trong PA2.

### Ngoài phạm vi

Tự đăng ký tài khoản, SSO của trường, thanh toán, tính năng AI, ứng dụng di động native, email/nhắc lịch, trung tâm thông báo, check-in, thống kê phân tích, phản hồi, phạt vắng mặt và giao diện cấu hình quy tắc đặt phòng.

## Công nghệ dự kiến

| Thành phần | Công nghệ |
| --- | --- |
| Giao diện | HTML, CSS, JavaScript |
| Backend | Node.js, Express |
| Cơ sở dữ liệu | PostgreSQL |
| Xác thực | Firebase Authentication; backend xác minh token và kiểm tra quyền |
| Triển khai | Linux VPS, Nginx, HTTPS |
| Kiểm thử và CI | Vitest, Supertest, Playwright, GitHub Actions |

Đây là lựa chọn ở giai đoạn lập kế hoạch, cần kiểm chứng trong giai đoạn thiết kế. Kiến trúc chi tiết sẽ được ghi nhận tại PA3.

## Cấu trúc repository

Các thư mục hiện có và mục đích sử dụng:

```text
ROOM4U/
├── README.md
├── src/                      # Mã nguồn ứng dụng (hiện là file giữ chỗ)
├── docs/
│   ├── management/           # Kế hoạch, WBS, rủi ro, biên bản
│   │   └── weekly/           # Báo cáo tiến độ tuần
│   ├── requirements/         # Yêu cầu, use case, mockup
│   ├── design/               # Kiến trúc, ADR, UML, thiết kế UI
│   ├── test/                 # Kế hoạch, ca kiểm thử, kết quả
│   └── ai-usage/             # Nhật ký sử dụng AI
└── pa/                       # Tài liệu bàn giao các PA
```

Theo proposal, `pa/` sẽ được tổ chức thành `PA0/` đến `PA5/`, mỗi thư mục lưu bản xuất source báo cáo và PDF tương ứng. Cấu trúc con này chưa được tạo đầy đủ trong repo hiện tại.

## Lộ trình dự kiến

Dự án theo Waterfall, kết hợp lập kế hoạch kiểm thử theo tinh thần V-model.

| Mốc | Nội dung | Tuần dự kiến |
| --- | --- | --- |
| PA1 | Proposal và kế hoạch | Cuối tuần 3 |
| PA2 | Yêu cầu và tiêu chí chấp nhận | Cuối tuần 5 |
| PA3 | Thiết kế hệ thống | Cuối tuần 7 |
| PA4 | Kế hoạch kiểm thử | Cuối tuần 8 |
| PA5 | Triển khai, kiểm thử và bàn giao | Cuối tuần 10 |
| Final | Thuyết trình và demo | Tuần 11 |

Lập trình chức năng dự kiến bắt đầu sau PA3. Hạn nộp chính thức theo thông báo trên Moodle.

## Thành viên

| MSSV | Họ và tên | Vai trò chính theo proposal |
| --- | --- | --- |
| 22127245 | Nguyễn Thành Long | Team Lead / Project Manager |
| 24127506 | Trần Thiên Phúc | Backend / Database / Architecture |
| 24127127 | Lại Minh Thông | Business Analyst / Testing |
| 24127461 | Nguyễn Kim Ngân | Frontend / UI Designer |

## Đóng góp

Quy trình dự kiến của nhóm: **Issue → branch → pull request → ít nhất một thành viên duyệt → merge**. Commit theo Conventional Commits, ví dụ `docs:`, `feat:`, `fix:`, `test:`. Không đưa mật khẩu, API key, file `.env` hoặc thông tin service account vào repo.

README sẽ tiếp tục được cập nhật theo các bản yêu cầu, thiết kế và sản phẩm đã được nhóm xác nhận.
