# ĐẶC TẢ YÊU CẦU PHẦN MỀM (SRS)

## Hệ thống quản lý chuỗi quán cà phê — Ứng dụng Desktop WPF

| Thông tin tài liệu | Giá trị |
|---|---|
| Mã tài liệu | SRS-COFFEE-001 |
| Phiên bản | 0.1 — Bản nháp để xác nhận yêu cầu |
| Ngày lập | 26/10/2026 |
| Nền tảng | Windows Desktop — C#/.NET, Windows Presentation Foundation (WPF) |
| Phạm vi | Đồ án môn Lập trình trực quan (IT008) |
| Trạng thái | **Chưa chốt**: các điểm `[ĐX]` cần được thống nhất trước khi thiết kế CSDL và lập trình |
| Tài liệu nguồn | `Report.md`; `DanhGiaDoAnIT008.pdf` (bảng tiêu chí chấm đồ án) |

> **Quy ước truy vết:** `[G]` = yêu cầu gốc có trong `Report.md`; `[ĐX]` = nội dung được đề xuất để hệ thống khả thi, nhất quán và kiểm thử được; `[CT]` = yêu cầu kỹ thuật đến từ tiêu chí chấm điểm; `[CHỐT]` = quyết định bắt buộc hỏi nhóm trước khi triển khai. Tài liệu này lấy cảm hứng từ cấu trúc SRS của ISO/IEC/IEEE 29148, **không tuyên bố đã được chứng nhận hoặc tuân thủ đầy đủ tiêu chuẩn**.

---

# 1. Giới thiệu

## 1.1. Mục đích

Đặc tả các yêu cầu nghiệp vụ, chức năng, dữ liệu, giao diện, chất lượng và tiêu chí nghiệm thu cho ứng dụng **quản lý chuỗi quán cà phê** chạy trên Windows bằng WPF. Đây là cơ sở thống nhất giữa thành viên nhóm, người thiết kế UI, người lập trình, người kiểm thử và giảng viên khi đánh giá đồ án.

## 1.2. Bối cảnh và vấn đề cần giải quyết

Chủ chuỗi quán cần nhìn được doanh thu, chi phí, lợi nhuận và hiệu quả hoạt động của nhiều chi nhánh. Nhân viên tại từng quán cần xem thực đơn, tạo đơn, xác nhận thanh toán mô phỏng và theo dõi kết quả bán hàng của chi nhánh. Dữ liệu phải có cấu trúc, thống nhất quyền truy cập và có thể truy vết kết quả thống kê về đơn hàng đã thanh toán.

## 1.3. Mục tiêu và kết quả đầu ra

- `[G]` Một ứng dụng WPF có màn hình đăng nhập, đăng xuất và chuyển giao diện theo vai trò **Admin / Staff**.
- `[G]` Admin xem dashboard toàn chuỗi; thống kê từng chi nhánh; quản lý chi nhánh và thực đơn chung.
- `[G]` Staff xem tổng quan chi nhánh, menu/tồn kho và thực hiện order; chọn **“Đã thanh toán”** hoặc **“Hủy”**.
- `[ĐX]` Bổ sung tối thiểu các quy trình duy trì tính đúng đắn dữ liệu: tài khoản nhân viên, tồn kho theo chi nhánh, khoản chi và trạng thái bản ghi.
- `[CT]` Minh chứng được CRUD, xử lý nhập liệu sai, WPF Layout/Resource, MVVM, Binding, Command, OOP, Git, teamwork và demo.

## 1.4. Phạm vi MVP (bắt buộc cho bản nộp)

1. Đăng nhập/đăng xuất; phân quyền Admin và Staff.
2. Dashboard Admin: doanh thu, chi phí, lợi nhuận, số chi nhánh hoạt động, biểu đồ xu hướng.
3. Thống kê doanh thu theo chi nhánh, biểu đồ tỷ trọng doanh thu.
4. Quản lý chi nhánh (tạo, xem, cập nhật, ngừng hoạt động; tìm kiếm/lọc).
5. Quản lý sản phẩm/thực đơn dùng chung (tạo, xem, sửa, ngừng bán) và top 5 bán chạy.
6. Dashboard Staff theo chi nhánh, xem menu và lượng hàng còn lại.
7. Tạo đơn hàng, xác nhận thanh toán mô phỏng hoặc hủy đơn.
8. `[ĐX]` Quản lý tồn kho theo chi nhánh, tài khoản Staff và ghi nhận chi phí tối thiểu, để các số liệu vận hành có nguồn dữ liệu rõ ràng.
9. Kiểm tra đầu vào, thông báo lỗi, truy vấn CSDL, dữ liệu nhất quán.

## 1.5. Ngoài phạm vi MVP

Không tích hợp cổng thanh toán thật, không quét QR thực thu, không quản lý nhà cung cấp, không tích điểm khách hàng, không hóa đơn điện tử, không app mobile/web, không giao hàng, không AI dự báo, không quản trị đa công ty, không đồng bộ offline và không đổi trả/hoàn tiền sau khi đơn đã thanh toán. Các mục này chỉ đưa vào khi nhóm chủ động mở rộng phạm vi.

# 2. Nguồn yêu cầu, vai trò và giả định

## 2.1. Vai trò sử dụng

| Vai trò | Phạm vi dữ liệu | Quyền chính |
|---|---|---|
| **Admin** | Toàn bộ chuỗi | Xem mọi dashboard; quản lý chi nhánh, sản phẩm, tài khoản Staff, tồn kho và khoản chi (`[ĐX]` cho ba nhóm sau). |
| **Staff** | Một chi nhánh được gán | Xem dashboard/menu/tồn kho chi nhánh; lập đơn, xác nhận thanh toán hoặc hủy đơn. |
| **Khách hàng** | Không đăng nhập hệ thống | Chỉ là chủ thể giao dịch ngoài hệ thống; không có màn hình riêng. |

**Giả định `[ĐX]`:** một tài khoản Staff thuộc đúng một chi nhánh tại một thời điểm; menu và giá niêm yết giống nhau ở mọi chi nhánh, nhưng tồn kho độc lập cho từng chi nhánh. Nếu điều này không đúng, phải sửa dữ liệu/permissions trước khi lập trình.

## 2.2. Ma trận quyền

| Tác vụ | Admin | Staff |
|---|:---:|:---:|
| Đăng nhập, đăng xuất | ✓ | ✓ |
| Xem báo cáo toàn chuỗi / mọi chi nhánh | ✓ | — |
| Tạo, cập nhật, vô hiệu hóa chi nhánh | ✓ | — |
| Quản lý menu và giá bán chung | ✓ | Xem |
| Xem top 5 sản phẩm bán chạy toàn chuỗi | ✓ | — |
| Điều chỉnh tồn kho từng chi nhánh `[ĐX]` | ✓ | Chỉ xem |
| Quản lý tài khoản Staff `[ĐX]` | ✓ | — |
| Ghi nhận khoản chi `[ĐX]` | ✓ | — |
| Lập đơn và xác nhận thanh toán | — | ✓, tại chi nhánh được gán |
| Hủy đơn chưa thanh toán | — | ✓, tại chi nhánh được gán |
| Xem doanh thu/số đơn của chi nhánh mình | ✓ | ✓ |

