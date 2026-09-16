# Tài liệu Walkthrough UAT Thủ công IPMS

| Thuộc tính | Nội dung |
| --- | --- |
| Mã tài liệu | `IPMS-UAT-WALKTHROUGH` |
| Phiên bản | `1.1` |
| Trạng thái | Lưu hành chính thức cho vòng UAT web MVP |
| Ngày ban hành | 2026-09-16 |
| Chủ sở hữu | Business Analyst / QA Coordination |
| Đối tượng sử dụng | BA, QA, tester UAT, đại diện nghiệp vụ khách hàng, đội triển khai |
| Phạm vi hệ thống | IPMS web MVP, API native, dữ liệu seed phục vụ UAT |
| Mức bảo mật | Nội bộ dự án; không chuyển tiếp ra ngoài phạm vi UAT nếu chưa được PM/PO phê duyệt |

## 0. Quản lý tài liệu

### 0.1 Phạm vi áp dụng

Tài liệu áp dụng cho các phân hệ web MVP sau:

| Nhóm | Phân hệ |
| --- | --- |
| Nền tảng | Đăng nhập, dashboard, điều hướng, phân quyền, audit |
| Nghiệp vụ lõi | Khách hàng, hợp đồng, lô đất/nhà xưởng, tài sản, hạ tầng, GIS |
| Vận hành | Tính phí, tài chính/công nợ, phiếu yêu cầu, SLA, thông báo |
| Tích hợp biên | E-invoice, chữ ký số, provider boundary ở chế độ không gọi provider thật |
| Báo cáo | Báo cáo tổng quan, export Excel/PDF, lịch sử export |

### 0.2 Vai trò và trách nhiệm

| Vai trò | Trách nhiệm |
| --- | --- |
| BA | Điều phối walkthrough, giải thích nghiệp vụ, xác nhận cách ghi nhận kết quả |
| QA/Test Lead | Phân công tester, kiểm tra bằng chứng, tổng hợp defect và regression |
| Tester UAT | Thực hiện case theo tài liệu, ghi kết quả và bằng chứng đúng mẫu |
| Đại diện nghiệp vụ khách hàng | Xác nhận nghiệp vụ, mẫu biểu, công thức và quyết định chấp nhận/không chấp nhận |
| Dev/Tech Lead | Phân tích lỗi kỹ thuật, fix defect, cung cấp bản build/migration mới |
| PM/PO | Chốt phạm vi, ưu tiên defect, phê duyệt sign-off hoặc quyết định gia hạn |

### 0.3 Quy tắc phát hành và thay đổi

1. Mỗi thay đổi nội dung chính thức phải cập nhật ngày ban hành hoặc ghi thêm dòng vào lịch sử thay đổi.
2. Không chỉnh sửa trực tiếp kết quả UAT đã được ký xác nhận; nếu cần điều chỉnh, tạo phụ lục hoặc biên bản đính chính.
3. Khi có migration, seed hoặc quyền truy cập thay đổi, QA/Test Lead phải xác nhận lại phần `Môi trường và tài khoản` trước khi phát hành cho tester.
4. Tài liệu này không thay thế release gate, security checklist, performance checklist hoặc biên bản nghiệm thu chính thức.

### 0.4 Lịch sử thay đổi

| Phiên bản | Ngày | Người thực hiện | Nội dung |
| --- | --- | --- | --- |
| 1.1 | 2026-09-16 | BA / Codex | Sắp xếp lại cấu trúc trình bày, bổ sung luồng nghiệp vụ, ma trận dữ liệu seed Trà Nóc/Bắc An và chuẩn hóa cách dùng tài liệu cho các bên triển khai. |
| 1.0 | 2026-09-16 | BA / Codex | Chuẩn hóa thành tài liệu lưu hành chính thức, bổ sung kiểm soát tài liệu, phạm vi, vai trò, điều kiện UAT và sign-off. |

### 0.5 Cấu trúc sử dụng tài liệu

| Phần | Nội dung | Người đọc chính | Cách sử dụng |
| --- | --- | --- | --- |
| 1 | Điều kiện chuẩn bị UAT | PM/PO, BA, QA/Test Lead | Xác nhận môi trường, tài khoản và dữ liệu trước khi mở phiên UAT |
| 2 | Nguyên tắc ghi nhận kết quả | QA/Test Lead, tester UAT | Thống nhất cách ghi trạng thái, bằng chứng và dữ liệu tester tự tạo |
| 3 | Bộ dữ liệu seed chuẩn | BA, QA, tester, đại diện nghiệp vụ | Tra mã dữ liệu khi thực hiện case hoặc đối chiếu lỗi |
| 4 đến 13 | Walkthrough nghiệp vụ | Tester UAT, BA, đại diện nghiệp vụ | Thực hiện từng case theo màn hình/phân hệ của hệ thống |
| 14 | Điều kiện sign-off UAT | PM/PO, QA/Test Lead, đại diện nghiệp vụ | Tổng hợp kết quả và xác nhận điều kiện ký UAT |

### 0.6 Luồng nghiệp vụ tổng quát

Tài liệu được sắp theo chuỗi nghiệp vụ mà một khu công nghiệp vận hành trên IPMS:

1. Chuẩn bị môi trường, tài khoản và phạm vi dữ liệu.
2. Kiểm tra nền tảng đăng nhập, dashboard, điều hướng và phân quyền.
3. Quản lý dữ liệu nền khu/cụm, user và phạm vi truy cập.
4. Quản lý khách hàng, liên hệ và hồ sơ pháp lý.
5. Quản lý hợp đồng, tài liệu, line item và lịch thanh toán.
6. Quản lý lô đất, nhà xưởng, tài sản, hạ tầng và bản đồ GIS.
7. Tính phí, phát sinh công nợ, ghi nhận thanh toán và đối soát tài chính.
8. Tiếp nhận phiếu yêu cầu, xử lý SLA, gửi thông báo và ghi nhận audit.
9. Kiểm tra báo cáo, export file, eInvoice và chữ ký số ở mức provider boundary.
10. Tổng hợp bằng chứng, defect còn mở và điều kiện sign-off.

## 1. Điều kiện chuẩn bị UAT

### 1.1 Điều kiện bắt buộc trước khi test

| Điều kiện | Tiêu chí chấp nhận |
| --- | --- |
| Môi trường web/API | Web và API khởi động ổn định, không lỗi cấu hình nền ngay sau đăng nhập |
| Database | Migration đã chạy đến ít nhất `V202609110002__uat_authorization_scope_hardening.sql` |
| Seed UAT | Dữ liệu Trà Nóc và bộ dữ liệu T07/T09/T10/T11/T12 có thể tra cứu được |
| Tài khoản | Tài khoản root, park admin và tenant user đăng nhập được bằng mật khẩu seed được xác nhận |
| Browser | Dùng Chrome hoặc Edge bản ổn định; bật DevTools Network khi cần ghi lỗi API |
| Bằng chứng | Tester có thư mục/biểu mẫu lưu ảnh màn hình, file export, log lỗi và defect log |

Nếu một điều kiện bắt buộc chưa sẵn sàng, QA/Test Lead ghi trạng thái `Blocked by environment` hoặc `Blocked by credential` trước khi bắt đầu walkthrough.

### 1.2 Môi trường và tài khoản

| Hạng mục | Giá trị |
| --- | --- |
| Web local | `http://localhost:8000` |
| API local | `http://localhost:3000/api/v1` |
| Swagger | `http://localhost:3000/api/docs` |
| Root admin | `admin@ipms.local` |
| Mật khẩu root admin | `Admin@123456` |
| Seed chuẩn cho manual test | `Khu công nghiệp Trà Nóc` |
| Mã khu seed | `KCN-TRA-NOC` |
| Migration seed | `V202609110001__tra_noc_manual_test_seed.sql`, `V202609110002__uat_authorization_scope_hardening.sql` |

Tài khoản seed để kiểm tra phân quyền:

| Email | Vai trò | Phạm vi | Mật khẩu quy ước |
| --- | --- | --- | --- |
| `admin@ipms.local` | `ROOT_ADMIN` | Toàn hệ thống | `Admin@123456` |
| `uat.tranoc.park.admin@ipms.local` | `PARK_ADMIN` | Khu công nghiệp Trà Nóc | `Demo@123456` |
| `uat.tranoc.mekongxanh@ipms.local` | `ENTERPRISE_ADMIN` | Công ty TNHH Thực phẩm Mekong Xanh | `Demo@123456` |
| `uat.tranoc.songhau@ipms.local` | `ENTERPRISE_USER` | Công ty TNHH Logistics Sông Hậu Demo | `Demo@123456` |

Ghi chú: nếu môi trường test đổi mật khẩu seed khi rebuild, tester ghi `Blocked by credential` và báo lại QA/Test Lead để xác nhận hash/mật khẩu đang dùng.

## 2. Nguyên tắc thực hiện và ghi nhận kết quả

### 2.1 Quy chuẩn ghi nhận case

Mỗi case cần ghi lại tối thiểu các thông tin sau. Trường `Bằng chứng` là bắt buộc đối với mọi case `Fail`, `Blocked` và các case nghiệp vụ tiền/công nợ/phân quyền dù kết quả là `Pass`.

| Trường | Cách ghi |
| --- | --- |
| Mã case | Ví dụ `UAT-02.03` |
| Tài khoản | Email đang đăng nhập |
| Dữ liệu dùng | Mã khu, mã doanh nghiệp, mã hợp đồng, mã ticket |
| Kết quả | `Pass`, `Fail`, `Blocked`, `Out of scope` |
| Bằng chứng | Ảnh màn hình, file export, mã bản ghi, log/audit |
| Ghi chú | Lỗi, API status, payload, thao tác tái hiện |

### 2.2 Quy ước trạng thái

| Kết quả | Ý nghĩa |
| --- | --- |
| `Pass` | Thao tác thành công, dữ liệu hiển thị đúng ở màn hình liên quan |
| `Fail` | Lỗi hệ thống, dữ liệu sai, trang trắng, lỗi quyền không hợp lý |
| `Blocked` | Thiếu tài khoản, provider, template chính thức, môi trường hoặc dữ liệu ngoài phạm vi |
| `Out of scope` | Không thuộc phạm vi web MVP/manual UAT hiện tại |

### 2.3 Quy tắc bằng chứng chính thức

1. Ảnh màn hình phải thể hiện URL hoặc tiêu đề màn hình, dữ liệu chính và trạng thái thao tác.
2. File export phải lưu nguyên tên file tải xuống; nếu đổi tên để lưu trữ, giữ lại tên gốc trong ghi chú.
3. Lỗi API cần ghi HTTP status, endpoint, thời điểm test và thông điệp lỗi hiển thị cho người dùng.
4. Không chụp/lưu mật khẩu, token, cookie, private key hoặc credential provider thật trong bằng chứng.
5. Một case chỉ được ký `Pass` khi cả thao tác chính, dữ liệu liên quan và phân quyền hiển thị đều đúng theo kết quả mong đợi.

### 2.4 Quy tắc dữ liệu tester tự tạo

Tiền tố dữ liệu tester tự tạo: dùng `UAT-YYYYMMDD-...` để dễ lọc và dọn sau test. Không sửa/xóa dữ liệu seed `TN-*` trừ khi case yêu cầu kiểm tra chỉnh sửa có chủ đích.

Khi một case yêu cầu tạo mới nhưng màn hình chưa có đủ chức năng, tester ghi `Blocked`, nêu rõ màn hình/field bị thiếu và tiếp tục các bước đọc/đối chiếu bằng dữ liệu seed nếu còn thực hiện được.

## 3. Bộ dữ liệu seed chuẩn và cách đối chiếu

Tester ưu tiên dùng `Khu công nghiệp Trà Nóc` để đi luồng nghiệp vụ lõi: quản trị, khách hàng, hồ sơ, lô đất/nhà xưởng, hợp đồng, lịch thanh toán, phải thu, thanh toán, ticket, thông báo và phân quyền tenant. Khi cần kiểm tra tạo mới, tạo thêm bản ghi `UAT-*` riêng.

