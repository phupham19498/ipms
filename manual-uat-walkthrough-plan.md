# Tài liệu Walkthrough UAT Thủ công IPMS

| Thuộc tính | Nội dung |
| --- | --- |
| Mã tài liệu | `IPMS-UAT-WALKTHROUGH` |
| Phiên bản | `1.7` |
| Trạng thái | Lưu hành chính thức cho vòng UAT web MVP |
| Ngày ban hành | 2026-09-17 |
| Chủ sở hữu | Business Analyst / QA Coordination |
| Đối tượng sử dụng | BA, QA, tester UAT, đại diện nghiệp vụ khách hàng, đội triển khai |
| Phạm vi hệ thống | IPMS web MVP, API native, dữ liệu seed nhập tay phục vụ UAT |
| Nguồn seed chuẩn | `documents/uat-manual-seed-data.md` — bộ `UAT-KCN-01 / Công ty cổ phần IIT` |
| Mức bảo mật | Nội bộ dự án; không chuyển tiếp ra ngoài phạm vi UAT nếu chưa được PM/PO phê duyệt |

> **Cách đưa lên Notion:** vào Notion → Settings → Import → Markdown rồi chọn file này (giữ nguyên bảng, code, heading), hoặc copy-paste toàn bộ vào một trang trắng. Tài liệu dài (~1200 dòng) nên có thể tách thành 1 trang mẹ + từng mục 0–15 thành subpage. Sau khi import: xóa mục `Mục lục` bên dưới và chèn block Table of Contents của Notion; hai bảng ở mục 14.2/14.3 có thể chuyển thành Board view để tick theo dõi.

## Mục lục

- 0. Quản lý tài liệu
- 1. Điều kiện chuẩn bị UAT
- 2. Nguyên tắc thực hiện và ghi nhận kết quả
- 3. Bộ dữ liệu seed chuẩn và cách đối chiếu
- 4. Kiểm tra khởi động phiên UAT (UAT-00)
- 5. Quản trị hệ thống (UAT-01)
- 6. Quản lý khách hàng (UAT-02)
- 7. Hợp đồng (UAT-03)
- 8. Hạ tầng, tài sản và GIS (UAT-04)
- 9. Tính phí và tài chính (UAT-05)
- 10. Phiếu yêu cầu, SLA và thông báo (UAT-06)
- 11. E-invoice, chữ ký số và provider boundary (UAT-07)
- 12. Báo cáo và xuất file (UAT-08)
- 13. Phân quyền và audit (UAT-09)
- 14. Điều kiện sign-off UAT
- 15. Cấu hình quyền cho từng vai trò
- Phụ lục A. Đối chiếu bản 1.1 → 1.2 (lịch sử)
- Phụ lục B. Công thức tính điện, nước, nước thải, rác, dịch vụ hạ tầng, tiền thuê

---

## 0. Quản lý tài liệu

### 0.1 Phạm vi áp dụng

Tài liệu áp dụng cho các phân hệ web MVP sau:

| Nhóm | Phân hệ |
| --- | --- |
| Nền tảng | Đăng nhập, dashboard, điều hướng, phân quyền, audit |
| Nghiệp vụ lõi | Khách hàng, hợp đồng, lô đất/GIS, tài sản, hạ tầng |
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
3. Khi có migration, seed tay hoặc quyền truy cập thay đổi, QA/Test Lead phải xác nhận lại phần `Môi trường và tài khoản` trước khi phát hành cho tester.
4. Bộ seed chuẩn duy nhất cho walkthrough này là `documents/uat-manual-seed-data.md`. Mọi mã `TN-*`, `KCN-TRA-NOC`, `IPMS-PARK-01/Bắc An`, `T07/T08/T09/T11/T12` của bản 1.1 không còn là seed chuẩn; nếu cần đối chiếu hồi quy thì ghi rõ là dữ liệu ngoài phạm vi bản 1.2.
5. Tài liệu này không thay thế release gate, security checklist, performance checklist hoặc biên bản nghiệm thu chính thức.

### 0.4 Lịch sử thay đổi

| Phiên bản | Ngày | Người thực hiện | Nội dung |
| --- | --- | --- | --- |
| 1.7 | 2026-09-17 | BA / QA | Chuẩn hóa trình bày thân thiện Notion: thêm hướng dẫn import + Mục lục, đường kẻ phân cách các phần lớn, giữ nguyên nội dung nghiệp vụ. |
| 1.6 | 2026-09-17 | BA / QA | Viết lại luồng sử dụng Tài chính công nợ cho người không chuyên: giải thích phải thu/thanh toán/phân bổ/chưa phân bổ/điều chỉnh/khóa kỳ/NCC theo đúng tab UI, kèm ví dụ IIT từng bước. |
| 1.5 | 2026-09-17 | BA / QA | Bổ sung Phụ lục B công thức tính điện/nước/nước thải/rác/dịch vụ hạ tầng/tiền thuê đầy đủ kèm chú thích, ví dụ số IIT và cách đối chiếu khi số live khác số seed. |
| 1.4 | 2026-09-17 | BA / QA | Bổ sung giải thích tính năng chi tiết cho từng phân hệ (UAT-00 đến UAT-09): tính năng là gì, hoạt động thế nào, liên quan dữ liệu nào trong bộ `UAT-KCN-01 / IIT`, ví dụ và lỗi hay gặp để tester không kỹ thuật vẫn hiểu. |
| 1.3 | 2026-09-17 | BA / QA | Bổ sung mục 15 cấu hình quyền chi tiết cho người không kỹ thuật (5 vai trò, phạm vi, ma trận xem/làm, cách tạo user, cách kiểm tra và lỗi hay gặp với bộ `UAT-KCN-01 / IIT`). |
| 1.2 | 2026-09-17 | BA / QA | Chuyển toàn bộ seed chuẩn sang `uat-manual-seed-data.md` (bộ `UAT-KCN-01 / IIT`); loại bỏ phụ thuộc seed Trà Nóc/Bắc An; cập nhật mọi case UAT-00 đến UAT-09 theo mã UAT tay. |
| 1.1 | 2026-09-16 | BA / Codex | Sắp xếp lại cấu trúc trình bày, bổ sung luồng nghiệp vụ, ma trận dữ liệu seed Trà Nóc/Bắc An và chuẩn hóa cách dùng tài liệu cho các bên triển khai. |
| 1.0 | 2026-09-16 | BA / Codex | Chuẩn hóa thành tài liệu lưu hành chính thức, bổ sung kiểm soát tài liệu, phạm vi, vai trò, điều kiện UAT và sign-off. |

### 0.5 Cấu trúc sử dụng tài liệu

| Phần | Nội dung | Người đọc chính | Cách sử dụng |
| --- | --- | --- | --- |
| 1 | Điều kiện chuẩn bị UAT | PM/PO, BA, QA/Test Lead | Xác nhận môi trường, tài khoản và dữ liệu nhập tay trước khi mở phiên UAT |
| 2 | Nguyên tắc ghi nhận kết quả | QA/Test Lead, tester UAT | Thống nhất cách ghi trạng thái, bằng chứng và dữ liệu tester tự tạo |
| 3 | Bộ dữ liệu seed chuẩn (từ `uat-manual-seed-data.md`) | BA, QA, tester, đại diện nghiệp vụ | Tra mã dữ liệu khi thực hiện case hoặc đối chiếu lỗi |
| 4 đến 13 | Walkthrough nghiệp vụ + giải thích tính năng | Tester UAT, BA, đại diện nghiệp vụ | Đọc phần `Tính năng này là gì` ở đầu mỗi mục để hiểu cách hoạt động, sau đó thực hiện từng case theo màn hình/phân hệ |
| 14 | Điều kiện sign-off UAT | PM/PO, QA/Test Lead, đại diện nghiệp vụ | Tổng hợp kết quả và xác nhận điều kiện ký UAT |
| 15 | Cấu hình quyền cho từng vai trò (không cần kỹ thuật) | Đại diện nghiệp vụ, tester UAT, QA | Hiểu mỗi vai trò được thấy/làm gì, cách tạo user đúng, cách tự kiểm tra bằng bộ `UAT-KCN-01 / IIT` |
| Mục lục | Điều hướng nhanh toàn tài liệu | Mọi độc giả | Tìm đúng mục cần đọc; khi lên Notion thì xóa mục này và chèn block Table of Contents |
| Phụ lục B | Công thức tính điện/nước/nước thải/rác/dịch vụ/tiền thuê | Tester UAT, đại diện nghiệp vụ, QA | Tra công thức + chú thích + ví dụ số IIT khi test UAT-05; khi số live khác số seed thì đối chiếu theo phụ lục này |

### 0.6 Luồng nghiệp vụ tổng quát

Tài liệu được sắp theo chuỗi nghiệp vụ nhập tay trong `uat-manual-seed-data.md`:

1. Chuẩn bị môi trường, tài khoản và nhập seed tay KCN UAT.
2. Kiểm tra nền tảng đăng nhập, dashboard, điều hướng và phân quyền.
3. Quản lý dữ liệu nền KCN/cụm, GIS/lô đất, hạ tầng, tài sản, user và phạm vi truy cập.
4. Quản lý khách hàng IIT và hồ sơ pháp lý.
5. Quản lý hợp đồng (thuê đất, thuê hạ tầng, dịch vụ điện/nước/rác), line item và điều khoản tài chính.
6. Tính phí kỳ `UAT-KTP-T09`: biểu giá, đồng hồ, chỉ số, rác thải, run tính phí, thông báo phí.
7. Phải thu, thanh toán và phân bổ công nợ IIT.
8. Tiếp nhận phiếu yêu cầu, xử lý SLA, gửi thông báo và ghi nhận audit (tester tự tạo vì seed tay chưa có sẵn).
9. Kiểm tra báo cáo, export file, eInvoice và chữ ký số ở mức provider boundary (tạo từ dữ liệu IIT).
10. Tổng hợp bằng chứng, defect còn mở và điều kiện sign-off.

---

## 1. Điều kiện chuẩn bị UAT

### 1.1 Điều kiện bắt buộc trước khi test

| Điều kiện | Tiêu chí chấp nhận |
| --- | --- |
| Môi trường web/API | Web và API khởi động ổn định, không lỗi cấu hình nền ngay sau đăng nhập |
| Database | DB sạch hoặc đã migrate nền; không yêu cầu migration Trà Nóc/Bắc An (`V202609110001/0002`) nữa |
| Seed UAT nhập tay | Đã nhập tay xong theo `documents/uat-manual-seed-data.md`: KCN `UAT-KCN-01`, cụm `UAT-LO-A/B`, GIS + lô `UAT-KCN-KD-01`, hạ tầng `UAT-KCN-HT-01`, tài sản `UAT_ASSET1`, khách `UAT_KCN-01`, hợp đồng `UAT-HD-*`, kỳ `UAT-KTP-T09`, biểu giá `BG-*-UAT-001`, đồng hồ `DH-*-UAT-001` |
| Tài khoản | Tài khoản root đăng nhập được; các user phân quyền do tester/root tự tạo theo mục 1.2 đăng nhập được |
| Browser | Dùng Chrome hoặc Edge bản ổn định; bật DevTools Network khi cần ghi lỗi API |
| Bằng chứng | Tester có thư mục/biểu mẫu lưu ảnh màn hình, file export, log lỗi và defect log |

Nếu một điều kiện bắt buộc chưa sẵn sàng, QA/Test Lead ghi trạng thái `Blocked by environment` hoặc `Blocked by credential` trước khi bắt đầu walkthrough. Nếu seed tay chưa nhập đủ (ví dụ thiếu đồng hồ hoặc kỳ tính phí), ghi `Blocked by seed` + nêu mã còn thiếu.

### 1.2 Môi trường và tài khoản

| Hạng mục | Giá trị |
| --- | --- |
| Web local | `http://localhost:8000` |
| API local | `http://localhost:3000/api/v1` |
| Swagger | `http://localhost:3000/api/docs` |
| Root admin | `admin@ipms.local` |
| Mật khẩu root admin | `Admin@123456` |
| Seed chuẩn cho manual test | `Khu công nghiệp UAT` |
| Mã KCN seed tay | `UAT-KCN-01` |
| Nguồn seed | `documents/uat-manual-seed-data.md` (nhập tay qua UI, không dùng SQL Trà Nóc/Bắc An) |

Tài khoản dùng để kiểm tra phân quyền (do root tạo trên UI, không còn dùng tài khoản `uat.tranoc.*`):

| Email quy ước | Vai trò | Phạm vi | Mật khẩu quy ước |
| --- | --- | --- | --- |
| `admin@ipms.local` | `ROOT_ADMIN` | Toàn hệ thống | `Admin@123456` |
| `uat.operator.kcn01@ipms.local` | `PARK_ADMIN` / vận hành khu | Khu công nghiệp UAT (`UAT-KCN-01`) | `Demo@123456` |
| `uat.iit.admin@ipms.local` | `ENTERPRISE_ADMIN` | Công ty cổ phần IIT (`UAT_KCN-01`) | `Demo@123456` |
| `uat.iit.user@ipms.local` | `ENTERPRISE_USER` | Công ty cổ phần IIT (`UAT_KCN-01`) | `Demo@123456` |

Ghi chú: nếu môi trường test đổi mật khẩu khi rebuild, tester ghi `Blocked by credential` và báo lại QA/Test Lead để xác nhận hash/mật khẩu đang dùng. Các email trên là quy ước đặt khi tạo user; nếu hệ thống đã có user khác tương đương phạm vi thì dùng user thực tế và ghi rõ email thực tế vào kết quả case.

---

## 2. Nguyên tắc thực hiện và ghi nhận kết quả

### 2.1 Quy chuẩn ghi nhận case

Mỗi case cần ghi lại tối thiểu các thông tin sau. Trường `Bằng chứng` là bắt buộc đối với mọi case `Fail`, `Blocked` và các case nghiệp vụ tiền/công nợ/phân quyền dù kết quả là `Pass`.

| Trường | Cách ghi |
| --- | --- |
| Mã case | Ví dụ `UAT-02.03` |
| Tài khoản | Email đang đăng nhập |
| Dữ liệu dùng | Mã KCN, mã khách, mã hợp đồng, mã kỳ, mã đồng hồ, mã thông báo phí/phải thu |
| Kết quả | `Pass`, `Fail`, `Blocked`, `Out of scope` |
| Bằng chứng | Ảnh màn hình, file export, mã bản ghi, log/audit |
| Ghi chú | Lỗi, API status, payload, thao tác tái hiện |

