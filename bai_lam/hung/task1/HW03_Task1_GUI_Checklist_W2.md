# HW03 – Task 1: Checklist GUI (bộ ban đầu do AI sinh) — Luồng W2

**Luồng W2:** Khách xem sản phẩm → thêm vào giỏ → bấm *Tiến hành thanh toán* → bị yêu cầu đăng nhập → quay lại giỏ → thanh toán kèm mã giảm giá.

**SUT:** EShop web frontend `http://localhost:5173` · backend `http://localhost:3000`

## Các màn hình trong phạm vi

| ID | Màn hình | Đường dẫn |
|---|---|---|
| S0 | Header / thanh điều hướng chung | `mọi trang` |
| S1 | Trang chủ – danh sách & tìm kiếm sản phẩm | `/` |
| S2 | Chi tiết sản phẩm | `/product/:id` |
| S3 | Giỏ hàng (có hàng và trống) | `/cart` |
| S4 | Đăng nhập (bị chuyển tới từ bước thanh toán) | `/login` |
| S5 | Thanh toán + mã giảm giá | `/checkout` |
| S6 | Thanh toán thành công | `/checkout (sau khi xác nhận)` |

## Độ bao phủ

| Nhóm | Tên nhóm | Số mục |
|---|---|---|
| IA-01 | Tiêu chuẩn giao diện chung | 20 |
| IA-02 | Biểu mẫu (Forms) | 18 |
| IA-03 | Điều hướng | 17 |
| IA-04 | Phản hồi / trạng thái | 17 |
| | **Tổng** | **72** |

Trạng thái: **mới thiết kế** — cột Kết quả / Ghi chú để trống cho tới giai đoạn thực thi. Mọi dòng hiện có Nguồn gốc = AI; các mục bổ sung khi rà soát sẽ ghi 'Thủ công' kèm lý do AI bỏ sót.

## IA-01 — Tiêu chuẩn giao diện chung