Khi cần kiểm tra các luồng đã có dữ liệu seed sâu hơn như điện 3 khung giờ, COS phi, giấy báo phí, công nợ nhà cung cấp, import/export, PDF, SLA nâng cao, GIS liên kết, eInvoice và chữ ký số provider boundary, tester dùng bộ `Khu công nghiệp Bắc An` làm bộ đối chiếu chuẩn. Các mã Bắc An ở mục 3.7 đến 3.14 là một phần của cùng kế hoạch UAT, không phải dữ liệu ngoài tài liệu.

### Hướng dẫn đọc bộ dữ liệu

| Nhóm dữ liệu | Khu dùng chính | Khi nào dùng |
| --- | --- | --- |
| Walkthrough nghiệp vụ lõi | Trà Nóc | Dùng cho khách hàng, hợp đồng, lô đất, công nợ, thanh toán, ticket, notification và phân quyền tenant |
| Regression/chuyên sâu | Bắc An | Dùng cho billing utility, provider boundary, báo cáo, import/export, GIS, SLA và tài chính nhà cung cấp |
| Dữ liệu tester tự tạo | Mọi khu theo case | Dùng tiền tố `UAT-*`, không ghi đè mã seed chính thức |

### 3.1 Khu và cụm

| Loại | Mã | Tên | Trạng thái |
| --- | --- | --- | --- |
| Khu công nghiệp | `KCN-TRA-NOC` | Khu công nghiệp Trà Nóc | `active` |
| Cụm/khu | `TRA-NOC-A` | Khu A - Trà Nóc 1 | `active` |
| Cụm/khu | `TRA-NOC-B` | Khu B - Trà Nóc 2 | `active` |

### 3.2 Doanh nghiệp

| Mã | Tên doanh nghiệp | MST | Email | Điện thoại | Nhóm | Công nợ còn lại |
| --- | --- | --- | --- | --- | --- | ---: |
| `TN-CUST-001` | Công ty TNHH Thực phẩm Mekong Xanh | `1809001001` | `contact@mekongxanh.demo` | `02923801001` | `strategic` | 46,200,000 |
| `TN-CUST-002` | Công ty Cổ phần Bao bì Hậu Giang Demo | `1809001002` | `admin@baobihaugiang.demo` | `02923801002` | `vip` | 326,700,000 |
| `TN-CUST-003` | Công ty TNHH Logistics Sông Hậu Demo | `1809001003` | `ops@logisticssonghau.demo` | `02923801003` | `standard` | 66,000,000 |
| `TN-CUST-004` | Công ty Cổ phần Cơ khí Tây Đô Demo | `1809001004` | `info@cokhitaydo.demo` | `02923801004` | `strategic` | 171,600,000 |
| `TN-CUST-005` | Công ty TNHH Dược liệu Cửu Long Demo | `1809001005` | `legal@duoclieucuulong.demo` | `02923801005` | `standard` | 33,000,000 |

Liên hệ chính tương ứng:

| Doanh nghiệp | Người liên hệ | Chức danh | Email | Điện thoại |
| --- | --- | --- | --- | --- |
| `TN-CUST-001` | Nguyễn Minh Huy | Giám đốc | `huy.nguyen@mekongxanh.demo` | `0909001001` |
| `TN-CUST-002` | Trần Thị Mai Anh | Tổng giám đốc | `maianh.tran@baobihaugiang.demo` | `0909001002` |
| `TN-CUST-003` | Lê Quốc Bảo | Giám đốc vận hành | `bao.le@logisticssonghau.demo` | `0909001003` |
| `TN-CUST-004` | Phạm Văn Tín | Chủ tịch HĐQT | `tin.pham@cokhitaydo.demo` | `0909001004` |
| `TN-CUST-005` | Võ Thanh Phương | Giám đốc pháp chế | `phuong.vo@duoclieucuulong.demo` | `0909001005` |

### 3.3 Hồ sơ khách hàng

| Doanh nghiệp | Loại hồ sơ | Số hiệu | File |
| --- | --- | --- | --- |
| `TN-CUST-001` | `business_license` | `TN-BL-001` | `business-license-tn-cust-001.pdf` |
| `TN-CUST-002` | `tax_registration` | `TN-TAX-002` | `tax-registration-tn-cust-002.pdf` |
| `TN-CUST-003` | `business_license` | `TN-BL-003` | `business-license-tn-cust-003.pdf` |
| `TN-CUST-004` | `environment_commitment` | `TN-ENV-004` | `environment-commitment-tn-cust-004.pdf` |
| `TN-CUST-005` | `tax_registration` | `TN-TAX-005` | `tax-registration-tn-cust-005.pdf` |

Seed download regression: customer `91310000-0000-4000-8000-000000000002` đã có file inline để kiểm tra tải hồ sơ khách hàng.

### 3.4 Lô đất, nhà xưởng, hạ tầng

| Mã | Tên | Trạng thái | Khách hàng hiện tại |
| --- | --- | --- | --- |
| `TN-LOT-A1` | Lô A1 - Thực phẩm Mekong Xanh | `leased` | `TN-CUST-001` |
| `TN-LOT-A3` | Lô A3 - Bao bì Hậu Giang | `leased` | `TN-CUST-002` |
| `TN-LOT-A7` | Lô A7 - Dược liệu Cửu Long | `leased` | `TN-CUST-005` |
| `TN-LOT-B5` | Lô B5 - Cơ khí Tây Đô | `leased` | `TN-CUST-004` |
| `TN-LOT-B9` | Lô B9 dự phòng dịch vụ | `available` | Chưa gán |

| Mã | Loại | Tên | Trạng thái |
| --- | --- | --- | --- |
| `TN-FAC-F1` | Nhà xưởng | Nhà xưởng F1 - Logistics Sông Hậu | `active` |
| `TN-FAC-F2` | Nhà xưởng | Nhà xưởng F2 - Dự phòng Trà Nóc | `active` |
| `TN-INF-WATER` | Hạ tầng cấp nước | Hạ tầng cấp nước Trà Nóc | `active` |
| `TN-INF-WW` | Xử lý nước thải | Hạ tầng xử lý nước thải Trà Nóc | `active` |

### 3.5 Hợp đồng và tài chính

| Mã hợp đồng | Tên | Loại | Giá trị | Hiệu lực |
| --- | --- | --- | ---: | --- |
| `TN-CON-LAND-001` | Hợp đồng thuê đất A1 - Mekong Xanh | `land_lease` | 4,320,000,000 | 2026-02-01 đến 2031-01-31 |
| `TN-CON-LAND-002` | Hợp đồng thuê đất A3 - Bao bì Hậu Giang | `land_lease` | 5,940,000,000 | 2026-03-01 đến 2031-02-28 |
| `TN-CON-FACT-001` | Hợp đồng thuê nhà xưởng F1 - Logistics Sông Hậu | `factory_lease` | 2,160,000,000 | 2026-04-01 đến 2029-03-31 |
| `TN-CON-LAND-003` | Hợp đồng thuê đất B5 - Cơ khí Tây Đô | `land_lease` | 7,488,000,000 | 2026-02-01 đến 2032-01-31 |
| `TN-CON-SVC-001` | Hợp đồng dịch vụ hạ tầng - Dược liệu Cửu Long | `service` | 720,000,000 | 2026-05-01 đến 2028-04-30 |

Line item hợp đồng:

| Mã hợp đồng | Đối tượng tính phí | Diễn giải | Số lượng/diện tích | Đơn giá | Thành tiền |
| --- | --- | --- | ---: | ---: | ---: |
| `TN-CON-LAND-001` | `TN-LOT-A1` | Thuê đất lô A1 | 18,000 m2 | 4,000 | 72,000,000 |
| `TN-CON-LAND-002` | `TN-LOT-A3` | Thuê đất lô A3 | 22,000 m2 | 4,500 | 99,000,000 |
| `TN-CON-FACT-001` | `TN-FAC-F1` | Thuê nhà xưởng xây sẵn F1 | 1 tháng | 60,000,000 | 60,000,000 |
| `TN-CON-LAND-003` | `TN-LOT-B5` | Thuê đất lô B5 | 26,000 m2 | 4,000 | 104,000,000 |
| `TN-CON-SVC-001` | `TN-INF-WATER` | Gói dịch vụ hạ tầng cấp nước và nước thải | 1 tháng | 30,000,000 | 30,000,000 |

Lịch thanh toán hợp đồng:

| Mã hợp đồng | Kỳ tính | Hạn thanh toán | Loại phí | Trước VAT | VAT | Tổng tiền | Trạng thái |
| --- | --- | --- | --- | ---: | ---: | ---: | --- |
| `TN-CON-LAND-001` | 2026-09-01 đến 2026-09-30 | 2026-09-10 | `rent` | 72,000,000 | 7,200,000 | 79,200,000 | `due` |
| `TN-CON-LAND-002` | 2026-09-01 đến 2026-11-30 | 2026-09-15 | `rent` | 297,000,000 | 29,700,000 | 326,700,000 | `due` |
| `TN-CON-FACT-001` | 2026-09-01 đến 2026-09-30 | 2026-09-10 | `rent` | 60,000,000 | 6,000,000 | 66,000,000 | `planned` |
| `TN-CON-LAND-003` | 2026-09-01 đến 2026-11-30 | 2026-09-20 | `rent` | 312,000,000 | 31,200,000 | 343,200,000 | `planned` |
| `TN-CON-SVC-001` | 2026-09-01 đến 2026-09-30 | 2026-09-05 | `service_fee` | 30,000,000 | 3,000,000 | 33,000,000 | `planned` |

Tài liệu hợp đồng tenant-visible:

| Mã hợp đồng | Tên tài liệu | File |
| --- | --- | --- |
| `TN-CON-LAND-001` | Hợp đồng thuê đất A1 đã ký | `signed-tn-con-land-001.pdf` |
| `TN-CON-LAND-002` | Hợp đồng thuê đất A3 đã ký | `signed-tn-con-land-002.pdf` |
| `TN-CON-FACT-001` | Hợp đồng thuê nhà xưởng F1 đã ký | `signed-tn-con-fact-001.pdf` |
| `TN-CON-LAND-003` | Hợp đồng thuê đất B5 đã ký | `signed-tn-con-land-003.pdf` |
| `TN-CON-SVC-001` | Hợp đồng dịch vụ hạ tầng đã ký | `signed-tn-con-svc-001.pdf` |

| Mã phải thu | Khách hàng | Tổng tiền | Đã thu | Còn lại | Hạn | Trạng thái |
| --- | --- | ---: | ---: | ---: | --- | --- |
| `REC-TN-MEKONG-2026-09` | `TN-CUST-001` | 79,200,000 | 33,000,000 | 46,200,000 | 2026-09-10 | `partial` |
| `REC-TN-BAOBI-2026-Q3` | `TN-CUST-002` | 326,700,000 | 0 | 326,700,000 | 2026-09-15 | `open` |
| `REC-TN-SONGHAU-2026-09` | `TN-CUST-003` | 66,000,000 | 0 | 66,000,000 | 2026-09-10 | `open` |
| `REC-TN-TAYDO-2026-Q3` | `TN-CUST-004` | 343,200,000 | 171,600,000 | 171,600,000 | 2026-09-20 | `partial` |
| `REC-TN-CUULONG-2026-09` | `TN-CUST-005` | 33,000,000 | 0 | 33,000,000 | 2026-09-05 | `overdue` |

Thanh toán seed:

| Mã thanh toán | Khách hàng | Số tiền | Trạng thái |
| --- | --- | ---: | --- |
| `PAY-TN-MEKONG-2026-09-001` | `TN-CUST-001` | 33,000,000 | `posted` |
| `PAY-TN-TAYDO-2026-09-001` | `TN-CUST-004` | 171,600,000 | `posted` |

### 3.6 Ticket và thông báo

| Mã ticket | Tiêu đề | Khách hàng | Trạng thái | Ưu tiên |
| --- | --- | --- | --- | --- |
| `TCK-TN-MEKONG-WATER-001` | Áp lực nước giảm tại lô A1 | `TN-CUST-001` | `in_progress` | `high` |
| `TCK-TN-SONGHAU-CONTRACT-001` | Cần xác nhận diện tích thuê nhà xưởng F1 | `TN-CUST-003` | `received` | `medium` |
| `TCK-TN-CUULONG-WW-001` | Kiểm tra lịch lấy mẫu nước thải | `TN-CUST-005` | `new` | `medium` |

Comment seed cho ticket `TCK-TN-MEKONG-WATER-001`:

