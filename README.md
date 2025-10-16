
Đây là tổng hợp của các repository 

link user:https://github.com/LongPH51747/s7mstore.git

link admin:https://github.com/LongPH51747/s7m_admin.git

link shipper:https://github.com/LongPH51747/s7mdrive.git

link backend:https://github.com/LongPH51747/s7m_backend.git

link demo user:https://youtu.be/XdffIX05k5s

S7M Store – Tóm tắt chức năng 

Giới thiệu
- Ứng dụng mua sắm thời trang S7M Store (React Native) với các tính năng mua hàng, giỏ hàng, thanh toán COD/MoMo, theo dõi đơn, thông báo đẩy, chat người dùng–admin.
- Backend được cấu hình qua API base: xem `src/config/api.js`.

Cấu trúc chính
- `screens/`: Màn hình tính năng người dùng
- `components/`: Thành phần giao diện tái sử dụng (UI)
- `contexts/`: Context quản lý trạng thái (thông báo, socket/chat)
- `navigation/`: Điều hướng stack `AppNavigator`
- `services/`: Tích hợp dịch vụ (Firebase, Chatbot, Database,...)
- `utils/`: Tiện ích (chuyển đổi trạng thái đơn, khởi tạo SDK)
- `config/`: Cấu hình API

Điểm nhấn tính năng theo module

1) Trang chủ và danh mục/sản phẩm
- `HomeScreen.js`:
  - Tải banner, danh mục, danh sách sản phẩm phân trang (All) và lọc theo danh mục, tìm kiếm theo tên.
  - Các khối: Top sale, Sản phẩm mới (phân trang ngang), gợi ý “Có thể bạn sẽ thích”.
  - Điều hướng nhanh đáy: Home, Cart, Notification, Profile.
  - Refresh dữ liệu sau thanh toán thành công (dựa trên AsyncStorage flags).
  = AI chatbot tư vấn sản phẩm
    
2) Chi tiết sản phẩm và đánh giá
- `ProductDetailScreen.js`:
  - Lấy dữ liệu sản phẩm theo `id`, tự chọn biến thể còn hàng đầu tiên, hiển thị ảnh theo biến thể.
  - Chọn biến thể màu/kích cỡ (kiểm tra tồn kho), thêm vào giỏ, mua ngay.
  - Hiển thị mô tả rút gọn/mở rộng, trung bình rating, danh sách đánh giá (kèm ảnh), phản hồi admin.
  - Sau thanh toán thành công: reset số lượng, refresh tồn kho.

3) Giỏ hàng và thanh toán
- `CartScreen.js`:
  - Lấy giỏ hàng theo người dùng, chọn/bỏ chọn sản phẩm, chọn tất cả.
  - Cập nhật số lượng có debounce, kiểm tra tồn kho biến thể thực tế.
  - Xóa từng item (xác nhận), tính tạm tính/giảm giá/phí ship và điều hướng sang Checkout.
- `CheckoutScreen.js`:
  - Nhận `cartItems` từ giỏ hoặc “mua ngay”.
  - Chọn/hiển thị địa chỉ mặc định; ghi chú đơn; áp dụng voucher (qua API) và tính tổng.
  - Phương thức thanh toán: COD hoặc MoMo. Tạo đơn hàng, điều hướng `PaymentSuccessScreen` (COD) hoặc mở deep link MoMo rồi cập nhật cờ `orderCreated`.

4) Đơn hàng và chi tiết đơn
- `OrderScreen.js`:
  - Tabs trạng thái: Chờ xác nhận, Đã xác nhận, Chờ giao, Giao thành công, Trả hàng, Đã hủy.
  - Lọc theo trạng thái, expand để xem thêm sản phẩm trong đơn; điều hướng “Xem chi tiết”, “Đánh giá” khi phù hợp.
- `OrderDetailScreen.js`:
  - Hiển thị chi tiết người nhận, danh sách sản phẩm, phí ship/giảm giá/tổng cộng.
  - Hủy đơn khi còn “Chờ xác nhận”, điều hướng theo dõi hành trình, trả hàng, mua lại.
- `OrderTrackingScreen.js`:
  - Theo dõi tiến trình (sử dụng dữ liệu history từ API, cấu trúc xem file màn hình này).

5) Tìm kiếm và kết quả
- `SearchScreen.js`, `SearchResultsScreen.js`:
  - Nhập từ khóa, gọi API search, hiển thị danh sách kết quả và điều hướng chi tiết.