**Quy tắc:** không chỉ ẩn nút trên UI; tầng nghiệp vụ phải kiểm tra role và `BranchId` trước khi truy cập, sửa hay tạo dữ liệu.

## 2.3. Ràng buộc và giả định kỹ thuật

- `[G]` Ứng dụng là phần mềm desktop quản lý cà phê.
- `[CT]` Bắt buộc sử dụng WPF layout phù hợp, Binding, Command và mô hình MVVM để lấy trọn trọng số mục 3 theo phiếu chấm.
- `[ĐX]` Môi trường Windows có .NET runtime phù hợp; sử dụng C#/.NET và CSDL quan hệ; công nghệ CSDL **chưa xác nhận**.
- `[CHỐT]` Cần quyết định demo **một máy + CSDL cục bộ** hay **nhiều máy/chi nhánh + CSDL tập trung**. Nếu chạy đa chi nhánh qua Internet thật, nên dùng API/server để kiểm soát xác thực và dữ liệu; không mở trực tiếp cổng CSDL cho client ngoài Internet.
- `[ĐX]` Tiền tệ: VND; ngày nghiệp vụ và báo cáo theo giờ Việt Nam (Asia/Ho_Chi_Minh); phép tính tiền dùng kiểu `decimal`, không dùng `float/double`.

# 3. Quy tắc nghiệp vụ (Business Rules)

| Mã | Quy tắc | Nguồn |
|---|---|---|
| BR-01 | Có đúng hai vai trò ứng dụng ở MVP: Admin và Staff. | [G] |
| BR-02 | Chỉ tài khoản đang hoạt động mới được đăng nhập. | [ĐX] |
| BR-03 | Staff chỉ được xem/giao dịch tại chi nhánh đã gán. | [ĐX] |
| BR-04 | Menu gồm danh sách sản phẩm và giá bán chung cho mọi chi nhánh. | [G] |
| BR-05 | Số lượng còn lại được lưu theo **cặp chi nhánh–sản phẩm**, không phải một số tồn toàn chuỗi. | [ĐX] |
| BR-06 | Sản phẩm/chi nhánh có lịch sử giao dịch được **ngừng hoạt động (soft delete)** thay vì xóa vật lý; lịch sử phải được bảo toàn. | [ĐX] |
| BR-07 | Đơn hàng có các trạng thái `PENDING`, `PAID`, `CANCELLED`; chỉ `PENDING` chuyển thành `PAID` hoặc `CANCELLED`. | [ĐX] |
| BR-08 | Chỉ đơn `PAID` được tính doanh thu, lượng bán top 5 và trừ tồn kho; đơn `CANCELLED` không tính vào các chỉ số này. | [ĐX] |
| BR-09 | Khi xác nhận `PAID`, hệ thống phải kiểm tra lại tồn và ghi đơn + chi tiết + trừ tồn trong **một giao dịch CSDL**; không cho tồn âm. | [ĐX] |
| BR-10 | Giá từng dòng đơn được chụp tại lúc chốt đơn (`UnitPriceSnapshot`), không tính lại theo giá sản phẩm hiện tại. | [ĐX] |
| BR-11 | `OrderTotal = Σ (Quantity × UnitPriceSnapshot)`; số tiền không được âm. | [ĐX] |
| BR-12 | `Revenue = Σ OrderTotal` các đơn `PAID` có `PaidAt` nằm trong khoảng báo cáo. | [ĐX] |
| BR-13 | `Expense = Σ Amount` các khoản chi hợp lệ trong khoảng báo cáo; `Profit = Revenue − Expense` (không tự mặc định có số liệu vốn hàng). | [ĐX] |
| BR-14 | Bộ lọc ngày, chi nhánh trên tất cả thẻ thống kê/biểu đồ phải nhất quán cùng quy tắc tính. | [ĐX] |
| BR-15 | Điểm đánh giá chi nhánh trong khoảng 0–5; nguồn điểm ở MVP là nhập thủ công bởi Admin cho đến khi xác nhận nguồn khác. | [ĐX][CHỐT] |
| BR-16 | Giá/số lượng sản phẩm, khoản chi không được âm; số lượng mua là số nguyên dương. | [ĐX] |
| BR-17 | Không chỉnh sửa/hủy đơn `PAID` trong MVP; nếu cần hoàn tiền sẽ lập yêu cầu phiên bản tiếp theo. | [ĐX] |

**Lưu ý đặc biệt về lợi nhuận:** `Report.md` có thẻ **chi phí, lợi nhuận** nhưng chưa nêu cách tạo dữ liệu chi phí. Phần **Quản lý khoản chi** trong tài liệu này là đề xuất tối thiểu để chỉ số có thể kiểm chứng; không nên hiển thị lợi nhuận như một số liệu thực nếu nhóm chưa chọn nguồn chi phí.

# 4. Đặc tả yêu cầu chức năng

**Quy ước:** `M` = Must (MVP bắt buộc); `S` = Should (nên có để demo hoặc hỗ trợ nghiệp vụ). Tất cả các mục `[ĐX]` cần xác nhận; nếu từ chối một mục thì đồng thời điều chỉnh các FR/BR/kiểm thử liên quan.

## 4.1. Xác thực và điều hướng

| Mã | Mức | Yêu cầu có thể kiểm thử |
|---|:---:|---|
| FR-AUTH-01 | M | `[G]` Màn hình Login nhận username và password (password che ký tự). |
| FR-AUTH-02 | M | `[G]` Kiểm tra trống/độ dài/định dạng đầu vào trước truy vấn CSDL; thông báo rõ trường cần sửa. |
| FR-AUTH-03 | M | `[G]` Đối chiếu tài khoản trong CSDL; từ chối sai thông tin hoặc bị vô hiệu hóa, không tiết lộ phần nào sai. |
| FR-AUTH-04 | M | `[G]` Login thành công điều hướng theo role: Admin → Admin Shell; Staff → Staff Shell đúng chi nhánh. |
| FR-AUTH-05 | M | `[G]` Logout qua menu Account xóa trạng thái phiên ứng dụng và quay về Login. |
| FR-AUTH-06 | M | `[G]` Đóng cửa sổ ứng dụng kết thúc phiên chạy; không ghi password thô vào file/log. |
| FR-AUTH-07 | M | `[ĐX]` Cấm truy cập tác vụ trái quyền kể cả khi gọi trực tiếp service/repository. |

## 4.2. Admin — Trang tổng quan toàn chuỗi