| Loại comment | Người ghi | Nội dung | Hiển thị với tenant |
| --- | --- | --- | --- |
| Public tenant comment | `uat.tranoc.mekongxanh@ipms.local` | Áp lực nước giảm từ 08:45, đề nghị phản hồi trước ca chiều | Có |
| Public operations comment | Đội vận hành khu | Đội nước đã tiếp nhận và kiểm tra áp tuyến cấp nước khu A | Có |
| Internal note | Đội vận hành khu | Đối chiếu bơm tăng áp `TN-INF-WATER` trước khi thông báo khách | Không |

Thông báo seed:

| Người nhận | Tiêu đề |
| --- | --- |
| `uat.tranoc.park.admin@ipms.local` | Trà Nóc - Công nợ tháng 09 cần theo dõi |
| `uat.tranoc.mekongxanh@ipms.local` | Trà Nóc - Phiếu nước đang xử lý |

Outbox/audit seed để kiểm tra nền tảng:

| Nhóm | Mã/sự kiện | Ý nghĩa khi walkthrough |
| --- | --- | --- |
| Outbox | `tranoc-manual-test-seed-ready` | Sự kiện park Trà Nóc đã sẵn sàng seed manual test |
| Outbox | `tranoc-mekong-water-ticket-notification` | Sự kiện yêu cầu gửi thông báo cho ticket nước Mekong Xanh |
| Activity log | `manual_test_seeded` | Ghi nhận seed Trà Nóc gồm park, cluster, 5 doanh nghiệp, hồ sơ, hợp đồng, công nợ, ticket, notification và auth scope |
| Auth audit | `uat_tranoc_manual_test_seed_ready` | Ghi nhận user park admin Trà Nóc sẵn sàng cho manual test |

### 3.7 Kỳ tính phí, biểu giá, đồng hồ và chỉ số

Nhóm dữ liệu billing/utility seed hiện dùng khu Bắc An và bộ UAT T09 làm chuẩn vì đã có đủ điện, nước, nước thải, rác, phí dịch vụ, dòng âm scope tenant và kỳ đã khóa để kiểm thử nghiệp vụ.

| Mã kỳ | Tên | Thời gian | Trạng thái | Mục đích test |
| --- | --- | --- | --- | --- |
| `BP-BA-2026-08` | Kỳ tính phí KCN Bắc An tháng 08/2026 | 2026-08-01 đến 2026-08-31 | `reviewed` | Đối chiếu chỉ số/giấy báo phí đã soát |
| `BP-BA-2026-09` | Kỳ tính phí KCN Bắc An tháng 09/2026 | 2026-09-01 đến 2026-09-30 | `draft` | Tạo/sửa dòng tính phí còn mở |
| `BP-NH-2026-08` | Kỳ tính phí KCN Nam Hải tháng 08/2026 | 2026-08-01 đến 2026-08-31 | `closed` | Kiểm tra chặn sửa kỳ đã đóng |
| `BP-T09-BA-2026-07-CLOSED` | UAT T09 - Kỳ billing đã khóa Bắc An tháng 07/2026 | 2026-07-01 đến 2026-07-31 | `closed` | Regression kỳ đã khóa |
| `BP-T09-BA-2026-09` | UAT T09 - Kỳ billing Bắc An tháng 09/2026 | 2026-09-01 đến 2026-09-30 | `reviewed` | Bộ chính để test điện/nước/rác/nước thải |
| `BP-T09-NH-NEG-2026-09` | UAT T09 - Kỳ billing âm Nam Hải tháng 09/2026 | 2026-09-01 đến 2026-09-30 | `reviewed` | Kiểm tra dữ liệu khác park không lộ sang Bắc An |

| Mã biểu giá | Tên | Loại dịch vụ | Đơn vị | Trạng thái |
| --- | --- | --- | --- | --- |
| `TRF-T09-BA-ELEC-TOU-2026` | UAT T09 - Điện Bắc An 3 khung giờ + COS phi | `electricity` | `kWh` | `active` |
| `TRF-T09-BA-WATER-2026` | UAT T09 - Nước sạch Bắc An theo bậc | `water` | `m3` | `active` |
| `TRF-T09-BA-WASTEWATER-2026` | UAT T09 - Xử lý nước thải Bắc An theo bậc | `wastewater` | `m3` | `active` |
| `TRF-T09-BA-WASTE-FIXED-2026` | UAT T09 - Phí thu gom rác cố định Bắc An | `waste` | `tháng` | `active` |
| `TRF-T09-BA-SHARED-FIXED-2026` | UAT T09 - Phí dịch vụ hạ tầng chung Bắc An | `shared_service` | `tháng` | `active` |
| `TRF-BA-LAND-2026` | Đơn giá thuê đất Bắc An | `land_rent` | `m2` | `active` |

| Mã đồng hồ | Loại | Điểm đo | Khách hàng | Kỳ có chỉ số | Sản lượng |
| --- | --- | --- | --- | --- | ---: |
| `MTR-T09-BA-ANPHU-ELEC` | `electricity` | `sales` | `CUS-T05-BA-001` | `BP-T09-BA-2026-09` | 13,200 kWh |
| `MTR-T09-BA-ANPHU-WATER` | `water` | `sales` | `CUS-T05-BA-001` | `BP-T09-BA-2026-09` | 620 m3 |
| `MTR-T09-BA-ANPHU-WASTEWATER` | `wastewater` | `sales` | `CUS-T05-BA-001` | `BP-T09-BA-2026-09` | 590 m3 |
| `MTR-T09-BA-ELEC-PURCHASE` | `electricity` | `purchase` | Nội bộ khu | `BP-T09-BA-2026-09` | 14,700 kWh |
| `MTR-T09-BA-ELEC-INTERNAL` | `electricity` | `internal` | Nội bộ khu | `BP-T09-BA-2026-09` | 1,500 kWh |
| `MTR-T09-NH-NEG-ELEC` | `electricity` | `sales` | `CUS-T05-NH-NEG` | `BP-T09-NH-NEG-2026-09` | 800 kWh |

Chỉ số điện `MTR-T09-BA-ANPHU-ELEC`: tổng 48,000 -> 61,200; bình thường 8,400 kWh, cao điểm 3,200 kWh, thấp điểm 1,600 kWh, COS phi `0.87`. Chỉ số nước/nước thải dùng kiểm tra rule đơn giản: nước 3,380 -> 4,000; nước thải 2,910 -> 3,500.

### 3.8 Dòng tính phí, giấy báo phí và công nợ billing

| Mã dòng tính phí | Loại | Nội dung | Số lượng | Tổng tiền |
| --- | --- | --- | ---: | ---: |
| `T09-ELEC-ANPHU` | `electricity` | UAT T09 - Điện sản xuất An Phú tháng 09/2026, gồm COS phi theo P13 | 13,200 | 29,266,272 |
| `T09-WATER-ANPHU` | `water` | UAT T09 - Nước sạch An Phú tháng 09/2026 | 620 | 7,858,200 |
| `T09-WASTEWATER-ANPHU` | `wastewater` | UAT T09 - Xử lý nước thải An Phú tháng 09/2026 | 590 | 5,164,950 |
| `T09-WASTE-ANPHU` | `waste` | UAT T09 - Phí thu gom rác cố định An Phú tháng 09/2026 | 1 | 231,000 |
| `T09-SHARED-ANPHU` | `shared_service` | UAT T09 - Phí dịch vụ hạ tầng chung An Phú tháng 09/2026 | 1 | 367,500 |
| `T09-NH-NEG-ELEC` | `electricity` | UAT T09 - Dòng âm Nam Hải để kiểm tra scope tenant Bắc An | 800 | 1,490,400 |

| Mã giấy báo phí | Ngày phát hành | Hạn thanh toán | Tổng tiền | Trạng thái | Trạng thái chứng từ |
| --- | --- | --- | ---: | --- | --- |
| `FN-T09-BA-ANPHU-2026-09` | 2026-10-01 | 2026-10-15 | 42,887,922 | `issued` | `generated` |
| `FN-T09-NH-NEG-2026-09` | 2026-10-01 | 2026-10-15 | 1,490,400 | `issued` | `pending` |
| `FN-BA-2026-09-SVLOG` | 2026-09-09 | 2026-09-25 | 15,336,000 | `issued` | `generated` |
| `FN-BA-2026-09-ANTIN` | 2026-09-09 | 2026-09-25 | 3,780,000 | `draft` | `pending` |

### 3.9 Nhà cung cấp, chi phí và phải trả

| Mã nhà cung cấp | Tên | MST | Email | Điện thoại | Trạng thái |
| --- | --- | --- | --- | --- | --- |
| `NCC-T09-DIENLUC-BA` | UAT T09 - Công ty Điện lực Bắc An | `3700009001` | `congno.t09@dienluc-bacan.example` | `02743889009` | `active` |
| `NCC-DIENLUC-BD` | Cong ty Dien luc Binh Duong | `3700123456` | `ketoan@dienlucbd.example` | `02743888001` | `active` |
| `NCC-CAYXANH-ANPHU` | Công ty Cây xanh An Phú | `0314567890` | `congno@cayxanhanphu.example` | `02743888018` | `active` |
| `NCC-MINH-PHAT` | Cong ty Co dien Minh Phat | `0312987654` | `congno@minhphat.example` | `02839990118` | `active` |

| Mã phải trả | Kỳ | Tổng tiền | Đã trả | Còn lại | Trạng thái |
| --- | --- | ---: | ---: | ---: | --- |
| `AP-T09-BA-DIEN-2026-09` | 2026-09 | 33,000,000 | 11,000,000 | 22,000,000 | `partial` |
| `AP-BA-BT-2026-09` | 2026-09 | 19,800,000 | 0 | 19,800,000 | `open` |
| `AP-BA-CX-2026-09` | 2026-09 | 13,500,000 | 0 | 13,500,000 | `open` |
| `AP-BA-DIEN-2026-08` | 2026-08 | 10,000,000 | 4,000,000 | 6,000,000 | `partial` |

### 3.10 Tài sản, hạ tầng và bảo trì

| Mã tài sản | Tên | Loại | Tình trạng | Ưu tiên | Giá trị hiện tại |
| --- | --- | --- | --- | --- | ---: |
| `AST-T07-BA-PUMP-01` | UAT T07 - Cụm bơm tăng áp Bắc An | `pump_station` | `watch` | `critical` | 408,000,000 |
| `AST-T07-BA-CCTV-02` | UAT T07 - Camera cổng logistics Bắc An | `security_camera` | `good` | `high` | 56,000,000 |
| `AST-BA-001` | Trạm bơm nước cấp trung tâm Bắc An | `pump_station` | `watch` | `critical` | 336,000,000 |
| `AST-BA-002` | Máy biến áp TBA-01 22/0.4kV | `transformer` | `good` | `high` | 1,715,000,000 |
| `AST-BA-007` | Cụm xử lý nước thải sinh hoạt 01 | `wastewater_unit` | `needs_repair` | `critical` | 855,000,000 |
| `TN-UTIL-WATER` | Trạm cấp nước Trà Nóc | `water_station` | `good` | `high` | 1,620,000,000 |
| `TN-UTIL-WW` | Trạm xử lý nước thải Trà Nóc | `wastewater_station` | `good` | `high` | 1,890,000,000 |

| Mã kế hoạch | Tên | Hạn tiếp theo | Ưu tiên | Trạng thái |
| --- | --- | --- | --- | --- |
| `MP-T07-BA-PUMP-01` | UAT T07 - Bảo trì cụm bơm tăng áp tháng 09 | 2026-09-20 | `critical` | `planned` |
| `MP-T07-BA-CCTV-02` | UAT T07 - Vệ sinh camera cổng logistics quý IV | 2026-12-12 | `high` | `planned` |
| `MP-BA-001` | Bảo trì trạm bơm cấp nước hằng tháng | 2026-10-05 | `critical` | `planned` |
| `MP-BA-006` | Theo dõi cụm xử lý nước thải 01 hằng tuần | 2026-09-12 | `critical` | `planned` |

Ticket/sự cố hạ tầng liên quan: `TCK-2026-1001` gắn `AST-BA-002`, `TCK-2026-1002` gắn `AST-BA-004`, `TCK-2026-1005` gắn `AST-BA-007`, `TCK-T08-BA-BREACH`, `TCK-T08-BA-DUE-SOON`, `TCK-T08-BA-MET`.