### 2.2 Quy ước trạng thái

| Kết quả | Ý nghĩa |
| --- | --- |
| `Pass` | Thao tác thành công, dữ liệu hiển thị đúng ở màn hình liên quan |
| `Fail` | Lỗi hệ thống, dữ liệu sai, trang trắng, lỗi quyền không hợp lý |
| `Blocked` | Thiếu tài khoản, provider, template chính thức, môi trường hoặc seed tay chưa nhập đủ |
| `Out of scope` | Không thuộc phạm vi web MVP/manual UAT hiện tại |

### 2.3 Quy tắc bằng chứng chính thức

1. Ảnh màn hình phải thể hiện URL hoặc tiêu đề màn hình, dữ liệu chính và trạng thái thao tác.
2. File export phải lưu nguyên tên file tải xuống; nếu đổi tên để lưu trữ, giữ lại tên gốc trong ghi chú.
3. Lỗi API cần ghi HTTP status, endpoint, thời điểm test và thông điệp lỗi hiển thị cho người dùng.
4. Không chụp/lưu mật khẩu, token, cookie, private key hoặc credential provider thật trong bằng chứng.
5. Một case chỉ được ký `Pass` khi cả thao tác chính, dữ liệu liên quan và phân quyền hiển thị đều đúng theo kết quả mong đợi.

### 2.4 Quy tắc dữ liệu tester tự tạo

Tiền tố dữ liệu tester tự tạo: dùng `UAT-YYYYMMDD-...` để dễ lọc và dọn sau test. Không sửa/xóa seed tay `UAT-KCN-01`, `UAT_KCN-01`, `UAT-KCN-KD-01`, `UAT-HD-*`, `UAT-KTP-T09`, `DH-*-UAT-001`, `TBP/REC-UAT-*` trừ khi case yêu cầu kiểm tra chỉnh sửa có chủ đích.

Khi một case yêu cầu tạo mới nhưng màn hình chưa có đủ chức năng, tester ghi `Blocked`, nêu rõ màn hình/field bị thiếu và tiếp tục các bước đọc/đối chiếu bằng dữ liệu seed tay nếu còn thực hiện được.

---

## 3. Bộ dữ liệu seed chuẩn và cách đối chiếu

Nguồn duy nhất: `documents/uat-manual-seed-data.md`. Tester nhập tay theo đúng thứ tự file đó: KCN -> cụm -> GIS/lô đất -> hạ tầng -> tài sản -> khách hàng -> hợp đồng -> kỳ tính phí/biểu giá/đồng hồ/chỉ số/rác -> run tính phí/thông báo phí -> phải thu/thanh toán.

Thứ tự nhập tay bắt buộc để không vỡ FK:

`UAT-KCN-01` -> `UAT-LO-A/B` -> `GIS-UAT-KCN-LOT` + `GIS-UAT-KCN-KD-01` -> `UAT-KCN-KD-01` -> `UAT-KCN-HT-01` -> `UAT_ASSET1` -> `UAT_KCN-01` -> `UAT-HD-*` -> `UAT-KTP-T09` + `BG-*-UAT-001` + `DH-*-UAT-001` + chỉ số -> run `BR-UAT-KTP-T09-001` -> `TBP-UAT-KTP-T09-IIT` -> `REC-UAT-KTP-T09-IIT` -> `PAY-UAT-IIT-2026-10-001`.

### Hướng dẫn đọc bộ dữ liệu

| Nhóm dữ liệu | Mã chính | Khi nào dùng |
| --- | --- | --- |
| Walkthrough nghiệp vụ lõi | `UAT-KCN-01`, `UAT_KCN-01`, `UAT-KCN-KD-01`, `UAT-HD-*`, `UAT-KTP-T09`, `DH-*-UAT-001`, `TBP/REC/PAY-UAT-*` | Dùng cho mọi case UAT-00 đến UAT-09 |
| Dữ liệu tester tự tạo thêm | Tiền tố `UAT-*` (VD `UAT-CUS-20260917-01`, `UAT-DEPT-20260917-01`, `TCK-UAT-*`) | Dùng khi cần tạo mới, không ghi đè mã seed chuẩn |
| Dữ liệu ngoài phạm vi bản 1.2 | `TN-*`, `KCN-TRA-NOC`, `IPMS-PARK-01/Bắc An`, `T07-T12` | Không dùng; chỉ đối chiếu hồi quy khi được yêu cầu riêng |

### 3.1 Khu và cụm

| Loại | Mã | Tên | Trạng thái |
| --- | --- | --- | --- |
| Khu công nghiệp | `UAT-KCN-01` | Khu công nghiệp UAT | Đang hoạt động |
| Cụm | `UAT-LO-A` | Lô A | Đang hoạt động |
| Cụm | `UAT-LO-B` | Lô B | Đang hoạt động |

KCN thuộc `UAT-KCN-01`, cụm thuộc KCN tương ứng theo file seed.

### 3.2 Doanh nghiệp

| Mã | Tên doanh nghiệp | MST | Email | Điện thoại | Trạng thái |
| --- | --- | --- | --- | --- | --- |
| `UAT_KCN-01` | Công ty cổ phần IIT | `0319999999` | `finance.iit@example.test` | `0909000001` | Đang hoạt động |

Thông tin pháp lý gợi ý khi form yêu cầu:

| Trường | Giá trị |
| --- | --- |
| Địa chỉ | Lô A, Khu công nghiệp UAT |
| Người đại diện | Nguyễn Văn UAT |
| Chức vụ | Giám đốc |
| Ngành nghề | Sản xuất linh kiện công nghiệp |
| KCN | Khu công nghiệp UAT (`UAT-KCN-01`) |

### 3.3 Hồ sơ khách hàng

Seed tay hiện chưa có file hồ sơ mẫu. Tester tự upload và đối chiếu:

| Doanh nghiệp | Loại hồ sơ gợi ý | Mã gợi ý | File gợi ý |
| --- | --- | --- | --- |
| `UAT_KCN-01` | `business_license` | `UAT-DOC-IIT-BL-01` | File ĐKKD IIT do tester upload |
| `UAT_KCN-01` | `tax_registration` | `UAT-DOC-IIT-TAX-01` | File thuế IIT do tester upload |

Khi kiểm tra download regression, dùng chính file vừa upload để kiểm tra tải lại.

### 3.4 GIS, lô đất, hạ tầng, tài sản

**Lớp bản đồ và feature:**

| Mã lớp | Tên | Loại | Trạng thái |
| --- | --- | --- | --- |
| `GIS-UAT-KCN-LOT` | UAT - Ranh khu đất | `polygon` | Đang hoạt động |

| Mã feature | Tên | Lớp | Liên kết |
| --- | --- | --- | --- |
| `GIS-UAT-KCN-KD-01` | UAT - Ranh khu đất 01 | `GIS-UAT-KCN-LOT` | `UAT-KCN-KD-01` (`land_lot`) |

Geometry chuẩn:

```json
{
  "type": "Polygon",
  "coordinates": [[[106.7044, 10.8024],[106.7054, 10.8024],[106.7054, 10.8034],[106.7044, 10.8034],[106.7044, 10.8024]]]
}
```

Properties: `{"entity":"land_lot","status":"available","areaM2":10000}`.

**Lô đất:**

| Mã | Tên | Cụm | Diện tích | Trạng thái | GIS |
| --- | --- | --- | ---: | --- | --- |
| `UAT-KCN-KD-01` | Khu đất 01 | `UAT-LO-A` | 10000 | Sẵn sàng | `GIS-UAT-KCN-KD-01` |

Số lô `SL-01`. Vị trí: lô đất sẽ giao cho công ty IIT sử dụng.

**Hạ tầng và tài sản:**

| Mã | Tên | Loại | Nhóm | Trạng thái vận hành | Mức quan trọng |
| --- | --- | --- | --- | --- | --- |
| `UAT-KCN-HT-01` | Camera giám sát số 123 | Camera | `security` | Bình thường | Trung bình |

Ngày lắp đặt `15/09/2026`, kiểm tra gần nhất `16/09/2026`, bảo trì tiếp theo `30/09/2026`. Thuộc `UAT-KCN-01` / `UAT-LO-A`.

| Mã | Tên | Loại | Tình trạng | Ưu tiên | Nguyên giá / giá trị hiện tại | Liên kết |
| --- | --- | --- | --- | --- | ---: | --- |
| `UAT_ASSET1` | Camera giám sát | `camera_system` | Tốt | Bình thường | 1000000 / 1000000 | `UAT-KCN-HT-01` |

Vị trí `Số 123, đường 123`, serial `123123`, model `123123`, hãng `IMOU`. Ngày lắp `15/09/2026`, bảo trì tiếp theo `30/09/2026`.

### 3.5 Hợp đồng

**Hợp đồng thuê đất cơ bản:**

| Trường | Giá trị |
| --- | --- |
| Mã | `UAT-HD-01` |
| Tên | Hợp đồng thuê đất |
| Loại | Thuê đất |
| Khách | Công ty cổ phần IIT (`UAT_KCN-01`) |
| KCN/cụm | `UAT-KCN-01` / `UAT-LO-A` |
| Ngày ký / hiệu lực / hết hạn | `16/09/2026` / `16/09/2026` / `15/10/2026` |
| Tiền tệ | VND |
| Lô thuê | `UAT-KCN-KD-01` - Khu đất 01 |
| Diện tích / đơn vị / đơn giá | 10000 / m2 / 100 |
| Thành tiền dự kiến | 1000000 |
| Khoản thu / VAT / chu kỳ / đến hạn | Tiền thuê / 10% / Hàng tháng / Ngày 15 |

**Hợp đồng thuê đất dài hạn:**

| Trường | Giá trị |
| --- | --- |
| Mã | `UAT-HD-LAND-2026-001` |
| Tên | Hợp đồng thuê lô đất UAT-KCN-KD-01 |
| Khách / KCN / cụm | `UAT_KCN-01` / `UAT-KCN-01` / `UAT-LO-A` |
| Ngày ký / hiệu lực / hết hạn | `16/09/2026` / `01/10/2026` / `30/09/2027` |
| Dòng thuê | Thuê lô đất `UAT-KCN-KD-01` để sản xuất thử nghiệm, 10000 m2 x 85000 = 850000000 |
| Khoản thu / VAT / chu kỳ / cọc / đến hạn | Tiền thuê đất / 10% / Hàng quý / 1700000000 / Ngày 10 |

**Hợp đồng thuê hạ tầng:**

| Trường | Giá trị |
| --- | --- |
| Mã | `UAT-HD-ASSET-2026-001` |
| Tên | Hợp đồng thuê hạ tầng camera giám sát |
| Loại | Thuê nhà xưởng / tài sản |
| Dòng thuê | `UAT-KCN-HT-01` - Camera giám sát số 123, SL 1 x 3500000 = 3500000 |
| Hiệu lực | `01/10/2026` đến `31/03/2027` |
| Khoản thu / VAT / chu kỳ / cọc / đến hạn | Tiền thuê hạ tầng camera / 10% / Hàng tháng / 7000000 / Ngày 15 |

**Hợp đồng dịch vụ điện/nước/rác:**

| Trường | Giá trị |
| --- | --- |
| Mã | `UAT-HD-SVC-2026-001` |
| Tên | Hợp đồng dịch vụ tiện ích IIT |
| Hiệu lực | `01/10/2026` (dịch vụ có thể để trống ngày kết thúc hoặc `30/09/2027`) |
| Điện | Cung cấp điện sản xuất theo `DH-DIEN-UAT-001`, 6180 kWh x 1850, hàng tháng, VAT 10%, đến hạn ngày 15 |
| Nước | Cấp nước sạch theo `DH-NUOC-UAT-001`, 225 m3 x 12500, VAT 5% |
| Rác | Thu gom rác công nghiệp, 1250 kg x 1500, VAT 5% |
| Tổng trước VAT kỳ vọng | Điện 11433000 + nước 2812500 + rác 1875000 = 16120500 |

### 3.6 Kỳ tính phí, biểu giá, đồng hồ và chỉ số

**Kỳ tính phí:**

| Mã | Tên | KCN | Thời gian | Trạng thái |
| --- | --- | --- | --- | --- |
| `UAT-KTP-T09` | Kỳ tính phí tháng 09 | `UAT-KCN-01` | 01/09/2026-30/09/2026 | Đã rà soát |

**Biểu giá:**

| Mã | Tên | Loại | Đơn vị | VAT | Trạng thái |
| --- | --- | --- | --- | --- | --- |
| `BG-DIEN-UAT-001` | Biểu giá điện sản xuất UAT 2026 | Điện | kWh | 0.08 | Đang áp dụng |
| `BG-NUOC-UAT-001` | Biểu giá nước sạch UAT 2026 | Nước | m3 | 0.05 | Đang áp dụng |
| `BG-NUOC-THAI-UAT-001` | Biểu giá xử lý nước thải UAT 2026 | Nước thải | m3 | 0.05 | Đang áp dụng |
| `BG-RAC-UAT-001` | Biểu giá thu gom rác thải UAT 2026 | Rác thải | kg | 0.05 | Đang áp dụng |

Biểu giá điện: giờ bình thường 1850, cao điểm 3100, thấp điểm 1200; bật điều chỉnh COSφ, ngưỡng 0.9, phạt 5%. Hiệu lực `01/09/2026-30/09/2027`, thuộc `UAT-KCN-01`. Các biểu giá nước/rác dùng giá phẳng, tắt COSφ (nước sạch 12500, nước thải 8200, rác 1500). Công thức chi tiết xem Phụ lục B.

**Đồng hồ và chỉ số (bộ ưu tiên có phạt COSφ):**

| Mã | Loại | Điểm đo | Hệ số | Khách |
| --- | --- | --- | ---: | --- |
| `DH-DIEN-UAT-001` | Điện | Bán ra | 1 | `UAT_KCN-01` |
| `DH-NUOC-UAT-001` | Nước | Bán ra | 1 | `UAT_KCN-01` |
| `DH-NUOC-THAI-UAT-001` | Nước thải | Bán ra | 1 | `UAT_KCN-01` |

