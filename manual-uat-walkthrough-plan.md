# Manual UAT Walkthrough Plan

Ngày cập nhật: 2026-09-11

Mục tiêu: tài liệu này dùng cho tester mới hoàn toàn chưa nắm hệ thống, chỉ cần tài khoản root admin để đăng nhập và đi tuần tự qua các module chính. Tester vừa kiểm tra dữ liệu seed chuẩn, vừa tạo thêm dữ liệu UAT từ giao diện để hiểu luồng nghiệp vụ tổng quan của dự án.

Căn cứ: `documents/requiment.xlsx`, `documents/production-acceptance/uat-requirement-matrix.md`, các test plan T01-T12 trong `documents/production-acceptance/`, source hiện tại `apps/` + `database/`, và seed DB đã rebuild tới migration `V202609110002__uat_authorization_scope_hardening.sql`.

Ngoài phạm vi tài liệu này: mobile native, live MISA/eInvoice, live provider chữ ký số, provider SMS/push thật, HTTPS production, backup/DR production, kiểm thử tải 200 user đồng thời. Các mục này cần biên bản kiểm thử riêng khi có môi trường/provider chính thức.

## 1. Môi trường và tài khoản

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

Ghi chú: nếu môi trường test đổi mật khẩu seed khi rebuild, tester ghi `Blocked by credential` và báo lại master plan để xác nhận hash/mật khẩu đang dùng.

## 2. Nguyên tắc ghi nhận kết quả

Mỗi case cần ghi lại:

| Trường | Cách ghi |
| --- | --- |
| Mã case | Ví dụ `UAT-02.03` |
| Tài khoản | Email đang đăng nhập |
| Dữ liệu dùng | Mã khu, mã doanh nghiệp, mã hợp đồng, mã ticket |
| Kết quả | `Pass`, `Fail`, `Blocked`, `Out of scope` |
| Bằng chứng | Ảnh màn hình, file export, mã bản ghi, log/audit |
| Ghi chú | Lỗi, API status, payload, thao tác tái hiện |

Quy ước:

| Kết quả | Ý nghĩa |
| --- | --- |
| `Pass` | Thao tác thành công, dữ liệu hiển thị đúng ở màn hình liên quan |
| `Fail` | Lỗi hệ thống, dữ liệu sai, trang trắng, lỗi quyền không hợp lý |
| `Blocked` | Thiếu tài khoản, provider, template chính thức, môi trường hoặc dữ liệu ngoài phạm vi |
| `Out of scope` | Không thuộc phạm vi web MVP/manual UAT hiện tại |

Tiền tố dữ liệu tester tự tạo: dùng `UAT-YYYYMMDD-...` để dễ lọc và dọn sau test. Không sửa/xóa dữ liệu seed `TN-*` trừ khi case yêu cầu kiểm tra chỉnh sửa có chủ đích.

## 3. Bộ dữ liệu seed chuẩn

Tester ưu tiên dùng `Khu công nghiệp Trà Nóc` để đi luồng đọc/tra cứu/xuất báo cáo. Khi cần kiểm tra tạo mới, tạo thêm bản ghi `UAT-*` riêng.

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

Thông báo seed:

| Người nhận | Tiêu đề |
| --- | --- |
| `uat.tranoc.park.admin@ipms.local` | Trà Nóc - Công nợ tháng 09 cần theo dõi |
| `uat.tranoc.mekongxanh@ipms.local` | Trà Nóc - Phiếu nước đang xử lý |

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

## 4. Smoke test trước khi đi UAT

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

### UAT-01.01 Kiểm tra dữ liệu nền khu/cụm/phòng ban

1. Vào `Quản trị hệ thống`.
2. Mở tab dữ liệu nền hoặc danh mục.
3. Tìm `KCN-TRA-NOC`, `TRA-NOC-A`, `TRA-NOC-B`.
4. Mở danh mục `Phòng ban`.
5. Tạo phòng ban mới:
   - Mã: `UAT-DEPT-20260911-01`
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
   - Email: `uat.operator.20260911@ipms.local`
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