### 3.11 GIS

| Mã layer | Tên | Loại | Trạng thái |
| --- | --- | --- | --- |
| `GIS-T07-BA-ASSET-INFRA` | UAT T07 - Tài sản và hạ tầng Bắc An | `mixed` | `active` |
| `GIS-T07-NH-NEG` | UAT T07 - Lớp kiểm tra Nam Hòa | `mixed` | `active` |
| `GIS-INF-BA` | Điểm hạ tầng kỹ thuật Bắc An | `point` | `active` |
| `GIS-LOT-BA` | Ranh giới lô đất Bắc An | `polygon` | `active` |
| `GIS-ROUTE-BA` | Tuyến giao thông và tiện ích Bắc An | `line` | `active` |

| Mã feature | Tên | Layer | Liên kết nghiệp vụ |
| --- | --- | --- | --- |
| `GIS-T07-INF-WATER-BOOST` | UAT T07 - Điểm bơm tăng áp | `GIS-T07-BA-ASSET-INFRA` | `assets`, `infrastructure_assets` |
| `GIS-T07-LOT-BA-31` | UAT T07 - Ranh lô BA-31 | `GIS-T07-BA-ASSET-INFRA` | `land_lots` |
| `GIS-T07-INC-WATER-001` | UAT T07 - Sự cố áp lực nước BA-31 | `GIS-T07-BA-ASSET-INFRA` | `tickets`, `infrastructure_assets` |
| `GIS-T07-NH-NEG-POINT` | UAT T07 - Điểm ngoại viên Nam Hòa | `GIS-T07-NH-NEG` | `land_lots` |
| `GIS-INF-POWER-01` | Trạm điện Bắc An | `GIS-INF-BA` | `infrastructure_assets` |
| `GIS-INF-WATER-01` | Nhà máy nước Bắc An | `GIS-INF-BA` | `infrastructure_assets` |
| `GIS-WATER-B6-LEAK` | Tuyến nước sau kho lạnh B6 | `GIS-ROUTE-BA` | `infrastructure_assets` |

### 3.12 Báo cáo, import/export và PDF

| Mã báo cáo | Tên | Module | Nhóm | Trạng thái |
| --- | --- | --- | --- | --- |
| `RPT_CUSTOMER_CONTRACTS` | Báo cáo khách hàng và hợp đồng | `contracts` | `customer_contract` | `active` |
| `RPT_UTILITY_CONSUMPTION` | Báo cáo sử dụng điện nước | `billing` | `utility` | `active` |
| `RPT_BILLING_FINANCE` | Báo cáo billing và tài chính | `finance` | `finance` | `active` |
| `RPT_FINANCE_RECONCILIATION` | Báo cáo đối soát phiếu thu | `finance` | `finance` | `active` |
| `RPT_SUPPLIER_DEBT` | Báo cáo công nợ nhà cung cấp | `finance` | `finance` | `active` |
| `RPT_INFRASTRUCTURE_OPERATIONS` | Báo cáo vận hành hạ tầng | `infrastructure` | `operations` | `active` |
| `RPT_TICKET_SLA` | Báo cáo SLA phiếu yêu cầu | `tickets` | `ticket` | `active` |

Export seed đã hoàn tất để tester kiểm tra lịch sử/tải lại file:

| Mã báo cáo | Định dạng | Số dòng | File |
| --- | --- | ---: | --- |
| `RPT_BILLING_FINANCE` | `xlsx` | 12 | `bao-cao-cong-no-bac-an-t10.xlsx` |
| `RPT_FINANCE_RECONCILIATION` | `xlsx` | 2 | `bao-cao-doi-soat-phieu-thu-t09.xlsx` |
| `RPT_INFRASTRUCTURE_OPERATIONS` | `xlsx` | 18 | `bao-cao-van-hanh-ha-tang-bac-an-2026-09.xlsx` |
| `RPT_SUPPLIER_DEBT` | `xlsx` | 1 | `bao-cao-cong-no-nha-cung-cap-t09.xlsx` |
| `RPT_TICKET_SLA` | `pdf` | 8 | `bao-cao-sla-ticket-bac-an-t10.pdf` |

### 3.13 E-invoice và chữ ký số provider boundary

| Mã eInvoice | Trạng thái | Duyệt | Loại | Số tiền | Số hóa đơn/lỗi |
| --- | --- | --- | --- | ---: | --- |
| `EINV-BA-2026-08-001` | `issued` | `approved` | `issue` | 37,048,200 | `00000042` |
| `EINV-BA-2026-09-001` | `draft` | `pending_approval` | `issue` | 15,336,000 | Chưa phát hành |
| `EINV-T11-BA-BOUNDARY-DRAFT` | `draft` | `draft` | `issue` | 42,887,922 | Dữ liệu nháp provider boundary |
| `EINV-T11-BA-BOUNDARY-APPROVED` | `failed` | `approved` | `issue` | 1,288,000 | `provider_mapping_blocked` |
| `EINV-T11-BA-DUPLICATE-BLOCKED` | `failed` | `rejected` | `issue` | 1,288,000 | `duplicate_issuance_blocked` |
| `EINV-T11-NH-NEG-SCOPE` | `draft` | `draft` | `issue` | 1,490,400 | Dòng âm khác park |

| Mã yêu cầu ký | Trạng thái | Loại chứng từ | File | Ghi chú |
| --- | --- | --- | --- | --- |
| `SIGN-BA-HD-LEASE-001` | `signed` | `contract_documents` | `hop-dong-hd-ba-lease-001-da-ky.pdf` | Đã ký mẫu |
| `SIGN-BA-PL-SVC-004-01` | `sent` | `contract_appendices` | `phu-luc-dich-vu-nuoc-sach-ca-dem.pdf` | Đang gửi provider |
| `SIGN-T12-BA-CONTRACT-READY` | `pending_send` | `contract_documents` | `hop-dong-thue-dat-bac-an-t06-signed.pdf` | Sẵn sàng gửi |
| `SIGN-T12-BA-APPENDIX-SENT` | `sent` | `contract_appendices` | `phu-luc-dieu-chinh-gia-t06.pdf` | Đã gửi |
| `SIGN-T12-BA-CONTRACT-FAILED` | `failed` | `contract_documents` | `bien-ban-tham-dinh-phap-ly-t06-internal.pdf` | Chặn do chưa cấu hình provider/certificate |
| `SIGN-T12-FOREIGN-SCOPE-BLOCKED` | `failed` | `contract_documents` | `hop-dong-thue-dat-bac-an-t06-signed.pdf` | Chặn do khác phạm vi park |

### 3.14 Bản đồ dữ liệu Bắc An mở rộng

Bắc An là bộ seed đầy đủ nhất để tester hiểu toàn bộ luồng xử lý dự án từ dữ liệu nền đến vận hành, tài chính, báo cáo và provider boundary. Khi trong các bước UAT có yêu cầu kiểm tra sâu nhưng Trà Nóc chưa có dữ liệu tương ứng, dùng các mã dưới đây để đối chiếu.

#### 3.14.1 Khu, cụm và khách hàng Bắc An

| Loại | Mã | Tên | Ghi chú |
| --- | --- | --- | --- |
| Khu công nghiệp | `IPMS-PARK-01` | Khu công nghiệp Bắc An | Bộ seed chính cho regression/UAT mở rộng |
| Cụm/khu | `BAC-AN-A` | Cụm sản xuất A | Khách sản xuất, cơ khí, điện tử |
| Cụm/khu | `BAC-AN-B` | Cụm logistics B | Khách logistics, kho vận, thực phẩm |

| Mã | Tên doanh nghiệp | Trạng thái | Nhóm | Vai trò trong walkthrough |
| --- | --- | --- | --- | --- |
| `CUS-BA-001` | Công ty TNHH Thiết bị Bắc An | `active` | `strategic` | Khách thuê chính, hợp đồng `HD-BA-LEASE-001`, fee notice/eInvoice mẫu |
| `CUS-BA-002` | Công ty Cổ phần Logistics Sao Việt | `active` | `vip` | Hợp đồng logistics, giấy báo phí tháng 09, ticket nước |
| `CUS-BA-003` | Công ty Cổ phần Cơ khí An Phát | `active` | `strategic` | Khách cơ khí, dữ liệu công nợ/ticket |
| `CUS-BA-004` | Công ty TNHH Bao bì Minh Khang | `active` | `standard` | Hợp đồng nhà xưởng/phụ lục, ticket hợp đồng |
| `CUS-BA-005` | Công ty TNHH Linh kiện Điện tử Hòa Bình | `active` | `vip` | Ticket điện áp, nhu cầu điện/nước lớn |
| `CUS-BA-006` | Công ty Cổ phần Kho vận Bắc An | `active` | `strategic` | Kho vận, cold storage, ticket nước |
| `CUS-BA-007` | Công ty TNHH Thực phẩm Sạch An Tâm | `suspended` | `watchlist` | Kiểm tra khách tạm ngưng, công nợ và môi trường |
| `CUS-BA-008` | Công ty Cổ phần Dệt may Phú Thịnh | `active` | `standard` | Khách lớn, kiểm tra lọc/danh sách |
| `CUS-BA-009` | Công ty TNHH Nội thất Gỗ Việt | `left` | `former` | Kiểm tra khách đã rời khu, không dùng tạo hợp đồng mới |
| `CUS-BA-011` | Công ty Cổ phần Cơ khí Long Phú | `active` | `standard` | Khách mới, hồ sơ pháp lý và workflow tiền hợp đồng |
| `CUS-BA-012` | Công ty TNHH Dược phẩm An Tín | `active` | `strategic` | Kho lạnh, nước, eInvoice/SLA |
| `CUS-T05-BA-001` | Công ty TNHH An Phú Bắc An | `active` | UAT tenant portal | Khách chính cho T05/T08/T09/T10/T11/T12 |

Hồ sơ/hợp đồng/tệp mẫu đáng kiểm tra:

| Mã/đối tượng | File | Mục đích |
| --- | --- | --- |
| `CUS-BA-011` | `giay-dkkd-co-khi-long-phu.pdf` | Hồ sơ đăng ký kinh doanh khách mới |
| `CUS-BA-012` | `giay-du-dieu-kien-kho-duoc.pdf` | Hồ sơ điều kiện kho dược |
| `HD-BA-LEASE-001` | `hop-dong-hd-ba-lease-001-da-ky.pdf` | Hợp đồng thuê đất tenant-visible |
| `FN-BA-2026-08-BACAN` | `giay-bao-phi-fn-ba-2026-08-bacan.pdf` | Giấy báo phí PDF đã render |
| `EINV-BA-2026-08-001` | `hoa-don-dien-tu-fn-ba-2026-08-bacan.pdf`, `.xml` | Hóa đơn điện tử log-only |

#### 3.14.2 Hợp đồng, phụ lục và lịch thanh toán Bắc An

| Mã hợp đồng | Khách hàng | Loại | Trạng thái | Giá trị | Ghi chú |
| --- | --- | --- | --- | ---: | --- |
| `HD-BA-LEASE-001` | `CUS-BA-001` | `land_lease` | `active` | 660,000,000 | Thuê lô `LOT-BA-02`, đã có tài liệu ký |
| `HD-BA-LOG-002` | `CUS-BA-002` | `land_lease` | `pending_approval` | 720,000,000 | Workflow pháp lý/logistics |
| `HD-BA-FAC-003` | `CUS-BA-004` | `factory_lease` | `draft` | 310,000,000 | Hợp đồng nhà xưởng nháp |
| `HD-BA-SVC-004` | `CUS-BA-005` | `service` | `active` | 145,000,000 | Dịch vụ điện nước, có phụ lục ký số |
| `HD-BA-INF-005` | `CUS-BA-006` | `service` | `pending_approval` | 198,000,000 | Dịch vụ hạ tầng logistics |
| `HD-BA-FOOD-006` | `CUS-BA-007` | `land_lease` | `terminated` | 540,000,000 | Kiểm tra hợp đồng đã thanh lý |
| `HD-T06-BA-LAND-101` | `CUS-BA-001` | `land_lease` | UAT T06 | Theo seed T06 | Hợp đồng dùng cho chữ ký số T12 |
| `HD-T06-BA-SVC-102` | `CUS-BA-002` | `service` | UAT T06 | Theo seed T06 | Case lỗi provider/certificate |