Điện `DH-DIEN-UAT-001` ngày đọc `30/09/2026`: tổng 32000 -> 42150 (=10150 kWh); bình thường 20000 -> 26200 (=6200); cao điểm 7000 -> 9400 (=2400); thấp điểm 5000 -> 6550 (=1550); COSφ `0.86`, kVArh `980`.

Kỳ vọng: bình thường 6200x1850=11470000; cao điểm 2400x3100=7440000; thấp điểm 1550x1200=1860000; trước phạt 20770000; phạt COSφ 5% (0.86 < 0.9) =1038500; sau phạt 21808500; VAT 8% =1744680; tổng 23553180.

Nước `DH-NUOC-UAT-001`: 820 -> 1045 = 225 m3. Nước thải `DH-NUOC-THAI-UAT-001`: 610 -> 785 = 175 m3.

Bộ seed cũ không phạt COSφ (dùng khi cần test case đạt): điện 12500 -> 18680 (=6180 kWh), COSφ `0.92`, kVArh `340`.

**Rác thải:**

| Khách | Ngày | Loại | Vào/ra | Phương pháp |
| --- | --- | --- | ---: | --- |
| `UAT_KCN-01` | 30/09/2026 | `industrial` | 1250 / 1180 | `sorting` |
| `UAT_KCN-01` | 29/09/2026 | `domestic` | 620 / 590 | `composting` |
| `UAT_KCN-01` | 28/09/2026 | `hazardous` | 85 / 80 | `incineration` |

### 3.7 Run tính phí, thông báo phí, phải thu, thanh toán

**Run kỳ vọng:**

| Trường | Giá trị |
| --- | --- |
| Kỳ | `UAT-KTP-T09` |
| Run code kỳ vọng | `BR-UAT-KTP-T09-001` |
| Trạng thái run | `completed` |

Kết quả tính phí kỳ vọng trong file seed (dùng để đối chiếu, có thể lệch với bộ COSφ phạt nếu hệ thống tính live — khi đó lấy số live làm chuẩn và ghi chú):

```text
Điện: 12,926,000 trước VAT; VAT 1,034,080; tổng 13,960,080
Nước sạch: 2,812,500 trước VAT; VAT 140,625; tổng 2,953,125
Nước thải: 1,435,000 trước VAT; VAT 71,750; tổng 1,506,750
Rác thải: 1,875,000 trước VAT; VAT 93,750; tổng 1,968,750
Dịch vụ hạ tầng: 350,000 trước VAT; VAT 17,500; tổng 367,500
Tổng trước VAT: 19,398,500
Tổng VAT: 1,357,705
Tổng phải thu: 20,756,205
```

**Thông báo phí:**

| Trường | Giá trị |
| --- | --- |
| Mã | `TBP-UAT-KTP-T09-IIT` |
| Khách / kỳ | `UAT_KCN-01` / `UAT-KTP-T09` |
| Phát hành / đến hạn / trạng thái | `01/10/2026` / `15/10/2026` / `issued` |
| Trước VAT / VAT / tổng | 19398500 / 1357705 / 20756205 |

**Phải thu và thanh toán:**

| Mã phải thu | Khách | Số phải thu | Đã thu | Còn lại | Đến hạn | Trạng thái |
| --- | --- | ---: | ---: | ---: | --- | --- |
| `REC-UAT-KTP-T09-IIT` | `UAT_KCN-01` | 20756205 | 0 | 20756205 | 15/10/2026 | `open` |

| Mã thanh toán | Khách | Ngày | Số tiền | Tham chiếu | Phân bổ |
| --- | --- | --- | ---: | --- | --- |
| `PAY-UAT-IIT-2026-10-001` | `UAT_KCN-01` | 05/10/2026 | 10000000 | `VCB-UAT-IIT-1005` | 10000000 vào `REC-UAT-KTP-T09-IIT` |

Kỳ vọng sau phân bổ: `REC-UAT-KTP-T09-IIT` đã thu 10000000, còn 10756205, trạng thái `partial`.

### 3.8 Ticket, SLA, thông báo, NCC, báo cáo, provider boundary

Seed tay hiện chưa có sẵn các nhóm này. Tester tự tạo theo quy tắc `UAT-YYYYMMDD-*`:

| Nhóm | Cách tạo khi test | Mã gợi ý |
| --- | --- | --- |
| Ticket | Tạo từ khách `UAT_KCN-01`, gắn hạ tầng `UAT-KCN-HT-01` hoặc lô `UAT-KCN-KD-01` | `TCK-UAT-20260917-01` |
| SLA | Dùng policy mặc định của hệ thống; ghi thời gian phản hồi/xử lý | Theo hệ thống |
| Notification | Tạo thủ công hoặc từ ticket/công nợ IIT | Theo hệ thống |
| Nhà cung cấp / phải trả / chi phí | Tạo NCC `UAT-SUP-20260917-01`, AP và expense tương ứng | `UAT-SUP-*`, `AP-UAT-*` |
| Báo cáo / export | Chạy báo cáo lọc `UAT-KCN-01`, export và đối chiếu với `REC-UAT-*` | Theo hệ thống |
| eInvoice | Tạo từ `TBP-UAT-KTP-T09-IIT` hoặc `REC-UAT-KTP-T09-IIT`, không gọi provider live | `EINV-UAT-*` |
| Chữ ký số | Tạo từ tài liệu hợp đồng `UAT-HD-*`, kiểm tra queue/log | `SIGN-UAT-*` |

Nếu UI chưa hỗ trợ tạo, ghi `Blocked` + nêu màn hình/field thiếu.

### 3.9 Ma trận case UAT theo phân hệ

| Phân hệ | Nhóm case | Dữ liệu chính | Bằng chứng tối thiểu |
| --- | --- | --- | --- |
| Khởi động phiên UAT | `UAT-00.*` | Root, `UAT-KCN-01` | Dashboard, bộ lọc KCN/cụm, không lỗi nền |
| Quản trị hệ thống | `UAT-01.*` | KCN/cụm, user, role, park scope | Ảnh danh sách user/role và kết quả chặn sai phạm vi |
| Khách hàng | `UAT-02.*` | `UAT_KCN-01`, hồ sơ `UAT-DOC-*` | Ảnh danh sách, chi tiết IIT, file hồ sơ hoặc export |
| Hợp đồng | `UAT-03.*` | `UAT-HD-01`, `UAT-HD-LAND-2026-001`, `UAT-HD-ASSET-2026-001`, `UAT-HD-SVC-2026-001` | Ảnh chi tiết hợp đồng, tài liệu, điều khoản tài chính |
| Hạ tầng/GIS | `UAT-04.*` | `UAT-KCN-KD-01`, `GIS-UAT-KCN-KD-01`, `UAT-KCN-HT-01`, `UAT_ASSET1` | Ảnh lô/tài sản, popup GIS |
| Tính phí/tài chính | `UAT-05.*` | `UAT-KTP-T09`, `BG-*-UAT-001`, `DH-*-UAT-001`, `TBP/REC/PAY-UAT-*` | Ảnh công nợ, thanh toán, giấy báo phí, run billing |
| Ticket/SLA/thông báo | `UAT-06.*` | `TCK-UAT-*`, notification tự tạo | Ảnh ticket, timeline/comment, notification |
| Provider boundary | `UAT-07.*` | `EINV-UAT-*`, `SIGN-UAT-*` từ dữ liệu IIT | Ảnh trạng thái queue/log, không gọi provider live |
| Báo cáo/export | `UAT-08.*` | Report lọc `UAT-KCN-01` | File tải xuống và ảnh số liệu nguồn |
| Phân quyền/audit | `UAT-09.*` | User `uat.operator.*` / `uat.iit.*` | Ảnh chặn quyền, audit có actor/action/entity |

---

## 4. Kiểm tra khởi động phiên UAT

> **Tính năng này là gì:** đây là bước "mở cửa kho" trước khi test. Dashboard là màn hình tổng quan sau đăng nhập, cho biết hệ thống có đang chạy tốt không. Bộ chọn KCN/cụm là công tắc lọc dữ liệu: chọn `Khu công nghiệp UAT` thì mọi màn hình sau đó chỉ hiện dữ liệu của khu đó. Nếu đăng nhập lỗi, trang trắng hoặc báo 401/403/500 thì mọi case phía sau đều vô nghĩa, phải dừng lại báo `Blocked`.
>
> **Cách hoạt động:** đăng nhập root (`admin@ipms.local`) để bỏ qua phân quyền hẹp nhất, vào dashboard xem menu đủ 12 phân hệ. Sau đó chọn `UAT-KCN-01` để "khóa" phiên làm việc vào đúng bộ seed IIT. Mở vài menu để chắc dữ liệu có tải (không báo thiếu dữ liệu nền). Ví dụ: mở Khách hàng phải thấy `UAT_KCN-01`, mở Tính phí phải thấy `UAT-KTP-T09`.
>
> **Lỗi hay gặp:** sai mật khẩu sau rebuild (`Blocked by credential`), web/API chưa chạy (trang trắng), chọn khu nhưng màn hình vẫn trống (lỗi scope hoặc seed chưa nhập đủ).

### UAT-00.01 Đăng nhập root admin

1. Mở `http://localhost:8000`.
2. Đăng nhập bằng `admin@ipms.local` / `Admin@123456`.
3. Xác nhận vào được dashboard.
4. Xác nhận menu có các phân hệ: Quản trị hệ thống, Quản lý khách hàng, Hợp đồng, Tính phí, Tài chính, Tài sản, Hạ tầng, GIS, Phiếu yêu cầu, Thông báo, E-invoice/ký số, Báo cáo.

Kết quả mong đợi: root admin đăng nhập được, không gặp 401/403/500, không có trang trắng.

### UAT-00.02 Chọn dữ liệu UAT-KCN-01

1. Tại bộ chọn KCN/cụm nếu có, chọn `Khu công nghiệp UAT`.
2. Tìm nhanh mã `UAT-KCN-01` hoặc tên `Khu công nghiệp UAT`.
3. Mở lần lượt các menu chính để xác nhận dữ liệu tải được.

Kết quả mong đợi: dữ liệu UAT-KCN-01 xuất hiện ở các màn hình liên quan, không còn trạng thái thiếu dữ liệu nền ở tab phạm vi dữ liệu.

---

## 5. Quản trị hệ thống

> **Tính năng này là gì:** đây là "phần móng" của cả hệ thống. Gồm 3 thứ: (1) Dữ liệu nền KCN/cụm/phòng ban — danh mục dùng chung, mọi khách/hợp đồng/đồng hồ đều phải gắn vào 1 KCN; (2) Phạm vi dữ liệu — bảng cho biết user nào được thấy khu/công ty nào; (3) Người dùng — nơi root tạo tài khoản, gán vai trò (`PARK_ADMIN`, `ENTERPRISE_ADMIN`...) và gán khu/công ty.
>
> **Cách hoạt động:** KCN `UAT-KCN-01` chứa 2 cụm `UAT-LO-A/B`. Phòng ban là đơn vị xử lý ticket sau này. Khi tạo user, hệ thống bắt chọn `Park ID` bằng ô chọn (hiện tên `Khu công nghiệp UAT`, không gõ tay mã), nếu là user doanh nghiệp thì chọn thêm `Customer ID` (hiện tên `Công ty cổ phần IIT`) và danh sách công ty tự lọc theo khu đã chọn. Tạo sai phạm vi thì user sẽ không thấy đúng dữ liệu ở các mục sau. Chi tiết quyền xem mục 15.
>
> **Ví dụ với bộ IIT:** tìm `UAT-KCN-01` phải ra 1 KCN đang hoạt động; tạo phòng ban `UAT-DEPT-20260917-01` để sau này gán ticket; tạo `uat.operator.kcn01@` phạm vi khu và `uat.iit.user@` phạm vi IIT.
>
> **Lỗi hay gặp:** báo `400 BAD_REQUEST` khi lưu (thiếu trường hoặc sai phạm vi), ô khu/công ty bắt gõ UUID tay (lỗi UI), tab phạm vi báo "Chưa chọn khu..." dù đã nhập seed (lỗi scope hoặc seed thiếu).

### UAT-01.01 Kiểm tra dữ liệu nền KCN/cụm/phòng ban

1. Vào `Quản trị hệ thống`.
2. Mở tab dữ liệu nền hoặc danh mục.
3. Tìm `UAT-KCN-01`, `UAT-LO-A`, `UAT-LO-B`.
4. Mở danh mục `Phòng ban`.
5. Tạo phòng ban mới:
   - Mã: `UAT-DEPT-20260917-01`
   - Tên: `Phòng UAT vận hành`
   - Trạng thái: đang hoạt động
6. Lưu và tìm lại bản ghi.

Kết quả mong đợi: tạo phòng ban không báo `400 BAD_REQUEST`, danh sách hiển thị bản ghi mới.

### UAT-01.02 Kiểm tra phạm vi dữ liệu

1. Vào tab `Phạm vi dữ liệu`.
2. Lọc theo `Toàn hệ thống`, `Khu công nghiệp`, `Doanh nghiệp` nếu có.
3. Tìm `Khu công nghiệp UAT`.
4. Tìm `Công ty cổ phần IIT`.

Kết quả mong đợi: tab có dữ liệu seed tay, không hiện thông báo `Chưa chọn khu công nghiệp hoặc kết nối hệ thống chưa sẵn sàng`.

### UAT-01.03 Tạo người dùng nội bộ phạm vi khu

1. Vào tab `Người dùng`.
2. Bấm `Tạo người dùng`.
3. Nhập:
   - Email: `uat.operator.kcn01@ipms.local`
   - Tên: `Nhân viên UAT KCN-01`
   - Vai trò: vai trò vận hành khu hoặc park admin/operator có sẵn
   - Park ID: chọn bằng select, option hiển thị `Khu công nghiệp UAT`
4. Lưu.
5. Mở chi tiết user và kiểm tra vai trò/phạm vi.

Kết quả mong đợi: `Park ID` là select hiển thị tên khu, không bắt nhập UUID tự do; root admin tạo user không bị `400 BAD_REQUEST`.

### UAT-01.04 Tạo người dùng phạm vi doanh nghiệp