| ID | Màn hình | Mục kiểm tra | Kết quả mong đợi | Nguồn | Kết quả | Ghi chú |
|---|---|---|---|---|---|---|
| GC01-01 | S0–S6 | Toàn bộ nhãn, nút, tiêu đề và thông báo dùng tiếng Việt (trừ thuật ngữ kỹ thuật chuẩn). | Không còn chữ tiếng Anh như 'Sign In', 'Username', 'Loading'. | FR-21 | | |
| GC01-02 | S0–S6 | Các nút hành động tích cực (Tìm, Thêm vào giỏ, Thanh toán, Đăng nhập, Áp dụng) dùng màu xanh dương. | Mọi nút hành động chính đều xanh dương; không có nút xanh lá hay cam. | FR-21 | | |
| GC01-03 | S0, S3 | Các nút nguy hiểm / hủy bỏ (Xóa, Đăng xuất) dùng màu đỏ. | Mọi hành động phá hủy đều màu đỏ. | FR-21 | | |
| GC01-04 | S1 | Giá sản phẩm trên lưới Trang chủ dùng ký hiệu ₫ và phân cách hàng nghìn. | VD: '30.000.000 ₫' – không dùng 'VND', không để số thô. | FR-05, FR-21 | | |
| GC01-05 | S2 | Giá ở trang Chi tiết sản phẩm dùng ₫ và phân cách hàng nghìn. | Cùng định dạng với Trang chủ. | FR-21 | | |
| GC01-06 | S3 | Đơn giá, thành tiền và tổng giỏ hàng đều dùng ₫ và phân cách hàng nghìn. | Ba cột định dạng giống nhau. | FR-21 | | |
| GC01-07 | S5 | Dòng sản phẩm, số tiền giảm và tổng cuối ở trang Thanh toán dùng ₫ và phân cách hàng nghìn. | Không có số nào chưa định dạng trên trang (kể cả trong ô nhập). | FR-21 | | |
| GC01-08 | S1 | Trang chủ có đúng một thẻ <h1>. | DevTools: document.querySelectorAll('h1').length === 1. | FR-05, FR-21 | | |
| GC01-09 | S2–S6 | Mỗi trang còn lại có đúng một <h1> mô tả nội dung trang. | Giỏ hàng, Đăng nhập, Thanh toán, Thành công đều có một <h1> với nội dung riêng của trang. | FR-21 | | |
| GC01-10 | S4 | Tiêu đề trang khớp với chức năng của trang. | Trang đăng nhập có tiêu đề 'Đăng nhập', không phải 'Đăng ký'. | FR-21 (heuristic: khớp giữa hệ thống và thế giới thực) | | |
| GC01-11 | S1 | Ảnh sản phẩm trên Trang chủ cùng tỷ lệ, không bị méo. | Các thẻ có ảnh cùng chiều cao, không bị kéo giãn. | FR-05 | | |
| GC01-12 | S1, S2 | Mọi ảnh sản phẩm có thuộc tính alt không rỗng, mô tả sản phẩm. | alt bằng hoặc chứa tên sản phẩm; alt="" là không đạt. | FR-05, FR-24 | | |
| GC01-13 | S2 | Trang chi tiết hiển thị ảnh lớn, tên, giá, mô tả và danh mục. | Đủ cả 5 thông tin. | FR-06 | | |
| GC01-14 | S1 | Tên sản phẩm dài không làm vỡ bố cục thẻ sản phẩm. | Tên bị cắt bằng dấu '…' và xem được tên đầy đủ (tooltip / trang chi tiết); các thẻ vẫn thẳng hàng. | Heuristic: nhất quán | | |
| GC01-15 | S1–S5 | Ở độ rộng di động (≤ 640 px) không có thanh cuộn ngang và mọi điều khiển hiển thị đầy đủ. | Chrome DevTools 375 px: nút 'Thêm vào giỏ hàng', bảng, các nút nằm trọn trong màn hình. | Heuristic: thiết kế responsive | | |
| GC01-16 | S1, S5 | Thứ tự focus khi nhấn Tab đi từ trên xuống dưới, trái sang phải. | Tab qua Trang chủ và Thanh toán: focus không nhảy ngược. | FR-21 | | |
| GC01-17 | S0–S6 | Mọi phần tử nhận focus đều có chỉ báo focus nhìn thấy được. | Có viền / outline focus trên link, nút, ô nhập. | WCAG 2.2 SC 2.4.7 | | |
| GC01-18 | S0–S6 | Độ tương phản màu chữ tối thiểu 4.5:1 với chữ thường. | Kiểm tra chữ xám/vàng (VD 'Chào, …' màu vàng trên nền xanh) bằng công cụ đo tương phản. | WCAG 2.2 SC 1.4.3 | | |
| GC01-19 | S0–S6 | Tiêu đề tab trình duyệt (<title>) thay đổi theo trang hiện tại. | Mỗi trang có title riêng (VD 'Giỏ hàng – EShop'). | Heuristic: hiển thị trạng thái hệ thống | | |
| GC01-20 | S0–S6 | Kiểu chữ (font, cỡ tiêu đề) nhất quán giữa các trang. | Tiêu đề cùng cấp có cùng cỡ / độ đậm. | Heuristic: nhất quán | | |

## IA-02 — Biểu mẫu (Forms)