| Mã phụ lục/tài liệu | Liên kết | Mục đích kiểm tra |
| --- | --- | --- |
| `PL-BA-SVC-004-01` | `HD-BA-SVC-004` | Phụ lục dịch vụ nước sạch ca đêm, có request ký `SIGN-BA-PL-SVC-004-01` |
| `PL-T06-BA-LAND-101-01` | `HD-T06-BA-LAND-101` | Phụ lục điều chỉnh giá T06, dùng trong `SIGN-T12-BA-APPENDIX-SENT` |
| `bien-ban-phap-ly-sao-viet.pdf` | `HD-BA-LOG-002` | Ghi chú rà soát pháp lý nội bộ |

#### 3.14.3 Lô đất, tài sản, hạ tầng và sự cố Bắc An

| Mã | Loại | Tên | Trạng thái | Ghi chú |
| --- | --- | --- | --- | --- |
| `LOT-BA-01` | Lô đất | Lô đất Bắc An 01 | `reserved` | Gắn khách `CUS-BA-002`, GIS `GIS-LOT-BA-01` |
| `LOT-BA-02` | Lô đất | Lô đất Bắc An 02 | `leased` | Gắn khách `CUS-BA-001`, GIS `GIS-LOT-BA-02` |
| `LOT-T07-BA-31` | Lô đất UAT | UAT T07 - Lô xưởng nhẹ BA-31 | `leased` | Liên kết `HD-BA-LEASE-001`, GIS, hồ sơ pháp lý |
| `LOT-T07-BA-32` | Lô đất UAT | UAT T07 - Lô mở rộng hạ tầng BA-32 | `maintenance` | Hồ sơ pháp lý sắp hết hạn |

| Mã tài sản/hạ tầng | Tên | Loại | Tình trạng | Ghi chú |
| --- | --- | --- | --- | --- |
| `AST-BA-001` | Trạm bơm nước cấp trung tâm Bắc An | `pump_station` | `watch` | Bảo trì `MP-BA-001` |
| `AST-BA-002` | Máy biến áp TBA-01 22/0.4kV | `transformer` | `good` | Liên kết ticket điện |
| `AST-BA-003` đến `AST-BA-008` | Tài sản vận hành Bắc An | Nhiều loại | Theo seed | Camera, đường, xử lý nước thải, barrier, sensor |
| `AST-T07-BA-PUMP-01` | UAT T07 - Cụm bơm tăng áp Bắc An | `pump_station` | `watch` | Bảo trì, tài liệu, GIS, incident |
| `AST-T07-BA-CCTV-02` | UAT T07 - Camera cổng logistics Bắc An | `security_camera` | `good` | Bảo trì quý IV |
| `INF-POWER-01` | Trạm điện Bắc An | `power_station` | `normal` | GIS `GIS-INF-POWER-01`, incident điện |
| `INF-WATER-01` | Nhà máy nước Bắc An | `water_supply` | `normal` | Dùng cho ticket nước/SLA |
| `INF-ROAD-01` | Đường nội bộ A | `internal_road` | `normal` | GIS tuyến đường |
| `INF-T07-BA-WATER-BOOST` | UAT T07 - Tuyến bơm tăng áp nước sạch | `water_booster` | `watch` | Liên kết asset, GIS, incident `INC-T07-BA-WATER-001` |
| `INF-T07-BA-DRAIN-RETENTION` | UAT T07 - Hồ điều tiết nước mưa B2 | `stormwater_retention` | `degraded` | Dùng kiểm tra cảnh báo degraded |

| Mã sự cố | Liên kết | Trạng thái | Mục đích |
| --- | --- | --- | --- |
| `SC-HT-2026-001` | `INF-POWER-01` | Theo seed | Sự cố sụt áp có biên bản kiểm tra RMU |
| `INC-T07-BA-WATER-001` | `AST-T07-BA-PUMP-01`, `INF-T07-BA-WATER-BOOST`, `LOT-T07-BA-31` | `open` | Dao động áp lực nước, tạo/đối chiếu ticket từ hạ tầng |
| `INC-T07-BA-DRAIN-002` | `INF-T07-BA-DRAIN-RETENTION`, `LOT-T07-BA-32` | `in_progress` | Mực nước hồ điều tiết vượt ngưỡng cảnh báo |

#### 3.14.4 GIS Bắc An

| Mã layer | Loại | Feature chính | Ghi chú |
| --- | --- | --- | --- |
| `GIS-LOT-BA` | `polygon` | `GIS-LOT-BA-01`, `GIS-LOT-BA-02` | Ranh giới lô đất Bắc An |
| `GIS-INF-BA` | `point` | `GIS-INF-POWER-01`, `GIS-INF-WATER-01` | Điểm hạ tầng kỹ thuật |
| `GIS-ROUTE-BA` | `line` | `GIS-INF-ROAD-01`, `GIS-WATER-B6-LEAK` | Tuyến giao thông/tiện ích |
| `GIS-T07-BA-ASSET-INFRA` | `mixed` | `GIS-T07-LOT-BA-31`, `GIS-T07-INF-WATER-BOOST`, `GIS-T07-INC-WATER-001` | Liên kết lô đất, asset, hạ tầng, ticket/sự cố |

#### 3.14.5 Ticket, SLA và thông báo Bắc An

| Mã ticket | Khách hàng | Trạng thái | SLA | Nội dung |
| --- | --- | --- | --- | --- |
| `TCK-2026-1001` | `CUS-BA-005` | `in_progress` | `on_track` | Điện áp chập chờn tại xưởng điện tử Hòa Bình |
| `TCK-2026-1002` | `CUS-BA-006` | `received` | `on_track` | Áp lực nước yếu tại trung tâm logistics |
| `TCK-2026-1003` | `CUS-BA-004` | `new` | `on_track` | Camera cổng logistics B nhiễu hình |
| `TCK-2026-1004` | `CUS-BA-009` | `completed` | `breached` | Đèn chiếu sáng gần lô A3 tắt liên tục |
| `TCK-2026-1005` | `CUS-BA-007` | `waiting_confirmation` | `met` | Mùi nước thải tại khu chế biến thực phẩm |
| `TCK-2026-1006` | `CUS-BA-003` | `in_progress` | `on_track` | Xác nhận diện tích bốc dỡ trước gia hạn thuê |
| `TCK-2026-1010` | `CUS-BA-001` | `cancelled` | `not_applicable` | Barrier cổng chính A đóng chậm |
| `TCK-T08-BA-DUE-SOON` | `CUS-T05-BA-001` | `assigned` | `warning` | Ticket nước sắp quá hạn SLA, có ảnh public |
| `TCK-T08-BA-BREACH` | `CUS-T05-BA-001` | `in_progress` | `breached` | Ticket môi trường quá hạn, có ghi chú/tệp internal |
| `TCK-T08-BA-MET` | `CUS-T05-BA-001` | `resolved` | `met` | Ticket đã xử lý đúng hạn, có ảnh sau xử lý |

| Mã chính sách/thông báo | Loại | Mục đích |
| --- | --- | --- |
| `SLA-T08-BA-WATER-HIGH` | SLA policy | Nước ưu tiên cao, 30 phút phản hồi, 240 phút xử lý |
| `SLA-T08-BA-ENV-HIGH` | SLA policy | Môi trường ưu tiên cao, 45 phút phản hồi, 360 phút xử lý |
| `t08_ticket_sla_warning_in_app` | Template | Cảnh báo ticket sắp quá hạn |
| `t08_ticket_sla_breach_in_app` | Template | Escalation ticket quá hạn |
| `t08_ticket_resolved_in_app` | Template | Thông báo ticket đã xử lý |

#### 3.14.6 Billing, tài chính, công nợ và nhà cung cấp Bắc An

| Nhóm | Mã | Ghi chú |
| --- | --- | --- |
| Kỳ billing | `BP-BA-2026-08`, `BP-BA-2026-09`, `BP-T09-BA-2026-09`, `BP-T09-BA-2026-07-CLOSED` | Kỳ thường, kỳ T09 và kỳ đã khóa |
| Kỳ tài chính | `FP-BA-2026-08`, `FP-BA-2026-09`, `FP-T09-BA-2026-09`, `FP-T09-BA-2026-07-CLOSED` | Dùng đối soát công nợ và khóa kỳ |
| Giấy báo phí | `FN-BA-2026-08-BACAN`, `FN-BA-2026-09-SVLOG`, `FN-BA-2026-09-ANTIN`, `FN-T09-BA-ANPHU-2026-09` | Có trạng thái draft/issued/generated |
| Receivable | `REC-FN-BA-2026-08-BACAN`, `REC-BA-DV-2026-09`, `REC-BA-CU-2026-06`, `REC-FN-BA-2026-09-SVLOG`, `REC-FN-BA-2026-09-ANTIN`, `REC-T09-ANPHU-FN-2026-09`, `REC-T09-ANPHU-OVERDUE-2026-06`, `REC-T09-ANPHU-CLOSED-2026-07` | Công nợ billing, công nợ thủ công, overdue và closed-period negative |
| Payment | `PAY-BA-2026-09-001`, `PAY-BA-2026-09-002`, `PAY-T09-ANPHU-2026-10-001`, `PAY-T09-ANPHU-UNAPPLIED-2026-10` | Thanh toán posted/draft/unapplied |
| Nhà cung cấp | `NCC-DIENLUC-BD`, `NCC-MINH-PHAT`, `NCC-CAYXANH-ANPHU`, `NCC-T09-DIENLUC-BA` | Nhà cung cấp điện, bảo trì, cây xanh |
| Phải trả NCC | `AP-BA-DIEN-2026-08`, `AP-BA-BT-2026-09`, `AP-BA-CX-2026-09`, `AP-T09-BA-DIEN-2026-09` | Supplier payable open/partial |
| Thanh toán NCC | `SPAY-BA-2026-09-001`, `SPAY-T09-BA-DIEN-2026-10-001` | Thanh toán nhà cung cấp đã posted |
| Chi phí | `EXP-BA-2026-09-001`, `EXP-BA-2026-09-002` | Chi phí bảo trì/bảo vệ |

| Mã đồng hồ | Loại | Khách hàng | Sản lượng/chỉ số chính |
| --- | --- | --- | --- |
| `MTR-BA-SV-ELEC-01` | `electricity` | `CUS-BA-002` | 8,200 -> 13,480 kWh; có tách bình thường/cao điểm/thấp điểm |
| `MTR-BA-AT-WATER-01` | `water` | `CUS-BA-012` | 120 -> 375 m3 |
| `MTR-T09-BA-ELEC-PURCHASE` | `electricity` | Nội bộ khu | Điện mua tổng 14,700 kWh |
| `MTR-T09-BA-ANPHU-ELEC` | `electricity` | `CUS-T05-BA-001` | 48,000 -> 61,200; COS phi `0.87`; sản lượng 13,200 kWh |
| `MTR-T09-BA-ANPHU-WATER` | `water` | `CUS-T05-BA-001` | 3,380 -> 4,000; sản lượng 620 m3 |
| `MTR-T09-BA-ANPHU-WASTEWATER` | `wastewater` | `CUS-T05-BA-001` | 2,910 -> 3,500; sản lượng 590 m3 |
| `MTR-T09-BA-ELEC-INTERNAL` | `electricity` | Nội bộ khu | Điện nội bộ 1,500 kWh |

#### 3.14.7 Import/export, báo cáo, eInvoice, ký số