1. Vào `Tạo người dùng`.
2. Chọn vai trò/phạm vi doanh nghiệp nếu UI hỗ trợ.
3. Chọn `Park ID` = `Khu công nghiệp UAT`.
4. Kiểm tra `Customer ID` chuyển thành select.
5. Chọn option `Công ty cổ phần IIT`.
6. Lưu với email `uat.iit.user@ipms.local`.

Kết quả mong đợi: `Customer ID` là select hiển thị tên doanh nghiệp, danh sách được load theo khu đã chọn.

### UAT-01.05 Responsive và validate form quản trị

1. Lặp lại các màn hình chính của `Quản trị hệ thống` ở desktop, tablet, mobile.
2. Thử bỏ trống trường bắt buộc, nhập mã quá dài, nhập email sai định dạng.
3. Kiểm tra nút lưu khi API đang xử lý.

Kết quả mong đợi: form không vỡ layout, lỗi validate rõ bằng tiếng Việt, FE chặn lỗi cơ bản trước khi gọi API, BE vẫn trả lỗi rõ khi payload không hợp lệ.

---

## 6. Quản lý khách hàng

> **Tính năng này là gì:** quản lý "ai đang thuê trong khu". Mỗi khách hàng (doanh nghiệp) có mã, tên, MST, email, điện thoại, khu trực thuộc và trạng thái (`active/suspended/left`). Hồ sơ là các file pháp lý đính kèm (ĐKKD, thuế...). Nút import/export giúp nhập hàng loạt hoặc xuất danh sách ra file.
>
> **Cách hoạt động:** danh sách lọc theo KCN. Mở chi tiết `UAT_KCN-01` sẽ thấy 2 tab chính: thông tin chung và hồ sơ. Hồ sơ upload lên thì phải tải lại được (kiểm tra cả tên file và nội dung). Tạo khách mới (`UAT-CUS-20260917-01`) dùng để test luồng tạo mà không làm bẩn seed IIT. Nút `Tải mẫu/Xuất CSV/Nhập XLSX` kiểm tra cả quyền (không có quyền thì không tải được) và giao diện (desktop/mobile không vỡ nút).
>
> **Ví dụ với bộ IIT:** `UAT_KCN-01 — Công ty cổ phần IIT`, MST `0319999999`, email `finance.iit@example.test`. Hồ sơ `UAT-DOC-IIT-BL-01` do tester tự upload vì seed tay chưa có file mẫu.
>
> **Lỗi hay gặp:** mở chi tiết trang trắng (lỗi FE/BE), MST/email trùng không báo rõ, file upload xong không tải lại được, nút import/export bị tách dòng xấu trên mobile.

### UAT-02.01 Kiểm tra danh sách seed

1. Vào `Quản lý khách hàng`.
2. Chọn/lọc khu `Khu công nghiệp UAT`.
3. Tìm mã `UAT_KCN-01`.
4. Mở chi tiết `Công ty cổ phần IIT`.

Kết quả mong đợi: thấy khách IIT với MST `0319999999`, email `finance.iit@example.test`, mở chi tiết không trang trắng.

### UAT-02.02 Tạo doanh nghiệp mới

1. Bấm `Thêm khách hàng`.
2. Nhập:
   - Mã khách hàng: `UAT-CUS-20260917-01`
   - Tên doanh nghiệp: `Công ty TNHH UAT An Phú`
   - MST: `1809999001`
   - Email: `uat.anphu@example.test`
   - Điện thoại: `02923809999`
   - Khu: `Khu công nghiệp UAT`
   - Trạng thái: `active`
3. Lưu và tìm lại.

Kết quả mong đợi: tạo mới thành công, validate giới hạn độ dài rõ ràng.

### UAT-02.03 Hồ sơ khách hàng

1. Mở chi tiết `UAT_KCN-01`.
2. Vào tab `Hồ sơ`.
3. Upload hồ sơ `UAT-DOC-IIT-BL-01` (giấy ĐKKD).
4. Tải lại file vừa upload.

Kết quả mong đợi: không trang trắng, hồ sơ tải/xem được hoặc báo lỗi nghiệp vụ rõ.

### UAT-02.04 Kiểm tra nút import/export khách hàng

1. Ở danh sách khách hàng, quan sát các nút `Xóa bộ lọc`, `Tải mẫu`, `Xuất CSV`, `Nhập XLSX`.
2. Kiểm tra ở desktop và mobile.
3. Bấm `Tải mẫu`, `Xuất CSV`.

Kết quả mong đợi: icon và text nằm trên một dòng ở kích thước đủ, không bị tách dòng xấu; file tải được nếu user có quyền.

---

## 7. Hợp đồng

> **Tính năng này là gì:** hợp đồng là "sợi dây" nối khách hàng với thứ họ thuê: lô đất, hạ tầng/tài sản hoặc dịch vụ điện/nước/rác. Mỗi hợp đồng có loại (`thuê đất / thuê tài sản / dịch vụ`), thời hạn, line item (thuê cái gì, bao nhiêu, đơn giá bao nhiêu) và điều khoản tài chính (khoản thu nào, VAT bao nhiêu %, trả theo tháng/quý, cọc bao nhiêu, đến hạn ngày mấy). Tài liệu đính kèm là file hợp đồng đã ký; phụ lục là bản sửa đổi bổ sung sau này.
>
> **Cách hoạt động:** 4 hợp đồng seed IIT bao phủ đủ 3 loại. `UAT-HD-01` là bản thuê đất đơn giản (10000 m2 x 100 = 1000000). `UAT-HD-LAND-2026-001` là bản dài hạn (10000 m2 x 85000 = 850000000, cọc 1700000000, trả theo quý, đến hạn ngày 10). `UAT-HD-ASSET-2026-001` là thuê camera `UAT-KCN-HT-01` (1 x 3500000). `UAT-HD-SVC-2026-001` là gói dịch vụ 3 dòng điện/nước/rác (tổng trước VAT 16120500). Khi tạo hợp đồng mới, hệ thống phải chặn: ngày kết thúc trước ngày bắt đầu, số tiền âm, mã trùng. Lô đã cho thuê (`UAT-KCN-KD-01`) thì không cho gán trùng nếu đã hết chỗ.
>
> **Ví dụ đối chiếu:** mở `UAT-HD-LAND-2026-001` phải thấy khách IIT, cụm `UAT-LO-A`, dòng thuê lô `UAT-KCN-KD-01`; mở `UAT-HD-SVC-2026-001` phải thấy 3 dòng điện 6180 kWh / nước 225 m3 / rác 1250 kg gắn đúng đồng hồ `DH-*-UAT-001`.
>
> **Lỗi hay gặp:** thiếu line item nhưng vẫn lưu được, ngày sai vẫn lưu, file hợp đồng không tải được, phụ lục không liên kết được với hợp đồng gốc.

### UAT-03.01 Kiểm tra hợp đồng seed

1. Vào `Quản lý hợp đồng`.
2. Lọc theo `Khu công nghiệp UAT`.
3. Tìm các mã `UAT-HD-01`, `UAT-HD-LAND-2026-001`, `UAT-HD-ASSET-2026-001`, `UAT-HD-SVC-2026-001`.
4. Mở chi tiết từng loại: thuê đất, thuê hạ tầng, dịch vụ.

Kết quả mong đợi: đủ 4 hợp đồng, hiển thị đúng khách IIT, KCN/cụm, ngày hiệu lực, điều khoản tài chính (ngày đến hạn 10/15, VAT, chu kỳ).

### UAT-03.02 Tạo hợp đồng mới từ UI

1. Bấm `Tạo hợp đồng`.
2. Chọn khách hàng `Công ty cổ phần IIT` hoặc `UAT-CUS-20260917-01`.
3. Chọn loại hợp đồng phù hợp.
4. Chọn lô đất còn available (nếu `UAT-KCN-KD-01` đã gắn IIT thì tạo lô mới `UAT-KCN-KD-02` hoặc chọn tài sản phù hợp).
5. Nhập ngày hiệu lực, giá trị, line item.
6. Lưu.

Kết quả mong đợi: hợp đồng mới lưu được, ngày kết thúc phải sau ngày bắt đầu, số tiền không âm, mã không trùng.

### UAT-03.03 Phụ lục, tài liệu và điều khoản tài chính

1. Mở `UAT-HD-LAND-2026-001`.
2. Kiểm tra dòng thuê 10000 m2 x 85000 = 850000000, cọc 1700000000, đến hạn ngày 10.
3. Mở `UAT-HD-SVC-2026-001`, đối chiếu 3 dòng điện/nước/rác và tổng trước VAT 16120500.
4. Upload/tải tài liệu hợp đồng nếu UI hỗ trợ.

Kết quả mong đợi: dữ liệu hợp đồng liên kết đúng IIT/lô đất; file hợp đồng tải được hoặc có trạng thái download rõ.

---

## 8. Hạ tầng, tài sản và GIS

> **Tính năng này là gì:** 3 lớp quản lý "đồ đạc" của khu. Lô đất là mảnh đất cho thuê (`UAT-KCN-KD-01`, 10000 m2, số lô `SL-01`, trạng thái sẵn sàng/đã thuê). Hạ tầng là công trình dùng chung (camera `UAT-KCN-HT-01`, nhóm `security`). Tài sản là thiết bị cụ thể gắn vào hạ tầng (camera `UAT_ASSET1`, hãng IMOU, serial `123123`). GIS là bản đồ: layer (`GIS-UAT-KCN-LOT`) là lớp bản đồ, feature (`GIS-UAT-KCN-KD-01`) là hình vẽ trên bản đồ (polygon tọa độ quanh 106.7044/10.8024) liên kết tới lô đất thật.
>
> **Cách hoạt động:** lô đất gắn cụm `UAT-LO-A` + feature GIS + khách/hợp đồng thuê. Hạ tầng gắn KCN/cụm, có ngày lắp/bảo trì và trạng thái vận hành (Bình thường). Tài sản gắn hạ tầng liên quan. Trên GIS, zoom/pan/mở popup phải ra đúng thông tin lô (`areaM2:10000`, trạng thái `available`), bấm vào feature phải nhảy được sang hồ sơ lô/tài sản/ticket liên quan. Ô chọn tài sản ở các màn hình khác phải hiện tên (`Camera giám sát`), không hiện mã khô khan hay UUID.
>
> **Ví dụ với bộ IIT:** lô `UAT-KCN-KD-01` cho IIT thuê theo `UAT-HD-01`; camera `UAT-KCN-HT-01` cho IIT thuê theo `UAT-HD-ASSET-2026-001`; feature GIS vẽ đúng ranh lô đó.
>
> **Lỗi hay gặp:** bản đồ trắng (lỗi tile/layer), popup không mở, feature không liên kết sang nghiệp vụ, trạng thái lô/tài sản sai (đã thuê vẫn hiện trống), dropdown chỉ hiện UUID.

### UAT-04.01 Lô đất

1. Vào `Hạ tầng kỹ thuật`.
2. Tìm `UAT-KCN-KD-01`.
3. Kiểm tra trạng thái, diện tích 10000, số lô `SL-01`, cụm `UAT-LO-A`.
4. Kiểm tra liên kết khách IIT / hợp đồng `UAT-HD-01` / `UAT-HD-LAND-2026-001`.

Kết quả mong đợi: lô đất hiển thị đúng trạng thái và liên kết khách hàng/hợp đồng.

### UAT-04.02 Hạ tầng và tài sản

1. Tìm `UAT-KCN-HT-01` (Camera giám sát số 123).
2. Mở chi tiết, kiểm tra trạng thái vận hành Bình thường, nhóm `security`, ngày bảo trì `30/09/2026`.
3. Tìm `UAT_ASSET1`, kiểm tra loại `camera_system`, tình trạng Tốt, hãng IMOU, liên kết `UAT-KCN-HT-01`.
4. Kiểm tra filter/field chọn tài sản hiển thị tên tài sản, không chỉ hiển thị mã/UUID.

Kết quả mong đợi: hạ tầng/tài sản hoạt động, có thể liên kết ticket/sự cố.

### UAT-04.03 GIS

1. Vào `GIS`.
2. Lọc/tìm layer `GIS-UAT-KCN-LOT`.
3. Tìm feature `GIS-UAT-KCN-KD-01`.
4. Thử zoom, pan, mở popup/chi tiết; đối chiếu geometry Polygon quanh `[106.7044, 10.8024]` và properties `areaM2:10000`.
5. Kiểm tra liên kết feature tới lô `UAT-KCN-KD-01`.

Kết quả mong đợi: bản đồ tải được, không trắng, feature mở được popup/chi tiết, liên kết đúng lô đất.

---

## 9. Tính phí và tài chính — luồng Tài chính công nợ cho người không chuyên