| Mã | Mức | Yêu cầu có thể kiểm thử |
|---|:---:|---|
| FR-ADM-01 | M | `[G]` Hiển thị 4 thẻ: tổng doanh thu, tổng chi phí, lợi nhuận, số chi nhánh đang hoạt động. |
| FR-ADM-02 | M | `[G]` Hiển thị biểu đồ doanh thu và lợi nhuận theo thời gian. |
| FR-ADM-03 | M | `[ĐX]` Bộ lọc thời gian: hôm nay, 7 ngày, 30 ngày, khoảng tùy chọn; mọi thẻ và biểu đồ đồng bộ. |
| FR-ADM-04 | M | `[ĐX]` Khi chưa có dữ liệu, hiển thị 0 và trạng thái trống; không tạo số liệu minh họa giả trong vận hành. |

## 4.3. Admin — Thống kê hiệu suất chi nhánh

| Mã | Mức | Yêu cầu có thể kiểm thử |
|---|:---:|---|
| FR-RPT-01 | M | `[G]` Liệt kê từng chi nhánh cùng trạng thái và doanh thu theo khoảng thời gian chọn. |
| FR-RPT-02 | M | `[G]` Biểu đồ tròn thể hiện tỷ lệ doanh thu từng chi nhánh; tổng phần trăm bằng 100% khi có doanh thu dương. |
| FR-RPT-03 | S | `[ĐX]` Cho phép chọn một chi nhánh để xem doanh thu, số đơn đã thanh toán, chi phí và lợi nhuận của riêng chi nhánh. |
| FR-RPT-04 | M | `[ĐX]` Chi nhánh có doanh thu 0 vẫn xuất hiện trong bảng; không tạo lát biểu đồ có tỷ lệ không xác định. |
| FR-RPT-05 | S | `[ĐX]` Bộ lọc trạng thái hoạt động và khoảng thời gian được giữ nhất quán giữa bảng và biểu đồ. |

## 4.4. Admin — Quản lý chi nhánh

| Mã | Mức | Yêu cầu có thể kiểm thử |
|---|:---:|---|
| FR-BRN-01 | M | `[G]` Xem danh sách chi nhánh gồm tên, địa chỉ, trạng thái hoạt động và điểm đánh giá sao. |
| FR-BRN-02 | M | `[G]` Thêm chi nhánh, kiểm tra dữ liệu bắt buộc; lưu và hiển thị bản ghi mới. |
| FR-BRN-03 | M | `[ĐX][CT]` Chỉnh sửa tên, địa chỉ, trạng thái, điểm đánh giá của chi nhánh. |
| FR-BRN-04 | M | `[G][ĐX]` Lệnh “Xóa” ở UI được hiểu là **ngừng hoạt động** để không mất giao dịch cũ; hỏi xác nhận thao tác. |
| FR-BRN-05 | M | `[G]` Tìm kiếm/lọc chi nhánh theo tên, mức sao và trạng thái. |
| FR-BRN-06 | M | `[ĐX]` Không cho chuyển ngừng hoạt động nếu có đơn `PENDING`; hiển thị hướng xử lý đơn chờ. |

## 4.5. Admin — Quản lý sản phẩm / thực đơn chung

| Mã | Mức | Yêu cầu có thể kiểm thử |
|---|:---:|---|
| FR-PRO-01 | M | `[G]` Hiển thị danh sách món, tên, giá niêm yết và trạng thái bán. |
| FR-PRO-02 | M | `[G]` Thêm sản phẩm, kiểm tra tên không trống, giá hợp lệ. |
| FR-PRO-03 | M | `[ĐX][CT]` Sửa tên/giá/trạng thái sản phẩm; giá mới chỉ áp dụng cho đơn chưa chốt về sau. |
| FR-PRO-04 | M | `[G][ĐX]` “Xóa” sản phẩm là ngừng bán, giữ nguyên chi tiết đơn lịch sử; không cho thêm vào đơn mới. |
| FR-PRO-05 | M | `[G]` Hiển thị **top 5** sản phẩm bán chạy nhất: tên, giá hiện tại và tổng lượng bán từ đơn `PAID`. |
| FR-PRO-06 | S | `[ĐX]` Tìm kiếm sản phẩm theo tên và lọc đang bán/ngừng bán. |

## 4.6. Admin — Tồn kho theo chi nhánh (đề xuất bổ sung)

| Mã | Mức | Yêu cầu có thể kiểm thử |
|---|:---:|---|
| FR-INV-01 | M | `[ĐX]` Có bản ghi tồn kho riêng cho mỗi cặp chi nhánh–sản phẩm. |
| FR-INV-02 | M | `[ĐX]` Admin xem số lượng tồn theo chi nhánh và điều chỉnh nhập hàng bằng số nguyên không âm. |
| FR-INV-03 | M | `[ĐX]` Mỗi điều chỉnh lưu thời điểm, người thực hiện, lượng thay đổi và lý do; không sửa trực tiếp lịch sử. |
| FR-INV-04 | M | `[ĐX]` Khi chốt thanh toán, trừ tồn đúng lượng trong đơn; giao dịch thất bại thì không ghi một phần. |
| FR-INV-05 | S | `[ĐX]` Cảnh báo hết hàng/sắp hết hàng trong màn hình menu/tồn kho. |

## 4.7. Staff — Tổng quan và bán hàng

| Mã | Mức | Yêu cầu có thể kiểm thử |
|---|:---:|---|
| FR-STF-01 | M | `[G]` Trang chủ hiển thị tên chi nhánh của Staff, doanh thu hôm nay và số đơn hàng hôm nay. |
| FR-STF-02 | M | `[G]` Menu hiển thị tên món, giá và số lượng còn lại **của chi nhánh đang đăng nhập**. |
| FR-STF-03 | M | `[G]` Staff chọn món, nhập số lượng và tạo giỏ hàng/đơn nháp; hệ thống tính tổng tiền. |
| FR-STF-04 | M | `[ĐX]` Không cho chọn món ngừng bán, số lượng ≤ 0 hoặc số lượng vượt tồn khả dụng. |
| FR-STF-05 | M | `[G]` Nút **“Đã thanh toán”** xác nhận thanh toán mô phỏng, lưu đơn `PAID`, trừ tồn, cập nhật thống kê. |
| FR-STF-06 | M | `[G]` Nút **“Hủy”** chuyển đơn `PENDING` thành `CANCELLED` (hoặc bỏ giỏ chưa lưu), không phát sinh doanh thu, không trừ tồn. |
| FR-STF-07 | M | `[ĐX]` Không cho chốt/hủy lại đơn đã kết thúc; tránh tính tiền/trừ tồn hai lần nếu người dùng nhấn lặp. |
| FR-STF-08 | M | `[ĐX]` Lưu thời điểm, Staff thực hiện, chi nhánh, từng dòng hàng, giá chốt và tổng tiền. |
| FR-STF-09 | S | `[ĐX]` Có trang lịch sử đơn của chi nhánh, xem chi tiết đơn và lọc theo trạng thái/ngày. |

## 4.8. Admin — Quản lý khoản chi (đề xuất bổ sung)