| Nhóm | Mã/file | Mục đích |
| --- | --- | --- |
| Báo cáo danh mục | `RPT_CUSTOMER_CONTRACTS`, `RPT_UTILITY_CONSUMPTION`, `RPT_BILLING_FINANCE`, `RPT_FINANCE_RECONCILIATION`, `RPT_SUPPLIER_DEBT`, `RPT_INFRASTRUCTURE_OPERATIONS`, `RPT_TICKET_SLA`, `RPT_WASTE_OPERATIONS` | Danh mục báo cáo active |
| Export | `bao-cao-cong-no-bac-an-t10.xlsx`, `bao-cao-doi-soat-phieu-thu-t09.xlsx`, `bao-cao-cong-no-nha-cung-cap-t09.xlsx`, `bao-cao-van-hanh-ha-tang-bac-an-2026-09.xlsx`, `bao-cao-sla-ticket-bac-an-t10.pdf` | File export seed để kiểm tra tải lại/lịch sử |
| Import | `CUSTOMER_IMPORT_V1`, `ASSET_IMPORT_V1`, large-file boundary 5,000 dòng | Tải mẫu, preview, commit, lỗi giới hạn dòng |
| eInvoice | `EINV-BA-2026-08-001`, `EINV-BA-2026-09-001`, `EINV-T11-BA-BOUNDARY-DRAFT`, `EINV-T11-BA-BOUNDARY-APPROVED`, `EINV-T11-BA-DUPLICATE-BLOCKED` | Không gọi MISA live; kiểm tra log/provider boundary |
| Ký số | `SIGN-BA-HD-LEASE-001`, `SIGN-BA-PL-SVC-004-01`, `SIGN-T12-BA-CONTRACT-READY`, `SIGN-T12-BA-APPENDIX-SENT`, `SIGN-T12-BA-CONTRACT-FAILED` | Kiểm tra queue, sent-like state, file signed và lỗi provider/certificate |
| Dữ liệu âm | `BP-T09-NH-NEG-2026-09`, `MTR-T09-NH-NEG-ELEC`, `FN-T09-NH-NEG-2026-09`, `REC-T09-NH-NEG-2026-09`, `TCK-T08-NH-NEG`, `EINV-T11-NH-NEG-SCOPE`, `SIGN-T12-FOREIGN-SCOPE-BLOCKED` | Kiểm tra park/customer scope không lộ dữ liệu Nam Hải sang Bắc An |

#### 3.14.8 Luồng xử lý end-to-end nên hiểu khi đọc dữ liệu

| Luồng | Dữ liệu gợi ý | Kết quả cần hiểu |
| --- | --- | --- |
| Khách hàng -> hồ sơ -> hợp đồng | `CUS-BA-001`, `HD-BA-LEASE-001`, `hop-dong-hd-ba-lease-001-da-ky.pdf` | Khách có hồ sơ, hợp đồng đang hiệu lực và tài liệu tenant-visible |
| Hợp đồng -> tính phí -> giấy báo phí | `HD-BA-LEASE-001`, `BP-BA-2026-08`, `FN-BA-2026-08-BACAN` | Kỳ billing sinh giấy báo phí, liên kết receivable và PDF |
| Đồng hồ -> billing item -> receivable | `MTR-T09-BA-ANPHU-ELEC`, `T09-ELEC-ANPHU`, `REC-T09-ANPHU-FN-2026-09` | Chỉ số điện/nước/nước thải tạo dòng phí và công nợ |
| Receivable -> payment -> allocation | `REC-T09-ANPHU-FN-2026-09`, `PAY-T09-ANPHU-2026-10-001` | Thanh toán phân bổ làm giảm số dư |
| Supplier -> payable -> payment | `NCC-T09-DIENLUC-BA`, `AP-T09-BA-DIEN-2026-09`, `SPAY-T09-BA-DIEN-2026-10-001` | Đối soát phải trả nhà cung cấp điện |
| Asset/hạ tầng -> incident -> ticket -> SLA | `INF-T07-BA-WATER-BOOST`, `INC-T07-BA-WATER-001`, `TCK-T08-BA-DUE-SOON` | Sự cố/ticket có SLA, comment, attachment, notification |
| GIS -> nghiệp vụ | `GIS-T07-INF-WATER-BOOST`, `GIS-T07-LOT-BA-31`, `GIS-T07-INC-WATER-001` | Feature bản đồ mở được liên kết tới asset/lô/ticket |
| Fee notice -> eInvoice -> file | `FN-BA-2026-08-BACAN`, `EINV-BA-2026-08-001`, `hoa-don-dien-tu-fn-ba-2026-08-bacan.pdf` | Hóa đơn log-only có file PDF/XML tenant-visible |
| Contract document -> ký số | `HD-T06-BA-LAND-101`, `SIGN-T12-BA-CONTRACT-READY`, `SIGN-T12-BA-APPENDIX-SENT` | Yêu cầu ký vào queue/sent-like state, không gọi provider thật |
| Báo cáo/export | `RPT_BILLING_FINANCE`, `RPT_SUPPLIER_DEBT`, `bao-cao-cong-no-bac-an-t10.xlsx` | Báo cáo có lịch sử export và file tải lại |

#### 3.14.9 Đối chiếu phạm vi seed Trà Nóc và Bắc An

Tester đọc theo nguyên tắc: Trà Nóc là bộ seed chính để đi walkthrough nghiệp vụ lõi và phân quyền tenant; Bắc An là bộ seed mở rộng, bao phủ thêm các module chuyên sâu. Nếu cần Trà Nóc có đủ 1:1 như Bắc An ở các module chuyên sâu, cần bổ sung seed database tương ứng ngoài phạm vi chỉnh tài liệu này.

| Nhóm dữ liệu | Trà Nóc hiện có trong seed | Bắc An/bộ UAT mở rộng hiện có | Cách dùng khi test |
| --- | --- | --- | --- |
| Khu/cụm | `KCN-TRA-NOC`, `TRA-NOC-A`, `TRA-NOC-B` | `IPMS-PARK-01`, `BAC-AN-A`, `BAC-AN-B` | Dùng Trà Nóc để test chọn khu/cụm và park scope |
| Doanh nghiệp/contact/user tenant | 5 khách `TN-CUST-001` đến `TN-CUST-005`, user park admin, tenant Mekong Xanh, tenant Sông Hậu | Nhiều khách Bắc An `CUS-BA-*`, `CUS-T05-BA-001` | Dùng Trà Nóc cho luồng tenant; dùng Bắc An khi cần nhiều trạng thái khách |
| Hồ sơ/tệp khách hàng | 5 hồ sơ pháp lý/môi trường/thuế `TN-BL-*`, `TN-TAX-*`, `TN-ENV-*` | Hồ sơ khách hàng, hợp đồng, PDF giấy báo phí, PDF/XML eInvoice | Dùng Trà Nóc để kiểm tra upload/list/download hồ sơ cơ bản |
| Lô đất/nhà xưởng/hạ tầng | `TN-LOT-*`, `TN-FAC-*`, `TN-INF-WATER`, `TN-INF-WW`, `TN-UTIL-*` | `LOT-BA-*`, `AST-*`, `INF-*`, T07 asset/infra/incident | Dùng Trà Nóc cho liên kết khách - hợp đồng - tài sản; dùng Bắc An cho incident/GIS/bảo trì sâu |
| Hợp đồng/line item/lịch thanh toán | 5 hợp đồng `TN-CON-*`, 5 line item, 5 lịch thanh toán, 5 file hợp đồng đã ký | Hợp đồng `HD-BA-*`, phụ lục, tài liệu pháp lý, hợp đồng T06/T12 | Dùng Trà Nóc cho luồng từ hợp đồng đến công nợ; dùng Bắc An cho phụ lục/chữ ký số |
| Phải thu/thanh toán | 5 khoản `REC-TN-*`, 2 thanh toán `PAY-TN-*` | Receivable/payment billing và T09, có overdue/closed/unapplied | Dùng Trà Nóc cho công nợ tenant; dùng Bắc An cho đối soát/kỳ đã khóa |
| Ticket/comment/notification | 3 ticket `TCK-TN-*`, comment public/internal, 2 notification, outbox/audit | Ticket Bắc An/T08, SLA policy, attachment, escalation template | Dùng Trà Nóc cho ticket tenant thật; dùng Bắc An cho SLA nâng cao |
| Billing utility | Chưa có kỳ billing/biểu giá/đồng hồ riêng cho Trà Nóc | Kỳ, biểu giá, đồng hồ, meter reading, fee notice Bắc An/T09 | Dùng Bắc An khi test điện/nước/rác/nước thải và giấy báo phí tự động |
| Nhà cung cấp/chi phí/phải trả | Chưa có supplier payable riêng cho Trà Nóc | Vendor, AP, supplier payment, expense Bắc An/T09 | Dùng Bắc An khi test tài chính chiều mua vào/nhà cung cấp |
| GIS | Chưa có layer/feature GIS riêng cho Trà Nóc | GIS lot/infra/route/T07 Bắc An | Dùng Bắc An khi test bản đồ và liên kết GIS - nghiệp vụ |
| Báo cáo/import/export | Chưa có lịch sử export/import riêng cho Trà Nóc | Report catalog, export history, import templates và boundary case | Dùng Bắc An/T10 khi test báo cáo, tải file, import preview/commit |
| eInvoice/ký số | Chưa có provider boundary riêng cho Trà Nóc | eInvoice T11, digital signature T12, negative scope | Dùng Bắc An để kiểm tra provider boundary, queue, trạng thái lỗi/thành công giả lập |

### 3.15 Ma trận case UAT theo phân hệ

| Phân hệ | Nhóm case | Dữ liệu chính | Bằng chứng tối thiểu |
| --- | --- | --- | --- |
| Khởi động phiên UAT | `UAT-00.*` | Tài khoản root, `KCN-TRA-NOC` | Dashboard, bộ lọc khu/cụm, không lỗi nền |
| Quản trị hệ thống | `UAT-01.*` | Khu/cụm, user, role, park scope | Ảnh danh sách user/role và kết quả chặn sai phạm vi |
| Khách hàng | `UAT-02.*` | `TN-CUST-*`, hồ sơ `TN-*` | Ảnh danh sách, chi tiết khách hàng, file hồ sơ hoặc export |
| Hợp đồng | `UAT-03.*` | `TN-CON-*`, line item, lịch thanh toán | Ảnh chi tiết hợp đồng, tài liệu, lịch thanh toán |
| Hạ tầng/GIS | `UAT-04.*` | `TN-LOT-*`, `TN-FAC-*`, `TN-INF-*`, Bắc An/T07 | Ảnh lô đất/tài sản, liên kết GIS nếu có |
| Tính phí/tài chính | `UAT-05.*` | `REC-TN-*`, `PAY-TN-*`, Bắc An/T09 | Ảnh công nợ, thanh toán, giấy báo phí hoặc dòng billing |
| Ticket/SLA/thông báo | `UAT-06.*` | `TCK-TN-*`, notification, Bắc An/T08 | Ảnh ticket, timeline/comment, notification |
| Provider boundary | `UAT-07.*` | eInvoice T11, chữ ký số T12 | Ảnh trạng thái queue/log, không gọi provider live |
| Báo cáo/export | `UAT-08.*` | Report catalog, export history, Trà Nóc/Bắc An | File tải xuống và ảnh số liệu nguồn |
| Phân quyền/audit | `UAT-09.*` | User Trà Nóc, tenant Mekong Xanh/Sông Hậu | Ảnh chặn quyền, audit có actor/action/entity |

## 4. Kiểm tra khởi động phiên UAT

Các case trong phần này dùng để xác nhận hệ thống đủ điều kiện thao tác trước khi tester đi sâu vào từng phân hệ. Nếu một case khởi động bị `Blocked`, QA/Test Lead cần xử lý trước khi tiếp tục các nhóm case sau.

### UAT-00.01 Đăng nhập root admin

1. Mở `http://localhost:8000`.
2. Đăng nhập bằng `admin@ipms.local` / `Admin@123456`.
3. Xác nhận vào được dashboard.
4. Xác nhận menu có các phân hệ: Quản trị hệ thống, Quản lý khách hàng, Hợp đồng, Tính phí, Tài chính, Tài sản, Hạ tầng, GIS, Phiếu yêu cầu, Thông báo, E-invoice/ký số, Báo cáo.

Kết quả mong đợi: root admin đăng nhập được, không gặp 401/403/500, không có trang trắng.

### UAT-00.02 Chọn dữ liệu Trà Nóc

1. Tại bộ chọn khu/cụm nếu có, chọn `Khu công nghiệp Trà Nóc`.
2. Tìm nhanh mã `KCN-TRA-NOC` hoặc tên `Trà Nóc`.
3. Mở lần lượt các menu chính để xác nhận dữ liệu tải được.

Kết quả mong đợi: dữ liệu Trà Nóc xuất hiện ở các màn hình liên quan, không còn trạng thái thiếu dữ liệu nền ở tab phạm vi dữ liệu.

## 5. Quản trị hệ thống

Phần này xác nhận dữ liệu nền, phạm vi truy cập và cấu hình người dùng. Đây là nền tảng cho toàn bộ luồng khách hàng, hợp đồng, tài chính và phân quyền tenant phía sau.