> **Nói một câu:** Tính phí là khâu "tính xem IIT nợ bao nhiêu", Tài chính công nợ là khâu "theo dõi IIT đã trả bao nhiêu, còn nợ bao nhiêu". Tiền chỉ hết nợ khi phiếu thu được gắn (phân bổ) đúng vào khoản nợ.
>
> **Sơ đồ đi của tiền (nhớ 6 bước này là hiểu hết mục 9):** đồng hồ/chỉ số → run tính phí `BR-UAT-KTP-T09-001` → thông báo phí `TBP-UAT-KTP-T09-IIT` → phải thu `REC-UAT-KTP-T09-IIT` → thanh toán `PAY-UAT-IIT-2026-10-001` → phân bổ (gắn tiền vào nợ). Chiều ngược lại là tiền khu phải trả cho người khác: nhà cung cấp → phải trả NCC → thanh toán NCC → phân bổ NCC.
>
> **Từ mới cần biết (gặp đúng chữ này trên nút/tab màn hình):**
>
> | Từ trên màn hình | Hiểu đơn giản | Ví dụ IIT |
> | --- | --- | --- |
> | Thông báo phí | Tờ giấy báo "tháng này IIT phải trả từng này" | `TBP-UAT-KTP-T09-IIT`, tổng 20756205, đến hạn 15/10/2026 |
> | Phải thu | Khoản nợ cần đòi, sinh ra từ thông báo phí | `REC-UAT-KTP-T09-IIT`, ban đầu `Đang mở`, nợ 20756205 |
> | Thanh toán / phiếu thu | Tờ tiền IIT đã chuyển, trạng thái `posted` là đã ghi nhận | `PAY-UAT-IIT-2026-10-001`, 10 triệu ngày 05/10, ref `VCB-UAT-IIT-1005` |
> | Phân bổ (tab `Phân bổ`) | Thao tác "gắn" tờ tiền vào đúng khoản nợ | Gắn 10 triệu vào `REC-UAT-KTP-T09-IIT` |
> | Đã phân bổ / Chưa phân bổ | Tiền đã gắn vào nợ / tiền còn treo chưa biết trừ vào đâu | Sau khi gắn: đã phân bổ 10000000, chưa phân bổ 0 ở phiếu này; nợ còn 10756205 |
> | Trạng thái nợ | `Bản nháp (draft)` → `Đang mở (open)` → `Thu một phần (partial)` → `Đã thanh toán (paid)`; quá hạn thì thành `Quá hạn (overdue)`, hủy thì `Đã hủy (cancelled)` | `REC-UAT-KTP-T09-IIT` đi từ `open` → `partial` sau khi gắn 10 triệu |
> | Gợi ý phân bổ | Nút hệ thống tự đề xuất "nên trừ vào khoản nào" | Bấm để xem gợi ý rồi mới bấm Lưu phân bổ |
> | Điều chỉnh (tab `Điều chỉnh`) | Giấy sửa nợ (tăng/giảm) khi tính nhầm, ở trạng thái yêu cầu và cần duyệt mới áp dụng | Ví dụ xin giảm 500000 vì đối soát khối lượng, phải có lý do |
> | Kỳ tài chính (tab `Kỳ tài chính`) | Cái khóa theo tháng: khóa rồi thì không cho phát sinh/sửa trong tháng đó | Khóa tháng 09 thì không tạo nợ/thu lùi ngày vào tháng 09 nữa |
> | Phải trả NCC / Thanh toán NCC / Chi phí | Chiều khu trả tiền: khu nợ NCC → khu chuyển tiền → gắn tiền vào nợ NCC | Tự tạo `UAT-SUP-20260917-01` vì seed tay chưa có NCC |
> | Đối soát (báo cáo `RPT_FINANCE_RECONCILIATION`) | Bảng kiểm tra phiếu thu và phân bổ có khớp nhau không, ai chưa khớp, ai thu một phần, ai phân bổ vượt | Dùng khi PM hỏi "tiền đã khớp hết chưa" |
>
> **Cách đọc 3 con số quan trọng (đừng nhầm):** `Số phải thu` là tổng nợ ban đầu; `Đã thu (đã phân bổ)` là tiền đã gắn vào nợ; `Còn lại = Số phải thu − Đã thu`. Với IIT: 20756205 − 10000000 = 10756205. Còn `Chưa phân bổ` nằm ở phía phiếu thu: `Chưa phân bổ = Số tiền phiếu − Tổng đã gắn`. Ví dụ phiếu 12 triệu mà mới gắn 10 triệu thì còn treo 2 triệu.
>
> **Lỗi hay gặp:** nhầm "tạo thanh toán" với "đã hết nợ" (tạo mà chưa phân bổ thì nợ vẫn còn), gắn vượt số nợ (hệ thống phải chặn), gắn nhầm sang khách khác, sửa kỳ đã khóa, tạo điều chỉnh nhưng không được duyệt mà tưởng đã trừ nợ.

### UAT-05.01 Mở khoản nợ IIT và đọc đúng 3 con số (phải thu)

> **Luồng:** vào `Tài chính & công nợ` → lọc khu `Khu công nghiệp UAT` → tìm `REC-UAT-KTP-T09-IIT` → mở chi tiết. Đây là khoản nợ sinh từ thông báo phí `TBP-UAT-KTP-T09-IIT` (phát hành 01/10, đến hạn 15/10/2026).
>
> **Đọc màn hình thế nào:** nhìn 3 ô `Số phải thu / Đã thu / Còn lại` và ô `Trạng thái`. Chưa test UAT-05.02 thì phải thấy 20756205 / 0 / 20756205 và `Đang mở (open)`. Đã test UAT-05.02 rồi thì phải thấy 20756205 / 10000000 / 10756205 và `Thu một phần (partial)`. Nếu quá ngày 15/10 mà chưa trả hết thì trạng thái chuyển `Quá hạn (overdue)`. Mở tab `Phân bổ` thì thấy đang gắn bao nhiêu tiền vào khoản này; mở tab `Điều chỉnh` thì thấy có giấy sửa nợ nào đang chờ duyệt không.

1. Vào `Tài chính & công nợ`.
2. Lọc theo khu `Khu công nghiệp UAT`.
3. Tìm `REC-UAT-KTP-T09-IIT`.
4. Mở chi tiết, đọc 3 số + trạng thái + ngày đến hạn, chụp ảnh.
5. Mở tab `Phân bổ` và tab `Điều chỉnh` để chắc không có gì lạ.

Kết quả mong đợi: tổng 20756205; đã thu 0 (hoặc 10000000 nếu đã test UAT-05.02); còn lại tương ứng; trạng thái `open`/`partial`; đến hạn `15/10/2026`. Nếu cần tạo nợ tay để test thêm thì dùng nút `Tạo phải thu thủ công` → điền `Mã phải thu / Khách hàng / Số tiền / Mô tả` → `Lưu phải thu`, nhưng đừng sửa `REC-UAT-*` gốc.

### UAT-05.02 Thu tiền IIT: tạo phiếu thu rồi gắn vào nợ (thanh toán + phân bổ)

> **Luồng:** đây là bước nhiều người nhầm nhất nên làm đúng thứ tự 2 việc: (1) tạo phiếu thu `PAY-UAT-IIT-2026-10-001` — nghĩa là "ghi nhận IIT đã chuyển 10 triệu ngày 05/10 bằng chuyển khoản, ref `VCB-UAT-IIT-1005`", trạng thái `posted`; (2) phân bổ — nghĩa là "gắn 10 triệu đó vào đúng khoản `REC-UAT-KTP-T09-IIT`". Chỉ sau bước 2 thì nợ mới giảm từ 20756205 xuống 10756205 và chuyển `open` → `partial`. Nếu mới tạo phiếu mà chưa phân bổ thì tiền nằm ở ô `Chưa phân bổ`, nợ vẫn nguyên — đây không phải lỗi.
>
> **Thao tác gợi ý trên UI:** tạo/lưu phiếu thu → vào chi tiết phiếu → tab `Phân bổ` → bấm gợi ý phân bổ để hệ thống đề xuất khoản `REC-UAT-KTP-T09-IIT` → nhập 10000000 → Lưu. Thử nhập vượt (ví dụ 99999999) thì hệ thống phải chặn, đó là `Pass` cho kiểm tra chặn phân bổ vượt.

1. Tạo thanh toán `PAY-UAT-IIT-2026-10-001` cho `REC-UAT-KTP-T09-IIT` (05/10/2026, 10000000, chuyển khoản, ref `VCB-UAT-IIT-1005`), lưu tới khi `posted`.
2. Vào tab `Phân bổ`, phân bổ 10000000 vào `REC-UAT-KTP-T09-IIT` (dùng gợi ý nếu có).
3. Quay lại phải thu, kiểm tra 20756205 / 10000000 / 10756205 + `partial`; quay lại phiếu thu, kiểm tra `Chưa phân bổ` = 0.
4. Thử phân bổ vượt để chắc hệ thống chặn.

Kết quả mong đợi: còn lại 10756205, trạng thái `partial`; không cho phân bổ vượt số phải thu. Bằng chứng: ảnh phải thu trước/sau + ảnh phiếu thu + tab phân bổ.

### UAT-05.03 Billing run / fee notice

1. Vào `Tính phí`.
2. Mở kỳ `UAT-KTP-T09` (01/09-30/09/2026, Đã rà soát).
3. Kiểm tra run `BR-UAT-KTP-T09-001` trạng thái `completed`.
4. Mở thông báo phí `TBP-UAT-KTP-T09-IIT`, kiểm tra trước VAT 19398500, VAT 1357705, tổng 20756205, phát hành `01/10/2026`, đến hạn `15/10/2026`.
5. Nếu tạo kỳ mới, dùng mã `UAT-KTP-202609-01`.

Kết quả mong đợi: dữ liệu hợp đồng/đồng hồ đủ để đối chiếu; PDF/template chính thức có thể `Blocked by official template` nếu chưa được khách hàng chốt.

### UAT-05.04 Điện, nước, nước thải, rác và phí dịch vụ

1. Trong module `Tính phí`, mở tab đồng hồ/chỉ số nếu có.
2. Tìm `DH-DIEN-UAT-001`, kiểm tra chỉ số tổng 32000 -> 42150 (10150 kWh); tách giờ 6200/2400/1550; COSφ `0.86`; tính phạt 5% theo mục 3.6.
3. Tìm `DH-NUOC-UAT-001` (820 -> 1045 = 225 m3) và `DH-NUOC-THAI-UAT-001` (610 -> 785 = 175 m3).
4. Đối chiếu biểu giá `BG-DIEN-UAT-001`, `BG-NUOC-UAT-001`, `BG-NUOC-THAI-UAT-001`, `BG-RAC-UAT-001`.
5. Kiểm tra bản ghi rác `industrial` 1250/1180 ngày `30/09/2026` (kèm `domestic`/`hazardous` nếu đã nhập).
6. Đối chiếu dòng dịch vụ hạ tầng 350000 từ `UAT-HD-ASSET-2026-001`.

Kết quả mong đợi: điện/nước/nước thải/rác tính đúng theo file seed; field phụ thuộc như kỳ, đồng hồ, khách hàng, biểu giá dùng select-option có tên dễ hiểu.

### UAT-05.05 Chiều khu trả tiền: NCC → phải trả → thanh toán NCC → phân bổ (cho người không chuyên)

> **Luồng:** đây là chiều ngược với thu tiền IIT. Hiểu bằng ví dụ: khu thuê Điện lực sửa trạm bơm → Điện lực thành `Nhà cung cấp (NCC)` → khu ghi giấy `Phải trả NCC` ("khu nợ NCC từng này", trạng thái `open/partial/paid/overdue` tương tự phải thu) → khu chuyển tiền bằng `Thanh toán NCC` (`posted`, có `UNC` tham chiếu) → vào tab `Phân bổ thanh toán NCC` để gắn tiền vào đúng giấy nợ. `Chi phí` là khoản ghi nhận đã tiêu (bảo trì, bảo vệ...), có thể gắn NCC hoặc không. Seed tay chưa có NCC nên phải tự tạo `UAT-SUP-20260917-01` trước, rồi mới tạo được phải trả.
>
> **Đảo ngược/điều chỉnh cần biết:** thanh toán NCC có thêm nút đảo ngược (`reversed`, phải nhập lý do) khi chuyển nhầm; khi đó tiền đã gắn về 0. Tab `Kỳ tài chính` khóa theo tháng: đã khóa thì không tạo nợ/thu NCC lùi ngày vào tháng đó. Doanh nghiệp IIT (tenant) không được thấy màn hình này — nếu IIT mở được thì đó là lỗi phân quyền.

1. Vào `Tài chính` -> tab nhà cung cấp/phải trả/chi phí nếu có.
2. Tạo NCC `UAT-SUP-20260917-01` (điền tên/MST/email kế toán/điện thoại, thuộc `UAT-KCN-01`).
3. Tạo phải trả NCC: điền `Mã phải trả NCC / Nhà cung cấp (chọn từ danh sách, không gõ tay) / Kỳ (VD 2026-09) / Tiền trước thuế / Thuế NCC / Diễn giải`, lưu tới `open`.
4. Tạo thanh toán NCC (`posted`), vào `Phân bổ thanh toán NCC` để gắn tiền vào phải trả vừa tạo; kiểm tra `Đã phân bổ / Chưa phân bổ` như luồng IIT.
5. Tạo 1 chi phí gắn NCC trên và KCN `UAT-KCN-01`, lưu và mở lại.
6. Đăng nhập thử `uat.iit.admin@` để chắc tenant không mở được màn hình NCC.

Kết quả mong đợi: NCC chọn từ danh sách, không nhập tay tự do; phải trả đi `open` → `partial` → `paid` đúng khi phân bổ; đảo ngược phải có lý do; kỳ đã khóa thì chặn phát sinh; tenant IIT bị chặn. Lưu chi phí không lỗi validate mơ hồ. Nếu UI chưa hỗ trợ, ghi `Blocked` + nêu tab thiếu.

---

## 10. Phiếu yêu cầu, SLA và thông báo

> **Tính năng này là gì:** ticket là "phiếu kêu cứu" của doanh nghiệp (hỏng camera, mất nước...). Mỗi ticket có khách gửi, hạ tầng/lô liên quan, loại sự cố, ưu tiên, phòng ban xử lý, trạng thái (mới → đã nhận → đang xử lý → chờ xác nhận → hoàn thành/hủy) và timeline bình luận. SLA là "đồng hồ deadline": hệ thống đo thời gian phản hồi/xử lý, gắn nhãn đúng hạn (`met`), sắp trễ (`warning`), trễ (`breached`). Thông báo là tin gửi tới inbox/log của ban quản lý hoặc doanh nghiệp khi có ticket/công nợ mới.
>
> **Cách hoạt động:** seed tay chưa có ticket sẵn nên tester tự tạo `TCK-UAT-20260917-01` cho IIT, gắn `UAT-KCN-HT-01` hoặc `UAT-KCN-KD-01`. Tạo xong thì chuyển trạng thái thử, mỗi lần chuyển phải ghi timeline + cập nhật SLA + sinh thông báo (nếu đã cấu hình SMTP thì gửi mail, chưa thì ghi log `skipped`). Bình luận có loại public (doanh nghiệp thấy) và internal (chỉ nội bộ khu thấy).
>
> **Ví dụ với bộ IIT:** ticket "Camera số 123 mờ hình tại Lô A" của IIT, ưu tiên trung bình, gán phòng vận hành; SLA tính từ lúc tạo; thông báo hiện trong inbox của `uat.operator.kcn01@`.
>
> **Lỗi hay gặp:** tạo ticket không gắn được hạ tầng, chuyển trạng thái không cập nhật SLA, timeline mất bình luận, thông báo lộ secret/token, provider email thật chưa cấu hình thì phải ghi `Blocked` chứ không báo `Fail`.

### UAT-06.01 Kiểm tra ticket (tự tạo từ seed IIT)