| ID | Màn hình | Mục kiểm tra | Kết quả mong đợi | Nguồn | Kết quả | Ghi chú |
|---|---|---|---|---|---|---|
| GC02-01 | S1 | Ô tìm kiếm có nhãn truy cập được và tìm được bằng phím Enter hoặc nút 'Tìm'. | Cả Enter và bấm nút đều thực hiện tìm kiếm. | FR-05 | | |
| GC02-02 | S1 | Từ khóa tìm kiếm được hiển thị lại dạng văn bản thuần, không render HTML. | Tìm <b>abc</b> hoặc <img src=x onerror=alert(1)> hiển thị đúng chuỗi ký tự; không in đậm, không bật alert. | FR-05, SEC-04 | | |
| GC02-03 | S2 | Ô Số lượng chỉ nhận số nguyên dương, tối thiểu 1. | 0, -1, 1.5, 'abc', để trống đều bị từ chối hoặc tự sửa về 1. | FR-06 | | |
| GC02-04 | S2 | Ô Số lượng có nhãn được liên kết đúng cách. | Bấm vào nhãn 'Số lượng' thì ô nhập được focus (<label for>). | WCAG 2.2 SC 1.3.1 | | |
| GC02-05 | S4 | Trường Email dùng type="email" và có kiểm tra định dạng HTML5. | Nhập 'abc' thì trình duyệt chặn gửi form và hiện thông báo. | FR-02, FR-22 | | |
| GC02-06 | S4 | Trường Mật khẩu dùng type="password" (ẩn ký tự). | Ký tự nhập vào bị che. | FR-22 | | |
| GC02-07 | S4 | Nhãn trường mô tả đúng dữ liệu cần nhập. | Trường email có nhãn 'Email', không phải 'Username'. | Heuristic: khớp giữa hệ thống và thế giới thực | | |
| GC02-08 | S4, S5 | Mọi trường bắt buộc có dấu '*' cạnh nhãn. | Email*, Mật khẩu* ở trang Đăng nhập. | FR-22 | | |
| GC02-09 | S4 | Thông báo lỗi đăng nhập xuất hiện PHÍA TRÊN nút submit. | Hộp lỗi nằm giữa tiêu đề/các trường và nút, không nằm dưới nút. | FR-22 | | |
| GC02-10 | S4 | Lỗi sai thông tin đăng nhập là thông báo chung, không lộ trường nào sai. | Cùng một thông báo cho email không tồn tại và sai mật khẩu. | FR-02 | | |
| GC02-11 | S4 | Form đăng nhập gửi được bằng phím Enter. | Nhấn Enter ở ô mật khẩu thì đăng nhập. | Heuristic: linh hoạt & hiệu quả | | |
| GC02-12 | S5 | Tổng tiền thanh toán được tính tự động và người dùng KHÔNG sửa được. | Tổng tiền là văn bản hoặc ô chỉ đọc. | FR-08 | | |
| GC02-13 | S5 | Ô mã giảm giá có nhãn và placeholder; nút 'Áp dụng' bị vô hiệu khi ô trống. | Ô trống thì nút bị disable; gõ chữ thì nút bật lại. | FR-09 | | |
| GC02-14 | S5 | Mã giảm giá không phân biệt hoa thường. | 'save10' được chấp nhận như 'SAVE10'. | FR-09 (heuristic: phòng ngừa lỗi) | | |
| GC02-15 | S5 | Thông báo lỗi mã giảm giá cụ thể, bằng tiếng Việt cho từng điều kiện không thỏa. | Thông báo khác nhau cho: mã không tồn tại (C1), hết hạn (EXPIRED, C2), chưa đủ ngưỡng (C3), đã dùng hết lượt (C5). | FR-09 | | |
| GC02-16 | S5 | Thông báo lỗi mã giảm giá / thanh toán nằm phía trên nút xác nhận. | Lỗi không nằm dưới nút 'Xác nhận thanh toán'. | FR-22 | | |
| GC02-17 | S5 | Nút xác nhận thanh toán chống bấm hai lần. | Sau lần bấm đầu, nút bị disable và hiện 'Đang xử lý…'. | Heuristic: phòng ngừa lỗi | | |
| GC02-18 | S5 | Trang thanh toán hiển thị (hoặc cho nhập/xác nhận) địa chỉ giao hàng và người nhận. | Địa chỉ mặc định trong hồ sơ được hiển thị trước khi xác nhận. | FR-04 (heuristic: thương mại điện tử thực tế) | | |

## IA-03 — Điều hướng