| Mã | Mức | Yêu cầu có thể kiểm thử |
|---|:---:|---|
| FR-EXP-01 | M | `[ĐX]` Admin nhập khoản chi gồm chi nhánh, ngày phát sinh, loại chi, số tiền, ghi chú. |
| FR-EXP-02 | M | `[ĐX]` Admin xem, sửa và vô hiệu hóa khoản chi khi nhập sai; báo cáo tính theo trạng thái hợp lệ. |
| FR-EXP-03 | M | `[ĐX]` Chỉ số tổng chi phí/lợi nhuận được tính từ khoản chi và đơn thanh toán theo cùng bộ lọc. |
| FR-EXP-04 | M | `[ĐX]` Khoản chi phải có số tiền > 0 và ngày hợp lệ; nhập sai hiện lỗi dễ hiểu. |
| FR-EXP-05 | S | `[ĐX]` Hiển thị bảng khoản chi theo chi nhánh/ngày/loại chi, để đối soát báo cáo. |

## 4.9. Admin — Quản lý tài khoản Staff (đề xuất bổ sung)

| Mã | Mức | Yêu cầu có thể kiểm thử |
|---|:---:|---|
| FR-USR-01 | M | `[ĐX]` Admin tạo tài khoản Staff với username duy nhất, tên hiển thị, mật khẩu ban đầu và chi nhánh được gán. |
| FR-USR-02 | M | `[ĐX]` Admin xem/sửa tên hiển thị, chuyển chi nhánh, vô hiệu hóa hoặc mở lại tài khoản Staff. |
| FR-USR-03 | M | `[ĐX]` Không gán Staff mới cho chi nhánh không hoạt động; giao dịch cũ vẫn giữ `StaffId` lịch sử. |
| FR-USR-04 | M | `[ĐX]` Mật khẩu được lưu ở dạng băm có muối (salt) phù hợp; không hiển thị hoặc lưu password gốc. |

# 5. Luồng sử dụng trọng yếu (Use Cases)

## UC-01 — Đăng nhập

- **Tác nhân:** Admin / Staff; **tiền điều kiện:** ứng dụng đã chạy, CSDL sẵn sàng.
- **Luồng chuẩn:** (1) mở Login → (2) nhập username/password → (3) kiểm tra định dạng → (4) xác thực với CSDL → (5) kiểm tra tài khoản hoạt động, vai trò, chi nhánh → (6) mở đúng shell.
- **Ngoại lệ:** trường rỗng/định dạng sai → lỗi ngay tại form; sai thông tin → thông báo chung; Staff chưa được gán chi nhánh hợp lệ → từ chối; lỗi kết nối → thông báo và cho thử lại, không treo UI.
- **Hậu điều kiện:** phiên đăng nhập gắn `UserId`, `Role`, `BranchId` nếu là Staff; không giữ password thô.
- **Liên quan:** FR-AUTH-01..07, BR-01..03.

## UC-02 — Admin quản lý chi nhánh

- **Tác nhân:** Admin; **tiền điều kiện:** đăng nhập Admin.
- **Luồng chuẩn:** mở trang Chi nhánh → lọc/tìm → thêm hoặc chọn bản ghi → chỉnh sửa thông tin → xác thực dữ liệu → lưu → danh sách refresh bằng Binding.
- **Nhánh xóa:** nhấn “Xóa/Ngừng hoạt động” → xác nhận → kiểm tra đơn chờ → đổi `IsActive = false` → giữ nguyên giao dịch trước đó.
- **Ngoại lệ:** dữ liệu thiếu/sai, trùng khóa tên nếu áp dụng, lỗi CSDL hoặc có đơn PENDING → hủy tác vụ và hiện lỗi.
- **Liên quan:** FR-BRN-01..06.

## UC-03 — Admin cập nhật thực đơn chung

- **Tác nhân:** Admin; **tiền điều kiện:** đăng nhập Admin.
- **Luồng chuẩn:** mở Sản phẩm → tạo/chỉnh sửa món và giá → kiểm tra hợp lệ → lưu; giá mới có hiệu lực cho đơn mới, không sửa `UnitPriceSnapshot` của đơn cũ.
- **Nhánh ngừng bán:** đánh dấu sản phẩm không hoạt động; các dòng đơn lịch sử không bị xóa.
- **Liên quan:** FR-PRO-01..06, BR-04, BR-06, BR-10.

## UC-04 — Staff tạo đơn và xác nhận thanh toán

- **Tác nhân:** Staff; **tiền điều kiện:** tài khoản và chi nhánh còn hoạt động; có món đang bán; kết nối CSDL.
- **Luồng chuẩn:** (1) Staff mở Menu → (2) chọn sản phẩm và số lượng → (3) hệ thống kiểm tra nhập liệu, tính tổng → (4) tạo đơn `PENDING` → (5) Staff nhấn **Đã thanh toán** → (6) kiểm tra quyền, trạng thái và tồn kho mới nhất → (7) trong một transaction, cập nhật đơn `PAID`, lưu thời điểm thanh toán, trừ tồn, ghi điều chỉnh tồn → (8) commit → (9) hiện xác nhận thành công, dashboard được cập nhật khi tải lại hoặc phát tín hiệu làm mới.
- **Ngoại lệ:** hết tồn do người khác vừa bán; CSDL lỗi; nhấn thanh toán lặp; sản phẩm đã ngừng bán → rollback, giữ dữ liệu không sai lệch và thông báo rõ; không ghi doanh thu hai lần.
- **Hậu điều kiện:** đơn `PAID` được tính doanh thu và lượng bán.
- **Liên quan:** FR-STF-02..08, FR-INV-01..04, BR-07..11.

## UC-05 — Staff hủy đơn

- **Tác nhân:** Staff; **tiền điều kiện:** có giỏ hàng hoặc đơn `PENDING` thuộc chi nhánh đăng nhập.
- **Luồng chuẩn:** nhấn **Hủy** → xác nhận nếu đã tạo PENDING → chuyển `CANCELLED` hoặc bỏ giỏ → hiển thị xác nhận, quay về menu.
- **Ngoại lệ:** đơn đã `PAID`/`CANCELLED` → từ chối thay đổi.
- **Hậu điều kiện:** không cộng doanh thu, không trừ tồn.
- **Liên quan:** FR-STF-06..07, BR-07..09.

## UC-06 — Admin xem báo cáo

- **Tác nhân:** Admin; **tiền điều kiện:** đăng nhập Admin.
- **Luồng chuẩn:** mở Dashboard/Doanh thu → chọn thời gian/chi nhánh → hệ thống truy vấn các đơn `PAID` và khoản chi hợp lệ → tính thẻ thống kê + biểu đồ theo một bộ lọc → hiển thị phân bổ doanh thu chi nhánh.
- **Ngoại lệ:** khoảng ngày sai, không có dữ liệu, doanh thu tổng bằng 0, lỗi CSDL; hiển thị trạng thái đúng, không chia cho 0.
- **Liên quan:** FR-ADM-01..04, FR-RPT-01..05, FR-EXP-01..05, BR-12..14.