### UAT-01.01 Kiểm tra dữ liệu nền khu/cụm/phòng ban

1. Vào `Quản trị hệ thống`.
2. Mở tab dữ liệu nền hoặc danh mục.
3. Tìm `KCN-TRA-NOC`, `TRA-NOC-A`, `TRA-NOC-B`.
4. Mở danh mục `Phòng ban`.
5. Tạo phòng ban mới:
   - Mã: `UAT-DEPT-20260916-01`
   - Tên: `Phòng UAT vận hành`
   - Trạng thái: đang hoạt động
6. Lưu và tìm lại bản ghi.

Kết quả mong đợi: tạo phòng ban không báo `400 BAD_REQUEST`, danh sách hiển thị bản ghi mới.

### UAT-01.02 Kiểm tra phạm vi dữ liệu

1. Vào tab `Phạm vi dữ liệu`.
2. Lọc theo `Toàn hệ thống`, `Khu công nghiệp`, `Doanh nghiệp` nếu có.
3. Tìm `Khu công nghiệp Trà Nóc`.
4. Tìm `Công ty TNHH Thực phẩm Mekong Xanh`.

Kết quả mong đợi: tab có dữ liệu seed, không hiện thông báo `Chưa chọn khu công nghiệp hoặc kết nối hệ thống chưa sẵn sàng`.

### UAT-01.03 Tạo người dùng nội bộ phạm vi khu

1. Vào tab `Người dùng`.
2. Bấm `Tạo người dùng`.
3. Nhập:
   - Email: `uat.operator.20260916@ipms.local`
   - Tên: `Nhân viên UAT Trà Nóc`
   - Vai trò: vai trò vận hành khu hoặc park admin/operator có sẵn
   - Park ID: chọn bằng select, option hiển thị `Khu công nghiệp Trà Nóc`
4. Lưu.
5. Mở chi tiết user và kiểm tra vai trò/phạm vi.

Kết quả mong đợi: `Park ID` là select hiển thị tên khu, không bắt nhập UUID tự do; root admin tạo user không bị `400 BAD_REQUEST`.

### UAT-01.04 Tạo người dùng phạm vi doanh nghiệp

1. Vào `Tạo người dùng`.
2. Chọn vai trò/phạm vi doanh nghiệp nếu UI hỗ trợ.
3. Chọn `Park ID` = `Khu công nghiệp Trà Nóc`.
4. Kiểm tra `Customer ID` chuyển thành select.
5. Chọn option `Công ty TNHH Thực phẩm Mekong Xanh`.
6. Lưu.

Kết quả mong đợi: `Customer ID` là select hiển thị tên doanh nghiệp, danh sách được load theo khu đã chọn.

### UAT-01.05 Responsive và validate form quản trị

1. Lặp lại các màn hình chính của `Quản trị hệ thống` ở desktop, tablet, mobile.
2. Thử bỏ trống trường bắt buộc, nhập mã quá dài, nhập email sai định dạng.
3. Kiểm tra nút lưu khi API đang xử lý.

Kết quả mong đợi: form không vỡ layout, lỗi validate rõ bằng tiếng Việt, FE chặn lỗi cơ bản trước khi gọi API, BE vẫn trả lỗi rõ khi payload không hợp lệ.

## 6. Quản lý khách hàng

Phần này kiểm tra vòng đời dữ liệu doanh nghiệp: danh sách khách hàng, thông tin liên hệ, hồ sơ pháp lý và thao tác import/export ở mức danh mục.

### UAT-02.01 Kiểm tra danh sách seed

1. Vào `Quản lý khách hàng`.
2. Chọn/lọc khu `Khu công nghiệp Trà Nóc`.
3. Tìm từng mã `TN-CUST-001` đến `TN-CUST-005`.
4. Mở chi tiết `TN-CUST-001`.

Kết quả mong đợi: đủ 5 doanh nghiệp seed, mở chi tiết không trang trắng.

### UAT-02.02 Tạo doanh nghiệp mới

1. Bấm `Thêm khách hàng`.
2. Nhập:
   - Mã khách hàng: `UAT-CUS-20260916-01`
   - Tên doanh nghiệp: `Công ty TNHH UAT An Phú`
   - MST: `1809999001`
   - Email: `uat.anphu@example.test`
   - Điện thoại: `02923809999`
   - Khu: `Khu công nghiệp Trà Nóc`
   - Trạng thái: `active`
3. Lưu và tìm lại.

Kết quả mong đợi: tạo mới thành công, validate giới hạn độ dài rõ ràng.

### UAT-02.03 Hồ sơ khách hàng

1. Mở chi tiết `TN-CUST-001`.
2. Vào tab `Hồ sơ`.
3. Kiểm tra hồ sơ `TN-BL-001`.
4. Tải file `business-license-tn-cust-001.pdf`.
5. Mở URL report cũ nếu cần kiểm tra hồi quy: `/customers?tab=documents&page=1&customerId=31c8dbe2-ab4d-47f5-bc21-a84611c341d5`.
6. Thử URL bị encode `&amp;` từ report nếu có.

Kết quả mong đợi: không trang trắng, hồ sơ tải/xem được hoặc báo lỗi nghiệp vụ rõ. URL có `&amp;` được chuẩn hóa, không làm mất `customerId`.

### UAT-02.04 Kiểm tra nút import/export khách hàng

1. Ở danh sách khách hàng, quan sát các nút `Xóa bộ lọc`, `Tải mẫu`, `Xuất CSV`, `Nhập XLSX`.
2. Kiểm tra ở desktop và mobile.
3. Bấm `Tải mẫu`, `Xuất CSV`.

Kết quả mong đợi: icon và text nằm trên một dòng ở kích thước đủ, không bị tách dòng xấu; file tải được nếu user có quyền.

## 7. Hợp đồng

Phần này kiểm tra luồng từ khách hàng sang hợp đồng, tài liệu đính kèm, line item và lịch thanh toán. Dữ liệu Trà Nóc là bộ chính để đối chiếu hợp đồng đang hiệu lực.

### UAT-03.01 Kiểm tra hợp đồng seed

1. Vào `Quản lý hợp đồng`.
2. Lọc theo `Khu công nghiệp Trà Nóc`.
3. Tìm các mã `TN-CON-LAND-001`, `TN-CON-LAND-002`, `TN-CON-FACT-001`, `TN-CON-LAND-003`, `TN-CON-SVC-001`.
4. Mở chi tiết từng loại hợp đồng: thuê đất, thuê nhà xưởng, dịch vụ.

Kết quả mong đợi: đủ 5 hợp đồng active, có line items, term versions, lịch thanh toán và tài liệu hợp đồng tenant-visible.

### UAT-03.02 Tạo hợp đồng mới từ UI

1. Bấm `Tạo hợp đồng`.
2. Chọn khách hàng `Công ty TNHH UAT An Phú` hoặc một doanh nghiệp seed.
3. Chọn loại hợp đồng phù hợp.
4. Chọn lô đất `TN-LOT-B9` nếu còn available hoặc tài sản phù hợp.
5. Nhập ngày hiệu lực, giá trị, line item.
6. Lưu.

Kết quả mong đợi: hợp đồng mới lưu được, ngày kết thúc phải sau ngày bắt đầu, số tiền không âm, mã không trùng.

### UAT-03.03 Phụ lục, tài liệu và lịch thanh toán

1. Mở `TN-CON-LAND-001`.
2. Kiểm tra lịch thanh toán tháng 09/2026.
3. Kiểm tra file `signed-tn-con-land-001.pdf`.
4. Tạo phụ lục UAT nếu UI hỗ trợ.

Kết quả mong đợi: dữ liệu hợp đồng liên kết đúng khách hàng/lô đất; file hợp đồng tải được hoặc có trạng thái download rõ.

## 8. Hạ tầng, tài sản và GIS

Phần này kiểm tra liên kết giữa lô đất, nhà xưởng, tài sản vận hành, hạ tầng kỹ thuật và bản đồ. Với các case GIS nâng cao, dùng thêm bộ seed Bắc An/T07.

### UAT-04.01 Lô đất và nhà xưởng

1. Vào `Hạ tầng kỹ thuật`.
2. Tìm `TN-LOT-A1`, `TN-LOT-A3`, `TN-LOT-A7`, `TN-LOT-B5`, `TN-LOT-B9`.
3. Kiểm tra trạng thái leased/available.
4. Tìm `TN-FAC-F1`, `TN-FAC-F2`.

Kết quả mong đợi: lô đất/nhà xưởng hiển thị đúng trạng thái và liên kết khách hàng/hợp đồng nếu có.

### UAT-04.02 Hạ tầng kỹ thuật

1. Tìm `TN-INF-WATER`.
2. Tìm `TN-INF-WW`.
3. Mở chi tiết, kiểm tra trạng thái vận hành và thông tin công suất.
4. Tìm thêm `AST-T07-BA-PUMP-01`, `AST-T07-BA-CCTV-02`, `AST-BA-001`, `AST-BA-007` để kiểm tra luồng tài sản nền, bảo trì và sự cố.
5. Kiểm tra filter/field chọn tài sản hiển thị tên tài sản, không chỉ hiển thị mã/UUID.

Kết quả mong đợi: tài sản hạ tầng hoạt động, có thể liên kết ticket/sự cố.

### UAT-04.03 GIS

1. Vào `GIS`.
2. Lọc/tìm layer `GIS-T07-BA-ASSET-INFRA`.
3. Tìm các feature `GIS-T07-INF-WATER-BOOST`, `GIS-T07-LOT-BA-31`, `GIS-T07-INC-WATER-001`.
4. Thử zoom, pan, mở popup/chi tiết nếu có.
5. Đổi sang layer âm `GIS-T07-NH-NEG` và xác nhận dữ liệu khác park không lẫn vào phạm vi Bắc An/tenant đang test.

Kết quả mong đợi: bản đồ tải được, không trắng, feature mở được popup/chi tiết, liên kết đúng bảng nghiệp vụ `assets`, `land_lots`, `tickets` hoặc `infrastructure_assets`.

## 9. Tính phí và tài chính

Phần này kiểm tra đường đi từ lịch thanh toán hoặc chỉ số sử dụng đến phải thu, thanh toán, giấy báo phí và đối soát tài chính. Trà Nóc dùng cho công nợ cơ bản; Bắc An/T09 dùng cho billing utility và nhà cung cấp.

### UAT-05.01 Kiểm tra lịch thanh toán/phải thu seed

1. Vào `Tài chính & công nợ`.
2. Lọc theo khu `Khu công nghiệp Trà Nóc`.
3. Tìm các mã `REC-TN-*`.
4. Mở `REC-TN-MEKONG-2026-09`.

Kết quả mong đợi: tổng tiền 79,200,000; đã thu 33,000,000; còn lại 46,200,000; trạng thái `partial`.

### UAT-05.02 Ghi nhận thanh toán UAT

1. Tạo thanh toán mới cho một khoản đang `open`, ví dụ `REC-TN-SONGHAU-2026-09`.
2. Nhập số tiền nhỏ hơn hoặc bằng số còn lại.
3. Lưu/ghi nhận.
4. Kiểm tra phân bổ và số dư.

Kết quả mong đợi: số dư giảm đúng, không cho phân bổ vượt số phải thu.

### UAT-05.03 Billing/fee notice

1. Vào `Tính phí`.
2. Mở kỳ `BP-T09-BA-2026-09`.
3. Đối chiếu các dòng phí `T09-ELEC-ANPHU`, `T09-WATER-ANPHU`, `T09-WASTEWATER-ANPHU`, `T09-WASTE-ANPHU`, `T09-SHARED-ANPHU`.
4. Mở giấy báo phí `FN-T09-BA-ANPHU-2026-09` và kiểm tra tổng tiền 42,887,922.
5. Nếu tạo kỳ mới, dùng mã `UAT-BP-202609-01`.
6. Thử xuất/tải giấy báo phí nếu UI hỗ trợ.

Kết quả mong đợi: dữ liệu hợp đồng/phải thu đủ để đối chiếu; PDF/template chính thức có thể `Blocked by official template` nếu chưa được khách hàng chốt.

### UAT-05.04 Điện, nước, nước thải, rác và phí dịch vụ