| ID | Màn hình | Mục kiểm tra | Kết quả mong đợi | Nguồn | Kết quả | Ghi chú |
|---|---|---|---|---|---|---|
| GC03-01 | S0 | Thanh điều hướng làm nổi bật link của trang hiện tại. | Ở /cart, link 'Giỏ hàng' có kiểu active. | FR-23 | | |
| GC03-02 | S0 | Link 'Giỏ hàng' có badge hiển thị số sản phẩm trong giỏ. | Badge hiển thị đúng số lượng (giỏ trống thì ẩn hoặc hiện 0). | FR-23 | | |
| GC03-03 | S0 | Nút đăng xuất có nhãn 'Đăng xuất'. | Nhãn không phải 'Thoát'. | FR-23 | | |
| GC03-04 | S0 | Logo 'EShop' đưa về Trang chủ từ mọi trang. | Bấm logo → '/'. | Heuristic: người dùng kiểm soát & tự do | | |
| GC03-05 | S2 | Trang chi tiết sản phẩm có breadcrumb. | VD: Trang chủ › <Danh mục> › <Tên SP>, mỗi phần bấm được. | FR-23 | | |
| GC03-06 | S3 | Trang giỏ hàng có breadcrumb. | Trang chủ › Giỏ hàng. | FR-23 | | |
| GC03-07 | S5 | Trang thanh toán có breadcrumb. | Trang chủ › Giỏ hàng › Thanh toán. | FR-23 | | |
| GC03-08 | S3 | Giỏ hàng có nút 'Tiếp tục mua sắm' quay về Trang chủ. | Nhãn đúng 'Tiếp tục mua sắm'; điều hướng về '/'. | FR-07 | | |
| GC03-09 | S1 | Bấm vào ảnh và tên trên thẻ sản phẩm cũng mở trang chi tiết. | Không chỉ nút 'Xem chi tiết' mới bấm được. | Heuristic: linh hoạt & hiệu quả | | |
| GC03-10 | S3→S4 | Khách chưa đăng nhập bấm 'Tiến hành thanh toán' thì được chuyển tới trang Đăng nhập. | Chuyển tới /login kèm lời giải thích. | FR-08 | | |
| GC03-11 | S4→S3/S5 | Đăng nhập xong từ bước chuyển hướng thanh toán, người dùng quay lại Giỏ hàng/Thanh toán, không về Trang chủ. | Trang đích sau đăng nhập là /cart hoặc /checkout. | Heuristic: người dùng kiểm soát & tự do (trọng tâm luồng W2) | | |
| GC03-12 | S4→S3 | Giỏ hàng được giữ nguyên sau khi khách đăng nhập. | Sau đăng nhập, giỏ có đúng các sản phẩm và số lượng như trước. | FR-07, FR-08 | | |
| GC03-13 | S3 | Giỏ hàng vẫn còn sau khi tải lại trang (F5). | Sản phẩm vẫn còn sau khi reload. | Heuristic: người dùng kiểm soát & tự do | | |
| GC03-14 | S5 | Mở trực tiếp /checkout khi chưa đăng nhập bị chặn. | Chuyển tới /login (khách không thấy form thanh toán). | FR-08 | | |
| GC03-15 | S5→S3 | Nút Back của trình duyệt từ Thanh toán quay về Giỏ hàng, giỏ không đổi. | Giỏ giữ nguyên sau khi Back. | Heuristic: người dùng kiểm soát & tự do | | |
| GC03-16 | S4 | Trang đăng nhập có link tới 'Đăng ký' và 'Quên mật khẩu'. | Cả hai link hiển thị và hoạt động. | FR-01, FR-03 | | |
| GC03-17 | S6 | Trang thành công gợi ý bước tiếp theo (Trang chủ và lịch sử đơn hàng). | Có link tới '/' và '/profile' (Lịch sử đơn hàng). | FR-11 (heuristic) | | |

## IA-04 — Phản hồi / trạng thái