# 6. Yêu cầu giao diện người dùng WPF

## 6.1. Sơ đồ màn hình

```text
LoginWindow
  ├── AdminShellWindow
  │    ├── Tổng quan (DashboardView)
  │    ├── Doanh thu chi nhánh (RevenueView)
  │    ├── Chi nhánh (BranchesView)
  │    ├── Sản phẩm (ProductsView)
  │    ├── Tồn kho [ĐX] (InventoryView)
  │    ├── Khoản chi [ĐX] (ExpensesView)
  │    ├── Nhân viên [ĐX] (StaffAccountsView)
  │    └── Account / Logout
  └── StaffShellWindow
       ├── Trang chủ (StaffDashboardView)
       ├── Menu & Order (PointOfSaleView)
       ├── Lịch sử đơn [ĐX] (OrderHistoryView)
       └── Account / Logout
```

## 6.2. Quy tắc UI và tương tác

| ID | Yêu cầu | Cách kiểm chứng |
|---|---|---|
| UI-01 | `[CT]` Dùng `Grid` chia vùng chính, `DockPanel` cho navigation/header, `StackPanel` cho nhóm nhập hoặc lệnh hợp lý. | Kiểm tra XAML, resize cửa sổ. |
| UI-02 | `[CT]` Giao diện thống nhất typography, màu, spacing, icon, trạng thái hover/disabled; dùng `ResourceDictionary` và `Style`. | Chụp/đối chiếu ít nhất 5 màn hình. |
| UI-03 | `[CT]` Co giãn cửa sổ không mất nút, che dữ liệu; bảng có scroll khi cần. | Test 1366×768 và 1920×1080 (đề xuất). |
| UI-04 | `[CT]` Dữ liệu hiển thị qua WPF Binding, `DataContext`; list dùng `ObservableCollection<T>`, thuộc tính thay đổi phát `INotifyPropertyChanged`. | Thay dữ liệu trong VM thấy UI cập nhật không sửa trực tiếp control. |
| UI-05 | `[CT]` Tác vụ Login, Logout, Save, Delete/Disable, Search, AddToCart, Pay, Cancel dùng `ICommand`/`AsyncRelayCommand` (nếu sử dụng toolkit). | Xem ViewModel và thao tác trên UI. |
| UI-06 | `[ĐX]` Validate cạnh trường và tóm tắt lỗi; nút lưu/thanh toán vô hiệu hóa khi dữ liệu không hợp lệ/đang xử lý. | Nhập sai rồi sửa lại. |
| UI-07 | `[ĐX]` Có Loading/Empty/Error state cho dữ liệu từ CSDL; lỗi nghiệp vụ thông báo bằng tiếng Việt nhất quán. | Ngắt CSDL và thử bộ lọc không dữ liệu. |
| UI-08 | `[ĐX]` Phân biệt hành động nguy hiểm (ngừng chi nhánh/món, hủy đơn) bằng xác nhận; không hiển thị thông báo thành công trước khi lưu CSDL xong. | Demo các nhánh xác nhận/hủy. |
| UI-09 | `[ĐX]` Bảng danh sách có tiêu đề rõ, định dạng VND/ngày theo locale vi-VN; biểu đồ có tiêu đề, chú giải và nhãn số liệu. | Kiểm tra dữ liệu mẫu. |

**Định hướng bố cục:** Login dạng form căn giữa; Admin dùng sidebar + topbar + content; Staff dùng header tên chi nhánh + dashboard + lưới thẻ món và panel giỏ hàng. WPF `DataTemplate` cho sản phẩm, `ControlTemplate` nếu cần hiệu ứng; ưu tiên bảo đảm MVVM trước phần đồ họa nâng cao.

# 7. Yêu cầu phi chức năng (NFR)

| ID | Nhóm | Tiêu chí nghiệm thu đề xuất |
|---|---|---|
| NFR-01 | Nền tảng | Chạy trên Windows tương thích bản .NET/WPF mà nhóm lựa chọn; build Release sạch, có hướng dẫn cài và chạy. |
| NFR-02 | Hiệu năng | Với bộ dữ liệu demo ~10 chi nhánh, 100 món, 10.000 đơn, chuyển trang danh sách thường ≤ 2 giây trên máy demo; các ngưỡng cần đo thực tế. |
| NFR-03 | Phản hồi UI | Truy vấn CSDL/đọc báo cáo chạy bất đồng bộ khi phù hợp, không làm treo luồng giao diện khi tải dữ liệu. |
| NFR-04 | Chính xác dữ liệu | Không cho tổng tiền/tồn kho âm; không trừ tồn hoặc tính doanh thu hai lần; transaction cho chốt đơn. |
| NFR-05 | Bảo mật | Lưu password dạng hash+salt; truy vấn tham số hóa/ORM; giới hạn quyền theo role và chi nhánh; không hard-code bí mật DB trong Git. |
| NFR-06 | Khả năng dùng | Người dùng thao tác được Login, tìm kiếm, tạo order, xác nhận/hủy mà không cần biết về CSDL; lỗi có hướng dẫn xử lý. |
| NFR-07 | Ổn định | Lỗi nhập liệu và sự cố CSDL có thông báo; ứng dụng không tự thoát do exception chưa xử lý trong các luồng demo. |
| NFR-08 | Bảo trì | View chỉ khai báo hiển thị/binding; ViewModel quản lý state/command; Service quản lý nghiệp vụ; Repository quản lý dữ liệu. |
| NFR-09 | Kiểm thử | Có test case thành công/thất bại cho CRUD, login, phân quyền, thanh toán, báo cáo, đồng thời có biên bản test. |
| NFR-10 | Nhật ký | `[ĐX]` Ghi log lỗi kỹ thuật không lộ mật khẩu, có thời điểm và ngữ cảnh tác vụ; việc thay đổi tồn kho có nhật ký nghiệp vụ. |
| NFR-11 | Khôi phục dữ liệu | `[ĐX]` Có dữ liệu seed và hướng dẫn backup/restore CSDL demo; không coi xóa sản phẩm/chi nhánh là xóa lịch sử. |
| NFR-12 | Tính nhất quán | Dùng cùng công thức và khoảng thời gian cho bảng, biểu đồ, thẻ thống kê; phép tính tiền bằng decimal. |
| NFR-13 | Triển khai | Chỉ định rõ mô hình demo một máy hay mạng LAN; không tuyên bố hỗ trợ đa điểm/offline khi chưa có cơ chế tương ứng. |

# 8. Dữ liệu và thiết kế logic định hướng

> **Lưu ý:** đây là **mô hình dữ liệu đề xuất**, không phải lược đồ đã chốt. Khóa/kiểu/index chi tiết sẽ được thiết kế sau khi trả lời các câu hỏi ở Mục 13. Tên bảng dùng tiếng Anh để dễ liên kết C# models.