1. Vào `Phiếu yêu cầu & phản ánh`.
2. Lọc theo `Khu công nghiệp UAT`.
3. Tạo ticket `TCK-UAT-20260917-01` cho `UAT_KCN-01`, gắn `UAT-KCN-HT-01` hoặc `UAT-KCN-KD-01`.
4. Mở chi tiết, kiểm tra trạng thái, ưu tiên, SLA, bình luận.

Kết quả mong đợi: ticket có timeline/bình luận, liên kết đúng IIT và hạ tầng camera/lô đất.

### UAT-06.02 Tạo và chuyển trạng thái ticket

1. Bấm `Tạo phiếu`.
2. Chọn khách `UAT_KCN-01`.
3. Chọn loại phản ánh hạ tầng/camera.
4. Gán ưu tiên, phòng ban xử lý nếu có.
5. Lưu và chuyển trạng thái.

Kết quả mong đợi: ticket tạo được, SLA/audit cập nhật theo thao tác.

### UAT-06.03 Thông báo

1. Vào `Thông báo`.
2. Kiểm tra inbox/log cho `uat.operator.kcn01@ipms.local` hoặc root admin.
3. Tạo thông báo thủ công từ ticket/công nợ IIT nếu UI hỗ trợ.

Kết quả mong đợi: thông báo hiển thị, nội dung không lộ secret/token. Provider email/SMS thật có thể `Blocked` nếu chưa cấu hình.

---

## 11. E-invoice, chữ ký số và provider boundary

> **Tính năng này là gì:** 2 tích hợp biên nhưng test ở chế độ "giả lập biên" (boundary), không gọi nhà cung cấp thật. E-invoice là hóa đơn điện tử phát hành từ thông báo phí/phải thu (ví dụ từ `TBP-UAT-KTP-T09-IIT`). Chữ ký số là luồng ký tài liệu hợp đồng (`UAT-HD-LAND-2026-001`): tạo yêu cầu ký → vào hàng đợi (queue) → gửi → ký xong → có file đã ký. Trạng thái gồm nháp/chờ duyệt/đã gửi/đã ký/thất bại.
>
> **Cách hoạt động:** tạo yêu cầu (`EINV-UAT-20260917-01` / `SIGN-UAT-20260917-01`) từ dữ liệu IIT, sau đó bấm phát hành/đồng bộ. Vì chưa có cấu hình MISA/certificate thật nên hệ thống phải chặn lại ở biên: không gọi live, chỉ ghi log/queue/timeline rõ lý do (`provider_mapping_blocked`, thiếu certificate...), tuyệt đối không "fake thành công". Tester chỉ kiểm tra trạng thái nội bộ + log, không kết luận tích hợp live thành công.
>
> **Ví dụ với bộ IIT:** từ `REC-UAT-KTP-T09-IIT` tạo hóa đơn nháp; từ file hợp đồng `UAT-HD-SVC-2026-001` tạo yêu cầu ký, kiểm tra vào queue.
>
> **Lỗi hay gặp:** hệ thống báo thành công dù chưa cấu hình provider (lỗi nghiêm trọng), log không rõ lý do chặn, mã trùng lặp vẫn cho phát hành 2 lần, dữ liệu khác khu lọt vào.

### UAT-07.01 E-invoice boundary

1. Vào module eInvoice/MISA nếu có trong menu.
2. Tạo yêu cầu hóa đơn từ `TBP-UAT-KTP-T09-IIT` hoặc `REC-UAT-KTP-T09-IIT` (mã gợi ý `EINV-UAT-20260917-01`).
3. Thử bước phát hành/đồng bộ provider.

Kết quả mong đợi: hệ thống không gọi live MISA khi chưa cấu hình; log provider boundary rõ ràng, không fake thành công live provider.

### UAT-07.02 Chữ ký số boundary

1. Vào module chữ ký số.
2. Tạo yêu cầu ký từ tài liệu hợp đồng `UAT-HD-LAND-2026-001` hoặc `UAT-HD-SVC-2026-001` (mã gợi ý `SIGN-UAT-20260917-01`).
3. Kiểm tra queue/timeline/log.

Kết quả mong đợi: yêu cầu ký vào queue/trạng thái nội bộ; live provider/certificate nằm ngoài phạm vi nếu chưa có cấu hình chính thức.

---

## 12. Báo cáo và xuất file

> **Tính năng này là gì:** báo cáo là "bảng tổng hợp" lấy số từ các màn hình nghiệp vụ (khách, hợp đồng, billing, công nợ, ticket, hạ tầng). Export là nút tải bảng đó ra CSV/XLSX/PDF. Lịch sử export cho biết ai đã xuất file nào, khi nào, để tải lại.
>
> **Cách hoạt động:** chọn/lọc `Khu công nghiệp UAT` rồi mở báo cáo: số khách phải khớp 1 IIT (+ khách tester tự tạo), số hợp đồng khớp 4 `UAT-HD-*`, phải thu khớp `REC-UAT-KTP-T09-IIT`, kỳ khớp `UAT-KTP-T09`. Xuất file ra thì mở lên kiểm tra: font tiếng Việt không lỗi, tiêu đề/cột tiền/cột trạng thái đúng, số trong file khớp số trên màn hình. PDF chính thức có thể `Blocked by official template` nếu khách chưa chốt mẫu.
>
> **Ví dụ với bộ IIT:** báo cáo công nợ lọc `UAT-KCN-01` phải ra tổng 20756205 / còn 10756205 sau thanh toán 10 triệu; file tải về đọc được bằng Excel.
>
> **Lỗi hay gặp:** số báo cáo lệch số màn hình nguồn, lỗi font tiếng Việt trong Excel/PDF, cột tiền sai định dạng, lịch sử export thiếu bản ghi, không tải lại được file cũ.

### UAT-08.01 Báo cáo tổng quan

1. Vào `Báo cáo & phân tích`.
2. Lọc `Khu công nghiệp UAT` để đối chiếu 1 khách IIT, 4 hợp đồng `UAT-HD-*`, phải thu `REC-UAT-KTP-T09-IIT`, kỳ `UAT-KTP-T09`.

Kết quả mong đợi: số liệu báo cáo không mâu thuẫn với danh sách nguồn.

### UAT-08.02 Xuất Excel/PDF

1. Xuất CSV/XLSX ở khách hàng, hợp đồng, tài chính, báo cáo (lọc `UAT-KCN-01`).
2. Mở file tải về.
3. Kiểm tra font tiếng Việt, tiêu đề, cột tiền, cột trạng thái.

Kết quả mong đợi: file đọc được, không lỗi font tiếng Việt; PDF chính thức có thể `Blocked by official template`.

---

## 13. Phân quyền và audit

> **Tính năng này là gì:** phân quyền đảm bảo "ai chỉ thấy phần của mình": park admin chỉ thấy khu `UAT-KCN-01`, IIT admin/user chỉ thấy công ty `UAT_KCN-01`. Audit (nhật ký) ghi lại ai đã làm gì, trên bản ghi nào, lúc nào — dùng để truy vết khi có sai sót tiền/quyền. Đọc kỹ giải thích không kỹ thuật ở mục 15 trước khi test mục này.
>
> **Cách hoạt động:** đăng nhập từng tài khoản (`uat.operator.kcn01@`, `uat.iit.admin@`, `uat.iit.user@`) rồi thử: thấy đúng dữ liệu mình, không thấy dữ liệu người khác, gõ URL trực tiếp (`/admin`, `/customers/new`) thì bị chặn, nút tạo/sửa ẩn đúng chỗ. Sau đó đăng nhập root vào `Nhật ký` lọc theo login/tạo user/tạo khách/tạo hợp đồng/thanh toán/export/run `BR-UAT-KTP-T09-001`: mỗi dòng phải có actor/action/entity/timestamp, không lộ mật khẩu/token.
>
> **Ví dụ với bộ IIT:** IIT user mở `UAT-HD-LAND-2026-001` của mình thì được, mở khách khác thì bị chặn; park admin mở khu khác thì bị chặn; audit phải ghi đủ vụ thanh toán 10 triệu `PAY-UAT-IIT-2026-10-001`.
>
> **Lỗi hay gặp:** tenant thấy được công ty khác (lỗi scope nghiêm trọng), nút tạo/sửa hiện sai chỗ, gõ URL trực tiếp vẫn vào được trang cấm, audit thiếu actor/timestamp hoặc lộ secret.

### UAT-09.01 User vận hành KCN-01

1. Đăng xuất root admin.
2. Đăng nhập `uat.operator.kcn01@ipms.local`.
3. Kiểm tra chỉ thấy dữ liệu trong `Khu công nghiệp UAT`.
4. Thử mở dữ liệu KCN khác bằng URL trực tiếp nếu có mã.
5. Kiểm tra menu quản trị hệ thống chỉ hiện khi role có quyền admin phù hợp.

Kết quả mong đợi: dữ liệu ngoài phạm vi bị chặn hoặc không hiển thị.

### UAT-09.02 User quản trị doanh nghiệp IIT

1. Đăng nhập `uat.iit.admin@ipms.local`.
2. Kiểm tra chỉ thấy dữ liệu liên quan `UAT_KCN-01`.
3. Thử tìm khách hàng khác (`UAT-CUS-20260917-01` nếu đã tạo ở UAT-02.02).
4. Thử mở `/admin` và `/admin/users/new` bằng URL trực tiếp.

Kết quả mong đợi: tenant không thấy dữ liệu doanh nghiệp khác và không vào được chức năng quản trị người dùng.

### UAT-09.03 User doanh nghiệp IIT thường

1. Đăng nhập `uat.iit.user@ipms.local`.
2. Kiểm tra chỉ thấy dữ liệu liên quan `UAT_KCN-01`.
3. Thử mở khách hàng khác, `/customers/new` hoặc màn hình sửa khách hàng bằng URL trực tiếp.

Kết quả mong đợi: dữ liệu ngoài doanh nghiệp bị chặn hoặc trả không tìm thấy; nút tạo/sửa/lưu trữ khách hàng không hiển thị.

### UAT-09.04 Audit hành động nhạy cảm

1. Đăng nhập lại root admin.
2. Vào `Quản trị hệ thống` -> `Nhật ký` hoặc audit.
3. Lọc các action: login, tạo user, tạo khách hàng, tạo hợp đồng, thanh toán, export, run billing `BR-UAT-KTP-T09-001`.

Kết quả mong đợi: audit có actor, action, entity, timestamp; không lộ mật khẩu/token/API key.

---

## 14. Điều kiện sign-off UAT

### 14.1 Điều kiện đủ để đề xuất ký UAT

QA/Test Lead chỉ đề xuất ký UAT khi các điều kiện sau được đáp ứng:

1. Toàn bộ case bắt buộc trong tài liệu này đã có kết quả và bằng chứng.
2. Không còn lỗi `Blocker` hoặc `High` chưa có phương án xử lý/được chấp nhận rủi ro bằng văn bản.
3. Các case liên quan tiền, công nợ, phân quyền và dữ liệu liên module đã được đại diện nghiệp vụ xác nhận (đối chiếu `TBP/REC/PAY-UAT-*`, `DH-*-UAT-001`, phân quyền `UAT-KCN-01`/`UAT_KCN-01`).
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

> Mẹo Notion: sau khi import, bôi đen bảng này và bảng 14.3 → Turn into → Board view để tick theo dõi từng hạng mục.

### 14.3 Phê duyệt lưu hành

| Vai trò | Họ tên | Chữ ký/Xác nhận | Ngày |
| --- | --- | --- | --- |
| BA Owner |  |  |  |
| QA/Test Lead |  |  |  |
| PM/PO |  |  |  |
| Đại diện nghiệp vụ khách hàng |  |  |  |

Tài liệu có hiệu lực cho vòng UAT web MVP kể từ ngày ban hành ở mục `Kiểm soát tài liệu`. Mọi thay đổi sau khi phát hành phải được ghi nhận trong lịch sử thay đổi và được QA/Test Lead xác nhận trước khi sử dụng lại cho tester.

---

## 15. Cấu hình quyền cho từng vai trò — viết cho người không kỹ thuật

> Mục này đọc độc lập được. Không cần biết code, API hay database. Chỉ cần biết đăng nhập web và đối chiếu với bộ seed `UAT-KCN-01 / Công ty cổ phần IIT` trong `documents/uat-manual-seed-data.md`.

### 15.1 Hiểu nhanh 2 khái niệm: vai trò và phạm vi

Hãy hình dung IPMS như một khu công nghiệp có nhiều cổng và nhiều phòng:

- **Vai trò (role)** = chiếc thẻ nhân viên của bạn. Thẻ ghi bạn là ai: ban quản lý toàn hệ thống, ban quản lý khu, nhân viên khu, quản trị công ty thuê, nhân viên công ty thuê.
- **Phạm vi (scope)** = khu vực mà thẻ của bạn mở được cửa. Có 3 loại phạm vi trong hệ thống:
  - `Toàn hệ thống (system)`: mở mọi cửa, mọi khu, mọi công ty. Chỉ dành cho quản trị cao nhất.
  - `Khu công nghiệp (park)`: chỉ mở được cửa trong 1 khu được giao, ví dụ `Khu công nghiệp UAT (UAT-KCN-01)`. Không nhìn thấy khu khác.
  - `Doanh nghiệp (customer)`: chỉ mở được cửa của đúng 1 công ty trong 1 khu, ví dụ `Công ty cổ phần IIT (UAT_KCN-01)` trong `UAT-KCN-01`. Không nhìn thấy công ty khác.

Nguyên tắc vàng: **thấy ít là đúng, thấy nhiều là sai**. Nếu bạn là công ty IIT mà lại thấy được công ty khác, đó là lỗi phải báo ngay.

### 15.2 Có những vai trò nào trong đợt UAT này

Hệ thống có 5 vai trò chuẩn. Đợt UAT này dùng 4 vai trò dưới đây (vai trò còn lại `PARK_STAFF` hiểu tương tự `PARK_ADMIN` nhưng quyền thấp hơn, khi nào cần thì QA sẽ cấp):