### UAT-02.01 Kiểm tra danh sách seed

1. Vào `Quản lý khách hàng`.
2. Chọn/lọc khu `Khu công nghiệp Trà Nóc`.
3. Tìm từng mã `TN-CUST-001` đến `TN-CUST-005`.
4. Mở chi tiết `TN-CUST-001`.

Kết quả mong đợi: đủ 5 doanh nghiệp seed, mở chi tiết không trang trắng.

### UAT-02.02 Tạo doanh nghiệp mới

1. Bấm `Thêm khách hàng`.
2. Nhập:
   - Mã khách hàng: `UAT-CUS-20260911-01`
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

### UAT-06.01 Kiểm tra ticket seed

1. Vào `Phiếu yêu cầu & phản ánh`.
2. Lọc theo `Khu công nghiệp Trà Nóc`.
3. Tìm `TCK-TN-MEKONG-WATER-001`.
4. Mở chi tiết, kiểm tra trạng thái, ưu tiên, SLA, bình luận.

Kết quả mong đợi: ticket có timeline/bình luận, liên kết đúng khách hàng và hạ tầng cấp nước.

### UAT-06.02 Tạo ticket mới

1. Bấm `Tạo phiếu`.
2. Chọn khách hàng `TN-CUST-001` hoặc `UAT-CUS-20260911-01`.
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

### UAT-08.01 Báo cáo tổng quan

1. Vào `Báo cáo & phân tích`.
2. Kiểm tra danh mục báo cáo có `RPT_CUSTOMER_CONTRACTS`, `RPT_UTILITY_CONSUMPTION`, `RPT_BILLING_FINANCE`, `RPT_SUPPLIER_DEBT`, `RPT_INFRASTRUCTURE_OPERATIONS`, `RPT_TICKET_SLA`.
3. Chọn/lọc `Khu công nghiệp Trà Nóc` để đối chiếu 5 doanh nghiệp, 5 hợp đồng, 5 khoản phải thu `REC-TN-*`.
4. Chọn/lọc bộ Bắc An/T09 để đối chiếu billing/utility/provider theo các mã ở mục 3.7-3.13.

Kết quả mong đợi: số liệu báo cáo không mâu thuẫn với danh sách nguồn.

### UAT-08.02 Xuất Excel/PDF

1. Xuất CSV/XLSX ở khách hàng, hợp đồng, tài chính, báo cáo.
2. Mở file tải về.
3. Kiểm tra font tiếng Việt, tiêu đề, cột tiền, cột trạng thái.
4. Kiểm tra export history/audit có các file seed `bao-cao-cong-no-bac-an-t10.xlsx`, `bao-cao-doi-soat-phieu-thu-t09.xlsx`, `bao-cao-cong-no-nha-cung-cap-t09.xlsx`, `bao-cao-sla-ticket-bac-an-t10.pdf`.

Kết quả mong đợi: file đọc được, không lỗi font tiếng Việt; PDF chính thức có thể `Blocked by official template`.

## 13. Phân quyền và audit

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

## 14. Thứ tự test khuyến nghị