| Thực thể | Thuộc tính tối thiểu được đề xuất | Quan hệ/ràng buộc |
|---|---|---|
| `Users` | `Id`, `Username`, `PasswordHash`, `DisplayName`, `Role`, `BranchId?`, `IsActive`, `CreatedAt` | Username unique; Staff phải có BranchId hợp lệ; Admin không bắt buộc. |
| `Branches` | `Id`, `Name`, `Address`, `IsActive`, `Rating`, `CreatedAt` | Điểm 0..5; không xóa cứng khi có đơn hàng. |
| `Products` | `Id`, `Name`, `CurrentPrice`, `IsActive`, `CreatedAt` | `CurrentPrice >= 0`; menu dùng chung. |
| `BranchInventories` | `BranchId`, `ProductId`, `Quantity`, `RowVersion` (hoặc đồng bộ tương đương) | Unique `(BranchId, ProductId)`; `Quantity >= 0`. |
| `InventoryMovements` | `Id`, `BranchId`, `ProductId`, `Delta`, `Reason`, `PerformedBy`, `CreatedAt`, `OrderId?` | Ghi nhận nhập/chỉnh kho và trừ theo đơn. |
| `Orders` | `Id`, `BranchId`, `StaffId`, `Status`, `CreatedAt`, `PaidAt?`, `TotalAmount` | `PENDING/PAID/CANCELLED`; mỗi đơn thuộc một chi nhánh. |
| `OrderItems` | `Id`, `OrderId`, `ProductId`, `ProductNameSnapshot`, `Quantity`, `UnitPriceSnapshot`, `LineTotal` | `Quantity > 0`; tiền không âm; bảo toàn lịch sử giá/tên. |
| `Expenses` | `Id`, `BranchId`, `ExpenseDate`, `Category`, `Amount`, `Note`, `IsVoided`, `CreatedBy` | `Amount > 0`; báo cáo chỉ tính bản ghi không void. |

## 8.1. Quan hệ và tính toàn vẹn

- `Branches 1—N Users`, `Branches 1—N Orders`, `Users 1—N Orders`.
- `Orders 1—N OrderItems`, `Products 1—N OrderItems`.
- `Branches N—N Products` thông qua `BranchInventories`.
- `Branches 1—N Expenses`, `Branches 1—N InventoryMovements`.
- Cấm xóa cứng sản phẩm/chi nhánh/tài khoản liên quan đơn đã phát sinh.
- Các trường thời gian nên lưu nhất quán theo UTC hoặc offset có quy ước, rồi quy đổi Asia/Ho_Chi_Minh cho báo cáo; tuyệt đối không trộn múi giờ khi lọc ngày.
- Nên có chỉ mục trên `Users.Username`, `Orders(BranchId, Status, PaidAt)`, `OrderItems(OrderId, ProductId)` và `Expenses(BranchId, ExpenseDate)`.

## 8.2. Nguồn số liệu báo cáo

| Chỉ số | Nguồn | Công thức / điều kiện |
|---|---|---|
| Doanh thu | `Orders` | Sum(`TotalAmount`) với `Status=PAID` và `PaidAt` thuộc khoảng lọc. |
| Số đơn | `Orders` | Count(`PAID`) trong khoảng lọc; trên Staff Dashboard ghi rõ “đơn đã thanh toán”. |
| Chi phí | `Expenses` | Sum(`Amount`) với `IsVoided=false`, ngày phát sinh thuộc khoảng lọc. |
| Lợi nhuận (theo dữ liệu nhập) | Doanh thu + khoản chi | `Revenue - Expense`; nếu không đủ chi phí thực tế thì nhãn phải ghi **“Lợi nhuận ước tính theo chi phí đã nhập”**. |
| Top 5 bán chạy | `OrderItems` JOIN `Orders` | Sum(`Quantity`) cho đơn `PAID`, sắp giảm dần; khi bằng nhau sắp `ProductId` tăng dần. |
| Tỷ trọng doanh thu | `Orders` | Doanh thu chi nhánh / doanh thu toàn chuỗi × 100%; nếu mẫu số 0 hiển thị “chưa có doanh thu”. |
| Số chi nhánh hoạt động | `Branches` | Count(`IsActive=true`), không phụ thuộc bộ lọc doanh thu. |

# 9. Kiến trúc và tổ chức mã nguồn WPF

## 9.1. Lớp kiến trúc đề xuất

```text
WPF Views (XAML)
   │ DataBinding + ICommand + DataContext
   ▼
ViewModels (state, validation, UI commands)
   │
   ▼
Application Services (Auth, Branch, Product, Sale, Report, Inventory)
   │
   ▼
Repositories / DbContext (EF Core hoặc thư viện truy cập dữ liệu đã chọn)
   │
   ▼
Relational Database (loại CSDL cần chốt)
```

- `[CT]` Không viết nghiệp vụ mua hàng, kiểm soát tồn, tính doanh thu bên trong `Window.xaml.cs`.
- `[CT]` `INotifyPropertyChanged`/toolkit tương đương cho thuộc tính trạng thái, `ObservableCollection<T>` cho danh sách, `ICommand`/async command cho nút; hiểu `DataContext`, `CanExecute` và cập nhật UI.
- `[CT]` Dùng `Resources/Styles.xaml`, `Resources/Colors.xaml` hoặc ResourceDictionary tương đương; tái sử dụng các kiểu điều khiển.
- `[ĐX]` Service báo lỗi bằng kết quả/tình huống nghiệp vụ; ViewModel đổi thành thông báo người dùng. Repository không tự điều hướng UI.
- `[ĐX]` Với nhiều client dùng CSDL qua Internet thật: thêm backend/API có authentication, authorization và transaction ở server. Ứng dụng WPF chỉ là client; đây là lựa chọn kiến trúc cần thống nhất, không mặc định thuộc MVP.

## 9.2. Cấu trúc thư mục tham khảo

```text
CoffeeChain.sln
src/
  CoffeeChain.Wpf/
    App.xaml
    Views/{Auth,Admin,Staff,Shared}/
    ViewModels/{Auth,Admin,Staff,Shared}/
    Resources/{Styles,Colors,Templates}/
    Converters/
  CoffeeChain.Application/
    Interfaces/
    Services/
    DTOs/
    Validators/
  CoffeeChain.Domain/
    Entities/
    Enums/
    Rules/
  CoffeeChain.Infrastructure/
    Persistence/{DbContext,Configurations,Migrations}/
    Repositories/
    Security/
tests/
  CoffeeChain.Tests/
docs/
  SRS.md
  ERD.md
  TEST_CASES.md
README.md
.gitignore
```

Với nhóm nhỏ có thể giảm số lượng project nhưng phải giữ **phân tách trách nhiệm**. Git phải có commit và lịch sử làm việc rõ ràng.

# 10. Kiểm thử và tiêu chí nghiệm thu

## 10.1. Bộ test tối thiểu