6) Tài khoản và hồ sơ
- `LoginScreen.js`, `SignUpScreen.js((OTP/email))`, `ForgotPassword.js`, `OtpScreen.js`, `ResetPassword.js`, `ChangePassword.js`:
  - Đăng nhập, đăng ký, quên mật khẩu (OTP/email), đổi mật khẩu.
- `ProfileScreen.js`, `EditProfileScreen.js`:
  - Xem/sửa thông tin cá nhân, điều hướng tới lịch sử đơn, địa chỉ, thông báo, chat.

7) Địa chỉ giao hàng
- `AddressScreen.js`, `AddAddressScreen.js`, `UpdateAddressScreen.js`:
  - Quản lý địa chỉ, đặt mặc định, chọn địa chỉ cho đơn hàng. Có thể sử dụng `MapScreen.js` để chọn vị trí (xem chi tiết file).

8) Voucher/Khuyến mãi
- `VoucherScreen.js`, `VoucherModal.js`:
  - Lấy danh sách voucher theo người dùng, chọn voucher để áp dụng tại `CheckoutScreen` (API trả discount/tổng mới).

9) Thông báo đẩy & trung tâm thông báo
- `contexts/NotificationContext.js`:
  - Tạo kênh thông báo, xin quyền Android 13+, lưu trữ thông báo trong AsyncStorage.
  - Polling sản phẩm mới và thay đổi trạng thái đơn; lọc trùng; bắn local notifications.
  - API tiện ích: đánh dấu đã đọc, xóa, reset hệ thống, force-check.
- `NotificationScreen.js`:
  - UI danh sách thông báo (xem file để biết chi tiết trình bày và tương tác).

10) Chat người dùng – admin (Socket.IO)
- `contexts/SocketContext.js`:
  - Quản lý xác thực cục bộ (JWT, `userInfo`), kết nối Socket.IO bằng `accessToken` và `_id` MongoDB.
  - Nhận lịch sử, nhận/gửi tin nhắn, đánh dấu đã đọc, xóa tin; quản lý phòng (admin).
- `ChatScreen.js`:
  - Gửi tin nhắn văn bản/ảnh (upload ảnh, xem full screen), tự động gửi thông tin sản phẩm từ trang chi tiết vào chat.

11) Thành phần UI tái sử dụng
- `components/`:
  - `Loading`, `CustomAlert`, `CustomAlertDelete`, `CustomAlertModal`, `CustomNavBottom`, `customTextInput`, `ImageMessage`, `NotificationPopup`, `ProductInfoCard`.

12) Dịch vụ và SDK
- `services/firebaseConfig.js`: khởi tạo Firebase Auth.
- `utils/initializeSdks.tsx`: khởi tạo Firebase app (nếu cần), Google Sign-In, SQLite và import dữ liệu địa lý.
- `services/chatbotService.js`: wrapper gọi Gemini API (lưu ý khóa API trong mã nguồn).

13) Cấu hình và tiện ích
- `config/api.js`: `API_BASE_URL` và các `API_ENDPOINTS` cho Auth/Product/Category/Cart/Order/Address/Voucher/Review…
- `utils/orderStatusUtils.js`: chuyển đổi trạng thái đơn giữa dạng chữ và số, nhóm trạng thái.

14) Điều hướng ứng dụng
- `navigation/AppNavigator.js`:
  - Khai báo toàn bộ màn hình, ẩn header mặc định, `initialRouteName="HomeScreen"`.
  - Xuất `global.navigationService` để điều hướng từ thông báo local.

Luồng chính (tóm tắt)
- Khám phá: Home → xem danh mục/sản phẩm → ProductDetail.
- Mua hàng: ProductDetail → Thêm giỏ hàng hoặc Mua ngay → Cart/Checkout → Chọn địa chỉ / voucher / phương thức thanh toán → Tạo đơn hàng → Admin xác nhận đơn → Gửi đơn đến kho gần khu vực người mua (ví dụ: "Hà Nội") → Shipper khu vực đó nhận đơn và giao hàng → PaymentSuccess.
- Sau mua: OrderScreen → lọc theo trạng thái → OrderDetail/Tracking → Đánh giá(check ảnh nhạy cảm bằng AI nudecheck)/Trả hàng/Mua lại.
- Tiện ích: Thông báo đẩy cập nhật sản phẩm/đơn; Chat hỗ trợ; Tìm kiếm.

Lưu ý tích hợp
- Thay `API_BASE_URL` trong `config/api.js` theo môi trường của bạn.
- Yêu cầu cấu hình Firebase (google-services) và quyền thông báo trên Android 13+.
- MoMo: backend cần trả `deeplink` hợp lệ để mở ứng dụng thanh toán.
  