| Vai trò | Hiểu nôm na | Tài khoản UAT tương ứng | Phạm vi |
| --- | --- | --- | --- |
| `ROOT_ADMIN` | Ban quản lý toàn hệ thống | `admin@ipms.local` | Toàn hệ thống |
| `PARK_ADMIN` | Ban quản lý khu UAT | `uat.operator.kcn01@ipms.local` | Khu `UAT-KCN-01` |
| `ENTERPRISE_ADMIN` | Giám đốc công ty IIT | `uat.iit.admin@ipms.local` | Công ty `UAT_KCN-01` trong khu `UAT-KCN-01` |
| `ENTERPRISE_USER` | Nhân viên công ty IIT | `uat.iit.user@ipms.local` | Công ty `UAT_KCN-01` trong khu `UAT-KCN-01` |
| `PARK_STAFF` | Nhân viên khu (khi cần) | Tạo thêm nếu cần, ví dụ `uat.staff.kcn01@ipms.local` | Khu `UAT-KCN-01` |

> Cả 3 tài khoản `uat.*` đều do `admin@ipms.local` tạo trên màn hình `Quản trị hệ thống -> Người dùng` theo đúng UAT-01.03/UAT-01.04. Mật khẩu quy ước `Demo@123456`. Nếu không đăng nhập được thì ghi `Blocked by credential`, đừng tự đoán mật khẩu.

### 15.3 Từng vai trò được thấy gì, được làm gì

#### A. `ROOT_ADMIN` — `admin@ipms.local` (quản trị toàn hệ thống)

- **Được thấy:** mọi khu, mọi cụm (`UAT-LO-A/B`), mọi lô (`UAT-KCN-KD-01`), hạ tầng (`UAT-KCN-HT-01`), tài sản (`UAT_ASSET1`), mọi khách hàng (IIT + khách tester tự tạo), mọi hợp đồng (`UAT-HD-*`), kỳ tính phí (`UAT-KTP-T09`), đồng hồ (`DH-*-UAT-001`), thông báo phí/phải thu/thanh toán (`TBP/REC/PAY-UAT-*`), ticket, báo cáo, nhật ký.
- **Được làm:** tạo/sửa user mọi phạm vi, tạo KCN/cụm/phòng ban, tạo khách/hợp đồng/kỳ/biểu giá/đồng hồ, chạy tính phí, ghi nhận thanh toán, xuất báo cáo, xem nhật ký, cấu hình hệ thống (ví dụ email SMTP).
- **Không nên làm hàng ngày:** đừng dùng root để nhập liệu nghiệp vụ thường xuyên. Root chỉ dùng để tạo user, kiểm tra tổng và xem nhật ký. Dùng xong thì đăng xuất.
- **Tự kiểm tra 1 phút:** đăng nhập root, vào `Quản trị hệ thống`, tìm `UAT-KCN-01` và `UAT_KCN-01` đều thấy. Vào `Tài chính` tìm `REC-UAT-KTP-T09-IIT` thấy được. Như vậy là đúng.

#### B. `PARK_ADMIN` — `uat.operator.kcn01@ipms.local` (ban quản lý khu UAT)

- **Được thấy:** chỉ trong `Khu công nghiệp UAT (UAT-KCN-01)`. Thấy cụm `UAT-LO-A/B`, lô `UAT-KCN-KD-01`, GIS `GIS-UAT-KCN-KD-01`, camera `UAT-KCN-HT-01`, tài sản `UAT_ASSET1`, khách IIT, 4 hợp đồng `UAT-HD-*`, kỳ `UAT-KTP-T09`, phải thu/thanh toán của IIT, ticket của khu.
- **Được làm:** tạo user cho khu mình và cho doanh nghiệp trong khu mình (`PARK_ADMIN`, `PARK_STAFF`, `ENTERPRISE_ADMIN`, `ENTERPRISE_USER`), tạo khách/hợp đồng/đồng hồ/kỳ trong khu mình, xử lý ticket, xuất báo cáo lọc khu mình.
- **Không được làm:** không tạo được `ROOT_ADMIN`, không tạo user toàn hệ thống, không xem/sửa dữ liệu khu khác (nếu gõ URL khu khác thì phải bị chặn hoặc báo không tìm thấy).
- **Ví dụ đúng:** đăng nhập park admin, lọc khu `UAT-KCN-01` thì thấy IIT và `REC-UAT-KTP-T09-IIT`. Thử tìm khách `UAT-CUS-20260917-01` (nếu tester đã tạo trong khu này) thì vẫn thấy vì cùng khu.
- **Ví dụ sai cần báo lỗi:** park admin mà thấy được khu khác, hoặc tự tạo được user `ROOT_ADMIN`. Gặp trường hợp này thì ghi `Fail` + chụp màn hình.

#### C. `ENTERPRISE_ADMIN` — `uat.iit.admin@ipms.local` (quản trị công ty IIT)

- **Được thấy:** chỉ đúng công ty mình `UAT_KCN-01` trong khu `UAT-KCN-01`. Thấy lô `UAT-KCN-KD-01` mà công ty mình thuê, hợp đồng `UAT-HD-*` của mình, đồng hồ `DH-*-UAT-001` của mình, thông báo phí `TBP-UAT-KTP-T09-IIT`, phải thu `REC-UAT-KTP-T09-IIT`, ticket do mình gửi.
- **Được làm:** xem/sửa thông tin công ty mình (tùy cấu hình), gửi ticket mới, xem công nợ và thanh toán của mình, tải hợp đồng/hóa đơn/báo cáo của mình, tạo thêm `ENTERPRISE_USER` cùng công ty mình.
- **Không được làm:** không vào `Quản trị hệ thống` tạo user khu/hệ thống, không thấy công ty khác (ví dụ `UAT-CUS-20260917-01`), không thấy tiền của công ty khác, không mở được trang `/admin` hay `/admin/users/new` (phải bị chặn), không vào được mua hàng nhà cung cấp phía khu (supplier payables).
- **Tự kiểm tra 1 phút:** đăng nhập IIT admin, tìm `UAT_KCN-01` thì thấy; tìm khách khác thì không thấy. Mở `UAT-HD-LAND-2026-001` của mình thì được; mở hợp đồng công ty khác thì phải báo không tìm thấy hoặc không có quyền.

#### D. `ENTERPRISE_USER` — `uat.iit.user@ipms.local` (nhân viên công ty IIT)

- **Được thấy:** giống IIT admin nhưng ít hơn. Chỉ xem dữ liệu công ty `UAT_KCN-01`: hợp đồng, công nợ, ticket, thông báo của mình.
- **Được làm:** xem, gửi ticket, xem tiến độ xử lý, tải file của công ty mình.
- **Không được làm:** không tạo/sửa khách hàng (nút `Thêm khách hàng` / `Lưu` phải ẩn), không tạo user, không vào `/customers/new` hay màn hình sửa khách hàng bằng đường dẫn gõ tay (phải bị chặn), không thấy/sửa dữ liệu công ty khác.
- **Tự kiểm tra 1 phút:** đăng nhập IIT user, vào `Quản lý khách hàng` thì không thấy nút tạo mới. Gõ tay `/customers/new` thì phải bị chặn. Mở `TN`/`CUS-BA` hay khách khác thì không được.

### 15.4 Ma trận nhanh: ai được làm gì với bộ UAT-KCN-01

Dấu `Được` = phải làm được; `Không` = phải bị chặn hoặc bị ẩn nút. Nếu ngược lại thì đó là lỗi.

| Việc cần kiểm tra | `ROOT_ADMIN` | `PARK_ADMIN` (khu UAT) | `ENTERPRISE_ADMIN` (IIT) | `ENTERPRISE_USER` (IIT) |
| --- | --- | --- | --- | --- |
| Xem KCN `UAT-KCN-01`, cụm `UAT-LO-A/B` | Được | Được (chỉ khu mình) | Được (chỉ để biết mình thuộc khu nào) | Được (chỉ xem) |
| Xem lô `UAT-KCN-KD-01`, GIS `GIS-UAT-KCN-KD-01`, camera `UAT-KCN-HT-01`, tài sản `UAT_ASSET1` | Được | Được | Được (chỉ phần liên quan IIT) | Được (chỉ xem) |
| Xem/sửa khách `UAT_KCN-01` (IIT) | Được | Được | Được xem (sửa hạn chế) | Được xem |
| Xem khách công ty khác | Được | Được nếu cùng khu | Không | Không |
| Tạo khách hàng mới | Được | Được trong khu mình | Không | Không (nút phải ẩn) |
| Xem hợp đồng `UAT-HD-*` của IIT | Được | Được | Được (của IIT) | Được xem |
| Tạo hợp đồng mới | Được | Được trong khu mình | Không (hoặc hạn chế) | Không |
| Xem kỳ `UAT-KTP-T09`, đồng hồ `DH-*-UAT-001`, biểu giá `BG-*-UAT-001` | Được | Được | Được xem của IIT | Được xem |
| Xem `TBP/REC-UAT-KTP-T09-IIT`, tạo `PAY-UAT-*` | Được | Được | Được xem của IIT | Được xem |
| Xem tiền/NCC phía khu (supplier payables) | Được | Được | Không | Không |
| Tạo ticket cho IIT (`TCK-UAT-*`) | Được | Được | Được | Được |
| Xem ticket công ty khác | Được | Được trong khu | Không | Không |
| Vào `/admin`, `/admin/users/new` | Được | Hạn chế (chỉ tạo user khu/doanh nghiệp, không tạo root) | Không | Không |
| Tạo user `ROOT_ADMIN` | Được (chỉ root) | Không | Không | Không |
| Xem nhật ký/audit | Được | Được trong khu (tùy quyền) | Không hoặc rất hạn chế | Không |

### 15.5 Cách tạo user đúng để không bị lỗi quyền (dành cho người giao UAT)

Chỉ `admin@ipms.local` mới tạo được đủ 3 loại user. Làm đúng 4 bước này trên màn hình `Quản trị hệ thống -> Người dùng -> Tạo người dùng`:

1. **Chọn vai trò trước:** muốn tạo ai thì chọn đúng thẻ: `PARK_ADMIN` cho ban quản lý khu, `ENTERPRISE_ADMIN` cho giám đốc IIT, `ENTERPRISE_USER` cho nhân viên IIT.
2. **Chọn khu bằng ô chọn sẵn (select), đừng gõ tay:** ô `Park ID` phải hiện chữ `Khu công nghiệp UAT`, đừng gõ mã `UAT-KCN-01` hay dãy số dài. Nếu ô này bắt gõ tay thì báo lỗi UAT-01.03.
3. **Nếu là user doanh nghiệp thì chọn công ty bằng ô chọn sẵn:** ô `Customer ID` phải hiện chữ `Công ty cổ phần IIT`, danh sách này phải tự lọc theo khu đã chọn ở bước 2. Nếu không lọc thì báo lỗi UAT-01.04.
4. **Lưu rồi mở lại để kiểm tra:** mở chi tiết user vừa tạo, xem đúng vai trò + đúng khu/công ty chưa. Đăng nhập thử 1 lần bằng user đó.

Ai được tạo ai (nhớ để không giao sai):

- Root tạo được tất cả, kể cả root khác.
- Park admin chỉ tạo được `PARK_ADMIN`, `PARK_STAFF`, `ENTERPRISE_ADMIN`, `ENTERPRISE_USER` trong khu mình. Không tạo được root.
- IIT admin chỉ tạo được `ENTERPRISE_ADMIN`, `ENTERPRISE_USER` cùng công ty IIT. Không tạo được user khu hay root.

Lỗi hay gặp khi tạo user:

| Hiện tượng | Nghĩa đơn giản | Ghi kết quả thế nào |
| --- | --- | --- |
| Báo `400 BAD_REQUEST` khi lưu | Form thiếu hoặc sai trường bắt buộc, hoặc chọn sai phạm vi | `Fail`, ghi rõ đã nhập gì, vai trò gì, khu/công ty gì |
| Ô khu/công ty bắt gõ mã dài | UI chưa làm ô chọn, dễ gõ sai | `Fail` UAT-01.03/01.04 |
| Tạo được user nhưng đăng nhập báo sai mật khẩu | Mật khẩu seed bị đổi khi rebuild | `Blocked by credential`, báo QA |
| Park admin tạo được root | Lỗi phân quyền nghiêm trọng | `Fail`, mức Blocker, báo ngay |

### 15.6 Tự kiểm tra quyền trong 10 phút với bộ IIT (không cần kỹ thuật)

Làm đúng thứ tự này, mỗi bước chụp 1 ảnh:

1. Đăng nhập root, mở `UAT-KCN-KD-01`, `UAT-HD-LAND-2026-001`, `REC-UAT-KTP-T09-IIT` đều thấy -> `Pass`.
2. Đăng xuất, đăng nhập park admin `uat.operator.kcn01@`. Vào lại 3 màn hình trên vẫn thấy. Thử gõ URL khu khác (nếu có) -> phải bị chặn hoặc không thấy -> `Pass` nếu bị chặn.
3. Đăng xuất, đăng nhập IIT admin `uat.iit.admin@`. Tìm `UAT_KCN-01` thấy; tìm khách khác không thấy. Mở `/admin` -> phải bị chặn. Mở `UAT-HD-SVC-2026-001` của mình -> thấy.
4. Đăng xuất, đăng nhập IIT user `uat.iit.user@`. Vào `Quản lý khách hàng` -> không thấy nút tạo mới. Gõ tay `/customers/new` -> phải bị chặn. Mở hợp đồng IIT -> chỉ xem.
5. Đăng nhập lại root, vào `Nhật ký`/audit, lọc hành động đăng nhập/tạo user/tạo khách/thanh toán/export -> thấy đủ ai làm, làm gì, lúc nào, không lộ mật khẩu.

Nếu bước nào ngược lại (ví dụ IIT user lại tạo được khách, IIT admin lại mở được `/admin`, park admin lại thấy khu khác) thì ghi `Fail` + chụp ảnh + ghi URL + giờ test.

### 15.7 Đọc thông báo lỗi quyền mà không sợ

Hệ thống chặn sai phạm vi bằng mã lỗi. Người không kỹ thuật chỉ cần nhớ bảng này:

| Mã hệ thống báo | Hiểu đơn giản | Có phải lỗi không |
| --- | --- | --- |
| `403 park_scope_forbidden` | Bạn đang cố mở dữ liệu khu khác, hệ thống chặn đúng | `Pass` cho case phân quyền (chặn đúng là tốt) |
| `403 customer_scope_forbidden` | Bạn đang cố mở dữ liệu công ty khác, hệ thống chặn đúng | `Pass` cho case phân quyền |
| `403 permission_denied` | Thẻ của bạn không có quyền làm việc này | `Pass` nếu đúng là việc bạn không được làm; `Fail` nếu đó là việc bạn phải làm được |
| `403 provisioning_scope_forbidden` | Bạn đang cố tạo user to hơn quyền của mình (ví dụ park admin tạo root) | `Pass` nếu bị chặn |
| `400 park_scope_required` | Thiếu chọn khu, hãy chọn `UAT-KCN-01` rồi thử lại | Thường là thao tác thiếu, không phải lỗi hệ thống |
| Vào trang trắng hoặc `500` | Hệ thống lỗi thật | `Fail`, ghi URL + giờ + tài khoản |

---

### Phụ lục A. Đối chiếu bản 1.1 → 1.2 (lịch sử)

| Nhóm | Bản 1.1 (Trà Nóc/Bắc An, đã bỏ) | Bản 1.2 (seed tay, đang dùng) |
| --- | --- | --- |
| KCN/cụm | `KCN-TRA-NOC`, `TRA-NOC-A/B`, `IPMS-PARK-01`, `BAC-AN-A/B` | `UAT-KCN-01`, `UAT-LO-A/B` |
| Khách | `TN-CUST-001..005`, `CUS-BA-*`, `CUS-T05-BA-001` | `UAT_KCN-01` (IIT) + `UAT-CUS-20260917-01` tự tạo |
| Hợp đồng | `TN-CON-*`, `HD-BA-*` | `UAT-HD-01`, `UAT-HD-LAND-2026-001`, `UAT-HD-ASSET-2026-001`, `UAT-HD-SVC-2026-001` |
| Lô/GIS/hạ tầng | `TN-LOT-*/FAC-*/INF-*`, `GIS-T07-*` | `UAT-KCN-KD-01`, `GIS-UAT-KCN-LOT`, `GIS-UAT-KCN-KD-01`, `UAT-KCN-HT-01`, `UAT_ASSET1` |
| Billing | `BP-*/TRF-*/MTR-*/T09-*/FN-*` | `UAT-KTP-T09`, `BG-*-UAT-001`, `DH-*-UAT-001`, `BR-UAT-KTP-T09-001`, `TBP-UAT-KTP-T09-IIT` |
| Tài chính | `REC-TN-*`, `PAY-TN-*`, `AP-*/NCC-*` | `REC-UAT-KTP-T09-IIT`, `PAY-UAT-IIT-2026-10-001`, NCC tự tạo `UAT-SUP-*` |
| Ticket/notify | `TCK-TN-*`, `TCK-2026-*`, `TCK-T08-*` | Tự tạo `TCK-UAT-*` từ IIT |
| User | `uat.tranoc.*` | `uat.operator.kcn01@`, `uat.iit.*@` tự tạo |
| eInvoice/sign | `EINV-*/SIGN-*` Bắc An/T11/T12 | Tạo mới `EINV-UAT-*` / `SIGN-UAT-*` từ `TBP-UAT-*` / `UAT-HD-*` |

---

### Phụ lục B. Công thức tính điện, nước, nước thải, rác, dịch vụ hạ tầng, tiền thuê

> Đọc cho tester không kỹ thuật. Mọi công thức dưới đây dùng đúng mã seed `UAT-KCN-01 / IIT` (`BG-*-UAT-001`, `DH-*-UAT-001`, `UAT-KTP-T09`). Quy ước: số tiền làm tròn tới đồng (VND), VAT tính trên số trước VAT sau mọi phụ phí/phạt, tổng sau VAT = trước VAT + VAT. Hệ số nhân đồng hồ (`multiplier`) mặc định 1 với bộ IIT.

#### B.0 Công thức chốt chỉ số (dùng chung cho mọi đồng hồ)

- Sản lượng tổng = (chỉ số tổng mới − chỉ số tổng cũ) × hệ số nhân. Chú thích: số mới phải ≥ số cũ, nếu âm là lỗi nhập liệu, hệ thống phải chặn.
- Với đồng hồ điện 3 khung giờ, thêm 3 sản lượng thành phần, mỗi loại tính riêng rồi cộng lại:
  - Bình thường = (bình thường mới − bình thường cũ) × hệ số.
  - Cao điểm = (cao điểm mới − cao điểm cũ) × hệ số.
  - Thấp điểm = (thấp điểm mới − thấp điểm cũ) × hệ số.
  - Ràng buộc hệ thống kiểm tra: tổng 3 khung phải bằng sản lượng tổng. Nếu lệch thì báo lỗi `band deltas` và không cho lưu.
- Ví dụ IIT (`DH-DIEN-UAT-001`, hệ số 1, ngày 30/09/2026): tổng 42150 − 32000 = 10150 kWh; bình thường 26200 − 20000 = 6200; cao điểm 9400 − 7000 = 2400; thấp điểm 6550 − 5000 = 1550; 6200 + 2400 + 1550 = 10150 nên hợp lệ.
- Nước (`DH-NUOC-UAT-001`): 1045 − 820 = 225 m3. Nước thải (`DH-NUOC-THAI-UAT-001`): 785 − 610 = 175 m3.

#### B.1 Tiền điện 3 khung giờ + phạt COSφ (`BG-DIEN-UAT-001`)

Áp dụng cho `service_type = electricity`, `calculation_mode = electricity_time_of_use`, thuế 8%.

```text
Tiền từng khung = sản lượng khung × đơn giá khung
  Bình thường = Q_normal × 1850
  Cao điểm   = Q_peak   × 3100
  Thấp điểm  = Q_offpeak × 1200
Tiền năng lượng = Bình thường + Cao điểm + Thấp điểm
Phạt COSφ = Tiền năng lượng × 5%  nếu (bật phạt VÀ COSφ đo được < 0.9)
          = 0                     nếu COSφ ≥ 0.9 hoặc tắt phạt
Trước VAT = Tiền năng lượng + Phạt COSφ
VAT       = Trước VAT × 8%
Tổng      = Trước VAT + VAT
```

- Chú thích COSφ: là hệ số công suất, đo chất lượng dùng điện. Ngưỡng 0.9 nghĩa là dưới 0.9 thì bị phạt. Tỷ lệ phạt 5% tính trên tiền năng lượng (mã nội bộ `flat_percentage_of_energy_amount`), không tính trên VAT. Chỉ số phản kháng kVArh (ví dụ 980) chỉ để tham khảo, không đưa vào công thức tiền.
- Ví dụ IIT bị phạt (COSφ 0.86 < 0.9): 6200×1850 = 11470000; 2400×3100 = 7440000; 1550×1200 = 1860000; năng lượng = 20770000; phạt = 20770000×5% = 1038500; trước VAT = 21808500; VAT = 21808500×8% = 1744680; tổng = 23553180.
- Ví dụ không phạt (bộ seed cũ, COSφ 0.92 ≥ 0.9): tổng 18680 − 12500 = 6180 kWh thì phạt = 0, trước VAT = tiền năng lượng, chỉ cộng VAT 8%.
- Tester đối chiếu ở UAT-05.04: mở đồng hồ `DH-DIEN-UAT-001` kiểm tra 4 số (tổng + 3 khung + COSφ), sau đó đối chiếu dòng tiền điện trong run `BR-UAT-KTP-T09-001` theo đúng 5 bước trên.

#### B.2 Tiền nước sạch (`BG-NUOC-UAT-001`)

Bộ IIT đang dùng giá phẳng (flat), thuế 5%. Nếu sau này có bậc thang (tiered) thì dùng công thức bậc thang bên dưới.

```text
Dạng phẳng (đang dùng cho IIT):
Trước VAT = sản lượng m3 × 12500
VAT       = Trước VAT × 5%
Tổng      = Trước VAT + VAT
Ví dụ IIT: 225 × 12500 = 2812500; VAT = 140625; tổng = 2953125.
(Kết quả seed mục 3.7 ghi 2812500/140625/2953125 khớp công thức này.)

Dạng bậc thang (khi có tariff_tiers, để tester hiểu nếu gặp ở môi trường khác):
Trước VAT = 500×11800 + phần vượt×13200  (ví dụ 620 m3 = 500×11800 + 120×13200 = 7484000)
VAT = Trước VAT × 5%; Tổng = Trước VAT + VAT.
```

- Chú thích: nước không có khung giờ, không có COSφ. Hệ thống từ chối biểu giá nước có `time_band_rates` hoặc thiếu giá phẳng.
- Tester đối chiếu ở UAT-05.04: chỉ số 820→1045 = 225 m3 rồi nhân 12500.

#### B.3 Tiền xử lý nước thải (`BG-NUOC-THAI-UAT-001`)

Tương tự nước sạch, thuế 5%, không khung giờ, không COSφ.

```text
Dạng phẳng (đang dùng cho IIT):
Trước VAT = sản lượng m3 × 8200
VAT       = Trước VAT × 5%
Tổng      = Trước VAT + VAT
Ví dụ IIT: 175 × 8200 = 1435000; VAT = 71750; tổng = 1506750.
(Khớp số seed mục 3.7: 1435000/71750/1506750.)

Dạng bậc thang (tham khảo): 500×8200 + phần vượt×9100, ví dụ 590 m3 = 500×8200 + 90×9100 = 4919000, rồi cộng VAT 5%.
```

#### B.4 Tiền rác thải (`BG-RAC-UAT-001`)

Bộ IIT dùng theo khối lượng (`calculation_mode = waste_volume`), thuế 5%. Lấy khối lượng đầu vào (input), không lấy đầu ra.

```text
Trước VAT = khối lượng đầu vào (kg) × 1500
VAT       = Trước VAT × 5%
Tổng      = Trước VAT + VAT
Ví dụ chính IIT (industrial, 30/09/2026): 1250 × 1500 = 1875000; VAT = 93750; tổng = 1968750.
Ví dụ thêm: domestic 620 × 1500 = 930000; hazardous 85 × 1500 = 127500 (mỗi loại tính riêng nếu có nhiều bản ghi).
```

- Chú thích: phương pháp xử lý (`sorting/composting/incineration`) và khối lượng đầu ra (1180/590/80) chỉ để theo dõi vận hành, không đưa vào công thức tiền. Một số môi trường khác có thể dùng phí cố định theo khách (`fixed_per_customer`, ví dụ 220000/khách/tháng) — khi đó trước VAT = `base_fee`, không nhân khối lượng.
- Tester đối chiếu ở UAT-05.04: kiểm tra ngày vận hành + loại rác + khối lượng vào, sau đó đối chiếu dòng rác trong run.

#### B.5 Phí dịch vụ hạ tầng chung (`BG-DVHT-UAT-001` / dòng 350000 trong seed)

Phí cố định theo kỳ (fixed), thuế 5%.

```text
Trước VAT = 350000 (base_fee theo tháng/kỳ, từ hợp đồng UAT-HD-ASSET-2026-001)
VAT       = 350000 × 5% = 17500
Tổng      = 367500
```

- Chú thích: không phụ thuộc đồng hồ hay khối lượng. Nếu có phân bổ theo diện tích hợp đồng (`allocation_basis = contract_area`) thì hệ thống chia theo m2, nhưng bộ IIT hiện tại là số cố định nên tester chỉ cần đối chiếu 350000.

#### B.6 Tiền thuê đất / nhà xưởng / tài sản (từ hợp đồng `UAT-HD-*`)

Lấy từ line item hợp đồng (`calculation_mode = contract_line`), VAT theo điều khoản hợp đồng (các hợp đồng IIT là 10%).

```text
Trước VAT = diện tích/số lượng × đơn giá (theo dòng thuê trong hợp đồng)
VAT       = Trước VAT × VAT hợp đồng
Tổng      = Trước VAT + VAT
Ví dụ UAT-HD-LAND-2026-001: 10000 m2 × 85000 = 850000000; VAT 10% = 85000000; tổng = 935000000.
Ví dụ UAT-HD-01: 10000 × 100 = 1000000; VAT 10% = 100000; tổng = 1100000.
Ví dụ UAT-HD-ASSET-2026-001: 1 × 3500000 = 3500000; VAT 10% = 350000; tổng = 3850000.
Ví dụ UAT-HD-SVC-2026-001: 3 dòng dịch vụ 11433000 + 2812500 + 1875000 = 16120500 trước VAT (VAT tính riêng từng dòng 10%/5%/5%).
```

- Chú thích: tiền cọc (ví dụ 1700000000) không cộng vào tiền thuê kỳ này. Chu kỳ (tháng/quý) và ngày đến hạn (ngày 10/15) chỉ quyết định kỳ nào phải trả, không đổi đơn giá.

#### B.7 Tổng hợp run, thông báo phí, phải thu

```text
Run BR-UAT-KTP-T09-001 (trạng thái completed):
Tổng trước VAT = Điện + Nước + Nước thải + Rác + Dịch vụ hạ tầng (+ tiền thuê nếu kỳ có)
Tổng VAT       = VAT từng dòng cộng lại
Tổng phải thu  = Tổng trước VAT + Tổng VAT
Thông báo phí TBP-UAT-KTP-T09-IIT: phát hành 01/10/2026, đến hạn 15/10/2026, trạng thái issued, mang 3 số trên.
Phải thu REC-UAT-KTP-T09-IIT = Tổng phải thu, ban đầu open.
Thanh toán PAY-UAT-IIT-2026-10-001: 10000000 vào REC → đã thu 10000000, còn 10756205, trạng thái partial.
Không cho phân bổ vượt số còn lại; kỳ đã đóng (closed) thì không cho sửa/chạy lại.
```

- Chú thích số seed: mục 3.7 ghi bộ run kỳ vọng điện 12926000 / nước 2812500 / nước thải 1435000 / rác 1875000 / hạ tầng 350000 → trước VAT 19398500, VAT 1357705, tổng 20756205. Bộ này dùng đầu vào khác bộ COSφ phạt ở B.1 (điện 21808500 trước VAT). Khi test live ra số khác seed thì lấy số live tính đúng công thức trên làm chuẩn, ghi chú "số live theo B.1, khác ví dụ seed 3.7 do khác đầu vào", đừng báo Fail vội. Tester UAT-05.03/UAT-05.04 đối chiếu theo 5 bước B.0→B.7 thay vì học thuộc con số.