| TC | Tình huống | Kết quả mong đợi | Yêu cầu truy vết |
|---|---|---|---|
| TC-01 | Login đúng với Admin | Mở Admin Shell, xem dữ liệu toàn chuỗi. | FR-AUTH-03/04 |
| TC-02 | Login đúng với Staff chi nhánh A | Mở Staff Shell, chỉ tải dữ liệu A. | FR-AUTH-04/07, BR-03 |
| TC-03 | Login rỗng/sai định dạng/sai mật khẩu | Chặn phù hợp; không crash; không tiết lộ password. | FR-AUTH-02/03 |
| TC-04 | Staff cố truy cập/xử lý dữ liệu chi nhánh B | Bị từ chối ở service, không chỉ ẩn giao diện. | FR-AUTH-07, BR-03 |
| TC-05 | Thêm/sửa/ngừng chi nhánh và tìm theo tên/sao/trạng thái | Danh sách phản ánh đúng, lịch sử giữ nguyên. | FR-BRN-01..06 |
| TC-06 | Thêm/sửa/ngừng sản phẩm; giá âm | CRUD hợp lệ; giá âm bị từ chối; lịch sử đơn giữ giá cũ. | FR-PRO-01..04, BR-10 |
| TC-07 | Tạo đơn nhiều sản phẩm hợp lệ và bấm “Đã thanh toán” | Lưu 1 đơn PAID, tổng đúng, trừ tồn đúng, chỉ số doanh thu tăng đúng. | FR-STF-03/05/08, FR-INV-04 |
| TC-08 | Tạo đơn rồi bấm “Hủy” | Đơn hủy không tăng doanh thu và không trừ tồn. | FR-STF-06, BR-08 |
| TC-09 | Đặt số lượng vượt tồn hoặc hai luồng cùng mua lượng tồn cuối | Từ chối chốt giao dịch không hợp lệ; không âm kho, không có đơn PAID dở dang. | FR-STF-04, FR-INV-04, NFR-04 |
| TC-10 | Nhấn thanh toán liên tiếp hai lần | Không có 2 đơn/2 lần trừ tồn cho cùng một xác nhận. | FR-STF-07 |
| TC-11 | Dashboard không có đơn, có doanh thu = 0 | Thẻ 0 và biểu đồ rỗng; không chia cho 0. | FR-ADM-04, FR-RPT-04 |
| TC-12 | Tạo/sửa/vô hiệu khoản chi; kiểm tra lọc ngày | Chi phí và lợi nhuận biến động chính xác. | FR-EXP-01..05, BR-13 |
| TC-13 | Chỉnh tồn kho tại chi nhánh A | Chỉ tồn A thay đổi, có lịch sử nhập/chỉnh. | FR-INV-01..03 |
| TC-14 | Đổi giá sản phẩm sau khi thanh toán đơn cũ | Đơn cũ và doanh thu lịch sử không thay đổi. | FR-PRO-03, BR-10 |
| TC-15 | Mất kết nối CSDL trong khi tải dữ liệu | Thông báo và khả năng thử lại; UI không treo/crash. | NFR-03/07 |
| TC-16 | Resize cửa sổ + điều khiển MVVM | Không che nút; thay đổi VM cập nhật UI; thao tác bằng Command. | UI-01..05 |
| TC-17 | Staff đăng xuất rồi truy cập lại màn hình trước | Không giữ quyền phiên cũ. | FR-AUTH-05/07 |

## 10.2. Definition of Done (hoàn thành một chức năng)

- Có màn hình và ViewModel/Service/Repository nếu chức năng cần CSDL.
- Có validate dữ liệu, trường hợp rỗng và thông báo lỗi.
- Có phân quyền tương ứng; thử thao tác trái quyền.
- Có test thành công và ít nhất một test thất bại/ngoại lệ.
- Có commit Git, cập nhật ghi chú README/changelog nếu thay đổi hành vi.
- Không viết lại logic giống nhau ở các màn hình khác nhau.

## 10.3. Tiêu chí chấp nhận bản MVP

Đồ án được xem là đáp ứng bản SRS sau khi các `FR` mức `M` đã **được nhóm xác nhận** triển khai và qua test liên quan; demo thực tế đầy đủ chuỗi **Login Admin → CRUD chi nhánh/sản phẩm → Login Staff → lập đơn/hủy/thanh toán → Admin xem doanh thu thay đổi**, đồng thời chứng minh Binding, Command, MVVM và xử lý trường hợp lỗi. Mục `[ĐX]` chỉ trở thành bắt buộc sau khi được nhóm phê duyệt; khi không phê duyệt phải thay thế bằng giải pháp được chốt và cập nhật tài liệu.

# 11. Ma trận liên kết với tiêu chí chấm đồ án

| Tiêu chí trong `DanhGiaDoAnIT008.pdf` | Điểm | Bằng chứng cần chuẩn bị |
|---|---:|---|
| **1. Chức năng và mức độ hoàn thiện** | 2.0 | Chạy thật các FR mức M đã chốt, CRUD đầy đủ cho dữ liệu quản lý, validation, xử lý lỗi, test case TC-01..17. |
| **2. Giao diện** | 2.0 | Layout `Grid`, `StackPanel`, `DockPanel`; resize; style/resource; giao diện Login/Admin/Staff nhất quán. |
| **3. Data Binding, Command, MVVM** | 3.0 | `DataContext`, Binding hai chiều khi sửa form, `ObservableCollection`, `INotifyPropertyChanged`, `ICommand`; View–VM–Service tách bạch. |
| **3'. Template, đồ họa, đa luồng** | 3.0 **thay thế mục 3** | **Tùy chọn** chỉ khi nhóm chọn hướng không bao hàm mục 3; không cộng đồng thời 3 và 3'. Có thể demo biểu đồ/templates/async như kỹ thuật nâng cao. |
| **4. Cấu trúc và tổ chức mã nguồn** | 1.0 | Kiến trúc phân tầng, OOP, tên rõ, ít trùng code, Git, README, cấu trúc repo ở Mục 9. |
| **5. Teamwork** | 0.5 | Có plan, phân công người phụ trách, commit và bảng tiến độ có minh chứng. |
| **6. Demo, trình bày, Q&A** | 1.5 | Kịch bản demo tại Mục 12, giải thích rõ MVVM, CSDL, transaction, biểu đồ và giới hạn MVP. |

**Tổng 10 điểm:** sử dụng **mục 3 hoặc 3'**, không tính hai mục cùng lúc. Đề xuất chủ động chọn **mục 3 (MVVM/Binding/Command)** vì phù hợp trực tiếp với ứng dụng quản lý WPF và bảo đảm nền tảng kỹ thuật dễ giải thích.

# 12. Kế hoạch làm đồ án và kịch bản demo

## 12.1. Giai đoạn thực hiện (tham khảo)