| Thứ tự | Nhóm test | Module | Dữ liệu chính |
| ---: | --- | --- | --- |
| 1 | Smoke | Auth/Dashboard | `admin@ipms.local` |
| 2 | Quản trị | System Management | `KCN-TRA-NOC`, tạo `UAT-DEPT-*`, tạo user |
| 3 | Khách hàng | Customers | `TN-CUST-001` đến `TN-CUST-005`, tạo `UAT-CUS-*` |
| 4 | Hợp đồng | Contracts | `TN-CON-*` |
| 5 | Hạ tầng/GIS | Infrastructure, Assets, GIS | `TN-LOT-*`, `TN-FAC-*`, `TN-INF-*`, `AST-T07-*`, `GIS-T07-*` |
| 6 | Tài chính/Billing | Finance, Billing | `REC-TN-*`, `PAY-TN-*`, `BP-T09-*`, `TRF-T09-*`, `MTR-T09-*`, `FN-T09-*`, `AP-T09-*` |
| 7 | Ticket/SLA | Tickets | `TCK-TN-*` |
| 8 | Thông báo | Notifications | `Trà Nóc - ...` |
| 9 | Provider boundary | eInvoice/eSign | `EINV-T11-*`, `SIGN-T12-*` |
| 10 | Báo cáo/export | Reports | `RPT_*`, file export seed |
| 11 | Phân quyền/audit | Auth/Admin | park admin, tenant users |

## 15. Mẫu report bug cho tester

Khi gặp lỗi, tester gửi report theo mẫu:

```text
Module:
Case:
Tài khoản:
URL:
Dữ liệu đang dùng:
Các bước tái hiện:
Kết quả thực tế:
Kết quả mong đợi:
API lỗi nếu thấy trên Network:
Ảnh/video bằng chứng:
Mức độ ảnh hưởng: Blocker / High / Medium / Low
```

Quy tắc ưu tiên:

| Mức | Khi nào dùng |
| --- | --- |
| `Blocker` | Không đăng nhập được, không vào được hệ thống, không thể đi tiếp luồng chính |
| `High` | Không tạo/sửa được dữ liệu chính, sai phân quyền, sai tiền/công nợ |
| `Medium` | Lỗi validate, lỗi layout ảnh hưởng thao tác, export thiếu cột |
| `Low` | Chính tả, spacing nhỏ, thông điệp chưa mượt nhưng không chặn test |

## 16. Tiêu chí kết thúc manual UAT

Manual UAT web MVP được coi là hoàn thành khi:

1. Các case `UAT-00` đến `UAT-09` có kết quả `Pass`, `Blocked` hợp lệ hoặc `Out of scope` được xác nhận.
2. Tester xác nhận seed Trà Nóc có đủ: 1 khu, 2 cụm, 5 doanh nghiệp, 5 hồ sơ khách hàng, 5 lô đất, 2 nhà xưởng, 2 tài sản hạ tầng, 5 hợp đồng, 5 khoản phải thu, 2 thanh toán, 3 ticket, 2 thông báo.
3. Có ít nhất một dữ liệu `UAT-*` được tạo từ UI cho các module chính: quản trị, khách hàng, hợp đồng, ticket hoặc tài chính.
4. Luồng dữ liệu liên module đọc được: khách hàng -> lô đất/nhà xưởng -> hợp đồng -> lịch thanh toán/phải thu -> thanh toán -> báo cáo.
5. Các file hồ sơ/hợp đồng/export tải được hoặc có lý do `Blocked` rõ ràng.
6. Phân quyền root/park admin/tenant được kiểm tra bằng cả menu và truy cập trực tiếp URL.
7. Các lỗi còn lại đã có bug report đủ bước tái hiện để master plan điều phối fix.

## 17. Các điểm cần chốt với khách hàng

| Nhóm | Cần chốt trước sign-off |
| --- | --- |
| Billing | Công thức điện/ nước/ nước thải/ rác/ tiện ích, COS phi, VAT, làm tròn |
| Mẫu biểu | Giấy báo phí, hóa đơn, hợp đồng, phụ lục, báo cáo PDF/XLSX |
| Provider | SMTP/SMS/push, MISA meInvoice, chữ ký số, callback, credential policy |
| Hạ tầng | HTTPS, domain, backup/restore, monitoring, DR, tải đồng thời |
| Mobile | Tách phase mobile native hay chấp nhận tenant portal web trong phase MVP |
| Dữ liệu thật | Danh mục khu/cụm/lô/khách hàng/hợp đồng thật để import trước UAT chính thức |