| ID | Màn hình | Mục kiểm tra | Kết quả mong đợi | Nguồn | Kết quả | Ghi chú |
|---|---|---|---|---|---|---|
| GC04-01 | S1 | Có chỉ báo đang tải khi lấy danh sách sản phẩm. | Spinner/skeleton hiện ra khi mạng chậm (DevTools 'Slow 3G'). | FR-05 | | |
| GC04-02 | S2 | Trang chi tiết có trạng thái đang tải bằng tiếng Việt. | Hiện 'Đang tải…' hoặc spinner khi mạng chậm. | FR-05 (heuristic) | | |
| GC04-03 | S1 | Tìm kiếm không có kết quả hiển thị empty state có icon/hình minh họa và thông báo thân thiện. | Tìm 'zzzz' hiện VD 'Không tìm thấy sản phẩm nào'. | FR-05, FR-24 | | |
| GC04-04 | S2 | Nút 'Thêm vào giỏ hàng' có phản hồi trực quan ngay ở lần bấm ĐẦU TIÊN. | Toast xuất hiện / badge tăng sau một lần bấm. | FR-06, FR-24 | | |
| GC04-05 | S1 | Nút 'Thêm vào giỏ' trên thẻ sản phẩm ở Trang chủ có phản hồi trực quan. | Có toast hoặc badge cập nhật sau khi bấm. | FR-24 | | |
| GC04-06 | S3 | Thêm cùng một sản phẩm hai lần thì tăng số lượng, không tạo dòng mới. | Một dòng với số lượng 2. | FR-07 | | |
| GC04-07 | S3 | Giỏ hàng có nút +/- và thành tiền / tổng cộng cập nhật ngay. | Bấm + → số lượng, thành tiền, tổng cập nhật; '-' không giảm dưới 1. | FR-07 | | |
| GC04-08 | S3 | Xóa sản phẩm khỏi giỏ có hộp thoại xác nhận. | 'Bạn có chắc muốn xóa?' – chọn Hủy thì sản phẩm vẫn còn. | FR-07, FR-24 | | |
| GC04-09 | S3 | Nhãn tổng tiền trong giỏ là 'Tổng cộng'. | Không phải 'Tổng tạm tính'. | FR-07 | | |
| GC04-10 | S3 | Giỏ hàng trống có hình minh họa và thông báo rõ ràng. | Có icon/hình + thông báo + link quay lại mua sắm. | FR-07, FR-24 | | |
| GC04-11 | S3→S4 | Thông báo 'cần đăng nhập' hiển thị ngay trên trang, bằng tiếng Việt rõ ràng. | Giải thích lý do cần đăng nhập; không chỉ là hộp alert() chặn của trình duyệt. | Heuristic: hiển thị trạng thái hệ thống | | |
| GC04-12 | S5 | Áp dụng mã hợp lệ thì hiển thị số tiền được giảm và tổng mới. | SAVE10 với giỏ ≥ 300.000 ₫ hiện 'Tiết kiệm' và tổng đã giảm. | FR-09 | | |
| GC04-13 | S5 | Không cho thanh toán khi giỏ hàng trống. | Nút xác nhận bị disable hoặc hiện 'Giỏ hàng trống'. | Heuristic: phòng ngừa lỗi | | |
| GC04-14 | S6 | Màn hình thành công xác nhận đơn hàng (có mã đơn) và giỏ hàng được xóa. | Hiện mã đơn; badge/số lượng giỏ về 0. | FR-08 | | |
| GC04-15 | S1, S2, S5 | Lỗi mạng/máy chủ hiển thị thông báo tiếng Việt thân thiện. | Khi tắt backend: không hiện 'Network Error' thô, stack trace hay mã HTML. | Heuristic: giúp người dùng khắc phục lỗi | | |
| GC04-16 | S2 | Sản phẩm không tồn tại (/product/99999) hiển thị trạng thái 'không tìm thấy' thân thiện. | Có thông báo + link về Trang chủ; không trắng trang hay hiện chữ debug. | Heuristic: giúp người dùng khắc phục lỗi | | |
| GC04-17 | S4 | Sau 3 lần đăng nhập sai liên tiếp, hiện thông báo khóa và chặn đăng nhập trong 30 giây. | Khóa đúng ở lần sai thứ 3 (không sớm hơn) và mở lại sau 30 giây. | FR-02 | | |

## Tài liệu tham khảo

- Đặc tả EShop: `eshop-sut/README.md` (FR-01 … FR-24, SEC-04)
- W3C, *Web Content Accessibility Guidelines (WCAG) 2.2* — https://www.w3.org/TR/WCAG22/
- J. Nielsen, *10 Usability Heuristics for User Interface Design* — https://www.nngroup.com/articles/ten-usability-heuristics/