| Giai đoạn | Công việc chính | Kết quả nghiệm thu |
|---|---|---|
| 1 — Chốt yêu cầu & dựng nền | Trả lời Mục 13; use case; ERD; UI wireframe; tạo solution/Git; tạo CSDL/seed. | SRS v1.0, ERD v1.0, app mở được, test DB. |
| 2 — Tài khoản & Admin | Login/role; shell, resource/styles; CRUD branch/product/staff; dashboard khung. | Login phân quyền, CRUD hoạt động, MVVM thể hiện rõ. |
| 3 — Bán hàng & dữ liệu báo cáo | Order/pay/cancel, tồn kho/transaction, khoản chi, truy vấn doanh thu và biểu đồ. | Chạy UC-04/05/06; số liệu sau bán chính xác. |
| 4 — Hoàn thiện & demo | Validation, loading/error, test, tối ưu resize, chuẩn bị data seed, slide và trình bày. | Test checklist đạt; kịch bản demo ổn định. |

**Phân công mẫu `[CHỐT]`:** 1 người chịu trách nhiệm tích hợp/Git + xác thực; 1 người UI/Styles + các màn Admin; 1 người bán hàng/CSDL + kiểm thử báo cáo. Nếu số thành viên khác, phân công lại để mỗi tác vụ có một người chịu trách nhiệm chính và người review. Không coi đây là phân công đã chốt.

## 12.2. Demo đề xuất (6–8 phút, tùy quy định lớp)

1. Admin đăng nhập; chỉ ra `LoginViewModel.LoginCommand`, xác thực, điều hướng theo role.
2. Tạo/sửa/ngừng bán một sản phẩm; minh họa kiểm tra giá âm.
3. Tạo chi nhánh mới hoặc chỉnh trạng thái; tìm kiếm theo tên/sao/trạng thái.
4. Chuẩn bị tồn kho, đăng nhập Staff đúng chi nhánh; tạo đơn gồm 2 món và thanh toán; sau đó tạo đơn khác và hủy.
5. Quay về Admin; biểu đồ/doanh thu/top 5 thay đổi từ đơn đã thanh toán; kiểm tra đơn hủy không được tính.
6. Mở code giải thích `DataContext`, Binding, `ObservableCollection`, `INotifyPropertyChanged`, `ICommand`, Service, Repository và DB transaction.
7. Thử ngắt kết nối hoặc nhập sai; nói rõ giới hạn thanh toán mô phỏng và những tính năng ngoài phạm vi.

# 13. Câu hỏi cần xác nhận trước khi chốt SRS v1.0

| Mã | Câu hỏi | Tác động nếu chưa chốt | Đề xuất mặc định để bắt đầu |
|---|---|---|---|
| Q-01 | Demo **một máy** hay **nhiều máy truy cập dữ liệu chung**? CSDL muốn dùng SQL Server, PostgreSQL hay SQLite? | Kiến trúc triển khai, DbContext, concurrency. | Một máy demo với CSDL quan hệ; chọn hệ quản trị theo kinh nghiệm nhóm. |
| Q-02 | Nhóm có mấy thành viên và thời hạn nộp? | Phân chia scope/sprint. | Giữ MVP, bỏ bớt các mục `S` nếu ít thời gian. |
| Q-03 | Chi phí nhập thủ công theo chi nhánh có phù hợp không? Có cần tính **giá vốn từng sản phẩm** không? | Công thức lợi nhuận & data model. | Nhập khoản chi; gọi là “lợi nhuận ước tính theo chi phí đã nhập”. |
| Q-04 | Điểm đánh giá sao từ đâu: Admin tự nhập hay khách hàng đánh giá? | Màn hình/đối tượng Ratings. | Admin tự nhập 0–5; không tích hợp review khách. |
| Q-05 | Số lượng còn lại là **tồn của từng món tại từng quán**? Ai được nhập thêm/chỉnh số lượng? | CSDL tồn kho, phân quyền, cách chốt đơn. | Admin quản lý tồn, Staff chỉ xem và bán. |
| Q-06 | Staff được gán một chi nhánh cố định hay đổi chi nhánh khi làm ca? | Phân quyền tài khoản. | Một chi nhánh/tài khoản; Admin có thể đổi. |
| Q-07 | Hệ thống có cần lưu **đơn hàng chờ (PENDING)** và xem lịch sử đơn không? | Trạng thái đơn và màn staff. | Lưu PENDING, lịch sử đơn mức `S`. |
| Q-08 | Admin có cần sửa giá/tên sản phẩm (ngoài thêm/xóa) và sửa thông tin chi nhánh để minh chứng CRUD đầy đủ? | Đối chiếu tiêu chí 1 về CRUD. | Có, sửa đầy đủ; xóa mềm để giữ lịch sử. |
| Q-09 | Có cần nhiều tài khoản Admin và tạo Staff ngay trên app không, hay tạo sẵn trong DB để demo? | Phạm vi màn nhân sự, setup data. | Một Admin seed sẵn; Admin quản lý các tài khoản Staff. |
| Q-10 | Phương thức thanh toán mô phỏng chỉ đúng hai nút **“Đã thanh toán” / “Hủy”** như report chứ? | UI/flow bán hàng. | Đúng; không thêm thẻ/QR/tiền mặt. |

## 13.1. Điều kiện để khóa phạm vi

Khi nhóm trả lời các câu hỏi Q-01 đến Q-10, hãy cập nhật SRS thành **v1.0 — Approved**, bỏ nhãn `[CHỐT]`, ghi rõ mục `[ĐX]` nào được chấp thuận/từ chối, sửa lại BR/FR/Data Model/Test Case liên quan rồi mới tạo migration CSDL chính thức và chia task code.

# 14. Phụ lục: nguồn, thay đổi và giới hạn

## 14.1. Nguồn trực tiếp

1. **`Report.md`**: ý tưởng gốc *Quản lý chuỗi quán cà phê*, Login/Logout, Admin dashboard/doanh thu/chi nhánh/sản phẩm, Staff dashboard/menu/order và hai nút xử lý thanh toán.
2. **`DanhGiaDoAnIT008.pdf`**: phiếu *Các tiêu chí đánh giá đồ án môn Lập trình trực quan*, 1 trang, với trọng số chức năng 2; giao diện 2; MVVM/Binding/Command 3 **hoặc** nâng cao 3'; cấu trúc mã 1; teamwork 0.5; demo/Q&A 1.5.

## 14.2. Lịch sử phiên bản

| Phiên bản | Ngày | Nội dung |
|---|---|---|
| 0.1 | 26/10/2026 | Chuyển report gốc sang SRS, thêm truy vết, yêu cầu kiểm thử, kiến trúc WPF/MVVM và các đề xuất nhằm khép kín nghiệp vụ. |

## 14.3. Giới hạn hiện tại của tài liệu

Đây là bản đặc tả **nháp**; chưa có xác nhận quy mô nhóm, loại CSDL, mô hình triển khai, nguồn dữ liệu chi phí, quy tắc tồn kho và nguồn đánh giá sao. Không coi các phần đề xuất là quyết định đã phê duyệt. Sau khi xác nhận SRS v1.0, có thể tách thêm tài liệu thiết kế chi tiết: ERD, cấu trúc bảng, wireframe, sequence diagram, quy cách exception và test plan.