1. Trong module `Tính phí`, mở tab đồng hồ/chỉ số nếu có.
2. Tìm `MTR-T09-BA-ANPHU-ELEC`, kiểm tra chỉ số tổng 48,000 -> 61,200 và sản lượng 13,200 kWh.
3. Kiểm tra tách khung giờ: bình thường 8,400 kWh, cao điểm 3,200 kWh, thấp điểm 1,600 kWh, COS phi `0.87`.
4. Tìm `MTR-T09-BA-ANPHU-WATER`, kiểm tra sản lượng 620 m3.
5. Tìm `MTR-T09-BA-ANPHU-WASTEWATER`, kiểm tra sản lượng 590 m3.
6. Đối chiếu biểu giá `TRF-T09-BA-ELEC-TOU-2026`, `TRF-T09-BA-WATER-2026`, `TRF-T09-BA-WASTEWATER-2026`, `TRF-T09-BA-WASTE-FIXED-2026`, `TRF-T09-BA-SHARED-FIXED-2026`.
7. Kiểm tra dòng âm `T09-NH-NEG-ELEC` không xuất hiện khi user đang ở phạm vi Bắc An.

Kết quả mong đợi: điện/nước/nước thải/rác/phí dịch vụ tính đúng theo dữ liệu seed; field phụ thuộc như kỳ, đồng hồ, khách hàng, biểu giá dùng select-option có tên dễ hiểu.

### UAT-05.05 Chi phí nhà cung cấp và phải trả

1. Vào `Tài chính` -> tab nhà cung cấp/phải trả/chi phí nếu có.
2. Tìm nhà cung cấp `NCC-T09-DIENLUC-BA`.
3. Mở phải trả `AP-T09-BA-DIEN-2026-09`.
4. Kiểm tra tổng tiền 33,000,000; đã trả 11,000,000; còn lại 22,000,000; trạng thái `partial`.
5. Tạo chi phí UAT mới, field `Nhà cung cấp` chọn từ danh sách, ưu tiên chọn `UAT T09 - Công ty Điện lực Bắc An`.

Kết quả mong đợi: không phải nhập nhà cung cấp tự do; sửa thông tin nhà cung cấp và lưu chi phí không lỗi validate mơ hồ.

## 10. Phiếu yêu cầu, SLA và thông báo

Phần này kiểm tra luồng vận hành sau khi khách hàng gửi phản ánh: tiếp nhận ticket, cập nhật trạng thái, ghi comment, theo dõi SLA và phát sinh thông báo.

### UAT-06.01 Kiểm tra ticket seed

1. Vào `Phiếu yêu cầu & phản ánh`.
2. Lọc theo `Khu công nghiệp Trà Nóc`.
3. Tìm `TCK-TN-MEKONG-WATER-001`.
4. Mở chi tiết, kiểm tra trạng thái, ưu tiên, SLA, bình luận.

Kết quả mong đợi: ticket có timeline/bình luận, liên kết đúng khách hàng và hạ tầng cấp nước.

### UAT-06.02 Tạo ticket mới

1. Bấm `Tạo phiếu`.
2. Chọn khách hàng `TN-CUST-001` hoặc `UAT-CUS-20260916-01`.
3. Chọn loại phản ánh nước/hạ tầng.
4. Gán ưu tiên, phòng ban xử lý nếu có.
5. Lưu và chuyển trạng thái.

Kết quả mong đợi: ticket tạo được, SLA/audit cập nhật theo thao tác.

### UAT-06.03 Thông báo

1. Vào `Thông báo`.
2. Kiểm tra inbox/log cho `uat.tranoc.park.admin@ipms.local` hoặc root admin.
3. Tìm tiêu đề `Trà Nóc - Công nợ tháng 09 cần theo dõi`.
4. Tạo thông báo thủ công nếu UI hỗ trợ.

Kết quả mong đợi: thông báo hiển thị, nội dung không lộ secret/token. Provider email/SMS thật có thể `Blocked` nếu chưa cấu hình.

## 11. E-invoice, chữ ký số và provider boundary

Phần này chỉ kiểm tra trạng thái, hàng đợi, log và dữ liệu boundary. Không kết luận tích hợp live provider thành công nếu môi trường chưa có cấu hình chính thức.

### UAT-07.01 E-invoice boundary

1. Vào module eInvoice/MISA nếu có trong menu.
2. Tìm `EINV-BA-2026-08-001` để kiểm tra hóa đơn đã phát hành mẫu.
3. Tìm `EINV-T11-BA-BOUNDARY-DRAFT`, `EINV-T11-BA-BOUNDARY-APPROVED`, `EINV-T11-BA-DUPLICATE-BLOCKED`, `EINV-T11-NH-NEG-SCOPE`.
4. Tạo yêu cầu hóa đơn từ `FN-T09-BA-ANPHU-2026-09` hoặc một khoản phải thu nếu UI hỗ trợ.
5. Thử bước phát hành/đồng bộ provider.

Kết quả mong đợi: hệ thống không gọi live MISA khi chưa cấu hình; log provider boundary rõ ràng, không fake thành công live provider.

### UAT-07.02 Chữ ký số boundary

1. Vào module chữ ký số.
2. Tìm `SIGN-BA-HD-LEASE-001` để kiểm tra chứng từ đã ký mẫu.
3. Tìm `SIGN-T12-BA-CONTRACT-READY`, `SIGN-T12-BA-APPENDIX-SENT`, `SIGN-T12-BA-CONTRACT-FAILED`, `SIGN-T12-FOREIGN-SCOPE-BLOCKED`.
4. Tạo yêu cầu ký từ tài liệu hợp đồng nếu UI hỗ trợ.
5. Kiểm tra queue/timeline/log.

Kết quả mong đợi: yêu cầu ký vào queue/trạng thái nội bộ; live provider/certificate nằm ngoài phạm vi nếu chưa có cấu hình chính thức.

## 12. Báo cáo và xuất file

Phần này kiểm tra tính nhất quán giữa dữ liệu nguồn, số liệu tổng hợp, file export và lịch sử xuất file.

### UAT-08.01 Báo cáo tổng quan

1. Vào `Báo cáo & phân tích`.
2. Kiểm tra danh mục báo cáo có `RPT_CUSTOMER_CONTRACTS`, `RPT_UTILITY_CONSUMPTION`, `RPT_BILLING_FINANCE`, `RPT_SUPPLIER_DEBT`, `RPT_INFRASTRUCTURE_OPERATIONS`, `RPT_TICKET_SLA`.
3. Chọn/lọc `Khu công nghiệp Trà Nóc` để đối chiếu 5 doanh nghiệp, 5 hợp đồng, 5 khoản phải thu `REC-TN-*`.
4. Chọn/lọc bộ Bắc An/T09 để đối chiếu billing/utility/provider theo các mã ở mục 3.7-3.14.

Kết quả mong đợi: số liệu báo cáo không mâu thuẫn với danh sách nguồn.

### UAT-08.02 Xuất Excel/PDF

1. Xuất CSV/XLSX ở khách hàng, hợp đồng, tài chính, báo cáo.
2. Mở file tải về.
3. Kiểm tra font tiếng Việt, tiêu đề, cột tiền, cột trạng thái.
4. Kiểm tra export history/audit có các file seed `bao-cao-cong-no-bac-an-t10.xlsx`, `bao-cao-doi-soat-phieu-thu-t09.xlsx`, `bao-cao-cong-no-nha-cung-cap-t09.xlsx`, `bao-cao-sla-ticket-bac-an-t10.pdf`.

Kết quả mong đợi: file đọc được, không lỗi font tiếng Việt; PDF chính thức có thể `Blocked by official template`.

## 13. Phân quyền và audit

Phần này xác nhận người dùng chỉ nhìn thấy dữ liệu đúng phạm vi park/doanh nghiệp, đồng thời các hành động nhạy cảm có audit đầy đủ.

### UAT-09.01 User park admin Trà Nóc

1. Đăng xuất root admin.
2. Đăng nhập `uat.tranoc.park.admin@ipms.local`.
3. Kiểm tra chỉ thấy dữ liệu trong `Khu công nghiệp Trà Nóc`.
4. Thử mở dữ liệu khu khác bằng URL trực tiếp nếu có mã.
5. Kiểm tra menu quản trị hệ thống chỉ hiện khi role có quyền admin phù hợp.

Kết quả mong đợi: dữ liệu ngoài phạm vi bị chặn hoặc không hiển thị.

### UAT-09.02 User quản trị doanh nghiệp Mekong Xanh

1. Đăng nhập `uat.tranoc.mekongxanh@ipms.local`.
2. Kiểm tra chỉ thấy dữ liệu liên quan `TN-CUST-001`.
3. Thử tìm `TN-CUST-003` hoặc `TN-CON-FACT-001`.
4. Thử mở `/admin` và `/admin/users/new` bằng URL trực tiếp.

Kết quả mong đợi: tenant không thấy dữ liệu doanh nghiệp khác và không vào được chức năng quản trị người dùng.

### UAT-09.03 User doanh nghiệp Sông Hậu

1. Đăng nhập `uat.tranoc.songhau@ipms.local`.
2. Kiểm tra chỉ thấy dữ liệu liên quan `TN-CUST-003`.
3. Thử mở `TN-CUST-001`, `TN-CON-LAND-001`, `/customers/new` hoặc màn hình sửa khách hàng bằng URL trực tiếp.

Kết quả mong đợi: dữ liệu ngoài doanh nghiệp bị chặn hoặc trả không tìm thấy; nút tạo/sửa/lưu trữ khách hàng không hiển thị.

### UAT-09.04 Audit hành động nhạy cảm

1. Đăng nhập lại root admin.
2. Vào `Quản trị hệ thống` -> `Nhật ký` hoặc audit.
3. Lọc các action: login, tạo user, tạo khách hàng, tạo hợp đồng, thanh toán, export.

Kết quả mong đợi: audit có actor, action, entity, timestamp; không lộ mật khẩu/token/API key.

## 14. Điều kiện sign-off UAT

### 14.1 Điều kiện đủ để đề xuất ký UAT

QA/Test Lead chỉ đề xuất ký UAT khi các điều kiện sau được đáp ứng:

1. Toàn bộ case bắt buộc trong tài liệu này đã có kết quả và bằng chứng.
2. Không còn lỗi `Blocker` hoặc `High` chưa có phương án xử lý/được chấp nhận rủi ro bằng văn bản.
3. Các case liên quan tiền, công nợ, phân quyền và dữ liệu liên module đã được đại diện nghiệp vụ xác nhận.
4. Các nội dung `Blocked` đều có lý do rõ ràng, chủ sở hữu xử lý và quyết định đưa vào phase sau hoặc điều kiện go-live.
5. Danh sách dữ liệu `UAT-*` do tester tạo đã được tổng hợp để phục vụ dọn dữ liệu hoặc giữ lại làm bằng chứng.
6. Biên bản tổng hợp UAT đã đính kèm ảnh màn hình, file export, defect log và quyết định xử lý defect.

### 14.2 Mẫu bảng tổng hợp sign-off

| Hạng mục | Kết quả | Người xác nhận | Ngày xác nhận | Ghi chú |
| --- | --- | --- | --- | --- |
| Smoke/Auth/Dashboard |  |  |  |  |
| Quản trị hệ thống và phân quyền |  |  |  |  |
| Khách hàng và hồ sơ |  |  |  |  |
| Hợp đồng và tài liệu |  |  |  |  |
| Hạ tầng, tài sản và GIS |  |  |  |  |
| Tính phí, tài chính và công nợ |  |  |  |  |
| Phiếu yêu cầu, SLA và thông báo |  |  |  |  |
| E-invoice/chữ ký số boundary |  |  |  |  |
| Báo cáo/export |  |  |  |  |
| Defect còn mở được chấp nhận |  |  |  |  |

### 14.3 Phê duyệt lưu hành

| Vai trò | Họ tên | Chữ ký/Xác nhận | Ngày |
| --- | --- | --- | --- |
| BA Owner |  |  |  |
| QA/Test Lead |  |  |  |
| PM/PO |  |  |  |
| Đại diện nghiệp vụ khách hàng |  |  |  |

Tài liệu có hiệu lực cho vòng UAT web MVP kể từ ngày ban hành ở mục `Kiểm soát tài liệu`. Mọi thay đổi sau khi phát hành phải được ghi nhận trong lịch sử thay đổi và được QA/Test Lead xác nhận trước khi sử dụng lại cho tester.
