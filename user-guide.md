# Hướng dẫn sử dụng NovaCore Browser (Toàn tập)

Chào mừng bạn đến với **NovaCore Browser** - giải pháp hàng đầu trong việc quản
lý đa tài khoản và bảo vệ danh tính số. Tài liệu này sẽ hướng dẫn bạn khai thác
tối đa sức mạnh của trình duyệt từ những bước cơ bản đến nâng cao.

---

## 1. Bắt đầu (Get Started)

### 1.1 Cài đặt

1. Tải về bộ cài đặt NovaCore Browser từ trang chủ hoặc link cung cấp.
2. Chạy file cài đặt. Ứng dụng sẽ tự động khởi động sau khi hoàn tất.
3. **Lưu ý**: Trong lần khởi đầu tiên, ứng dụng có thể mất vài giây để tải xuống
   "Nhân trình duyệt" (Browser Core). Vui lòng không đóng ứng dụng trong lúc
   này.

### 1.2 Đăng nhập & Xác thực OTP

NovaCore sử dụng cơ chế đăng nhập bảo mật qua mã OTP gửi về Email:

1. Nhập địa chỉ Email của bạn vào màn hình đăng nhập.
2. Kiểm tra hộp thư (bao gồm cả mục Spam) để lấy mã xác thực 6 số.
3. Nhập mã OTP vào ứng dụng để vào Dashboard chính.

---

## 2. Quản lý Hồ sơ (Profiles)

Hồ sơ (Profile) là "danh tính ảo" riêng biệt. Mỗi hồ sơ có Cookies, Cache và dấu
vân tay máy tính khác nhau.

### 2.1 Tạo Hồ sơ mới

1. Nhấn nút **"Create Profile"**.
2. **Name**: Đặt tên gợi nhớ cho tài khoản (ví dụ: FB_Ngo_Ngan).
3. **Group**: Phân nhóm để dễ tìm kiếm.
4. **Proxy**: Cấu hình địa chỉ mạng riêng cho hồ sơ (xem mục 2.2).
5. **Fingerprint**: Hệ thống sẽ tự động tạo một cấu hình vân tay "sạch". Bạn có
   thể nhấn **"Roll"** để đổi cấu hình khác nếu muốn.

### 2.2 Cấu hình Proxy

Để tránh bị liên kết tài khoản, mỗi hồ sơ nên dùng một Proxy riêng:

- **Định dạng hỗ trợ**: `IP:Port`, `IP:Port:User:Pass` hoặc `User:Pass@IP:Port`.
- **Thao tác nhanh**: Bạn chỉ cần copy chuỗi proxy và nhấn nút **"Paste"**, ứng
  dụng sẽ tự động phân tách các trường thông tin.
- **Check Proxy**: Luôn nhấn nút kiểm tra để đảm bảo Proxy còn hoạt động trước
  khi chạy.

### 2.3 Thao tác Hàng loạt (Bulk Actions)

Bạn có thể chọn nhiều hồ sơ cùng lúc để:

- **Launch**: Mở hàng loạt cửa sổ trình duyệt.
- **Stop**: Đóng nhanh toàn bộ trình duyệt đang chạy.
- **Delete**: Xóa sạch dữ liệu hồ sơ.

---

## 3. Duyệt Web & Tiện ích (Extensions)

### 3.1 Giao diện Trình duyệt

Mỗi cửa sổ trình duyệt sẽ có số thứ tự trên thanh Taskbar (ví dụ: `[#1]`,
`[#2]`) để bạn không bị nhầm lẫn giữa các tài khoản.

### 3.2 Cài đặt Extension (Chrome Web Store)

NovaCore hỗ trợ cài đặt Extension trực tiếp từ cửa hàng Chrome:

1. Truy cập `chromewebstore.google.com`.
2. Tìm kiếm tiện ích bạn cần.
3. Nhấn nút **"Add to Chromium"**. Hệ thống đã được tối ưu để tự động vá lỗi và
   cài đặt mượt mà.

---

## 4. Tự động hóa (Automation) - Không cần lập trình

Đây là tính năng mạnh mẽ nhất của NovaCore, giúp bạn tự động hóa các thao tác
lặp đi lặp lại.

### 4.1 Workflow Designer (Thiết kế Quy trình)

Truy cập mục **"Automation"** để bắt đầu:

- **Kéo-Thả**: Kéo các node từ thanh công cụ bên trái vào màn hình thiết kế.
- **Kết nối**: Nối các node lại với nhau để tạo thành dòng chảy hành động.
- **Node phổ biến**:
  - `Click`: Click vào nút trên trang web.
  - `Type`: Nhập dữ liệu, văn bản.
  - `Wait`: Chờ trang load hoặc chờ một khoảng thời gian.
  - `AI (GPT/Gemini)`: Sử dụng trí tuệ nhân tạo để xử lý nội dung.

### 4.2 Chế độ Rà soát (Inspector)

Bạn không cần biết code để chọn phần tử web:

1. Bật trình duyệt hồ sơ.
2. Trên màn hình Automation, nhấn **"Inspect Browser"**.
3. **Thông minh**: Nếu trình duyệt chưa chạy, NovaCore sẽ tự động khởi chạy hồ
   sơ và hiển thị trạng thái **"Đang kết nối..."**.
4. Di chuyển chuột trên trình duyệt và click vào nút/ô nhập liệu bạn muốn.
   NovaCore sẽ tự động tạo Node tương ứng cho bạn. Trạng thái chọn sẽ được hiển
   thị bằng khung vàng (hover) và khung xanh lá (click).

---

## 5. Đồng bộ hóa (Sync Center)

Tính năng **Synchronizer** cho phép bạn điều khiển 1 trình duyệt (Master) và tất
cả các trình duyệt khác (Slaves) sẽ làm theo y hệt:

1. Mở các trình duyệt cần đồng bộ.
2. Vào tab **"Sync Center"**.
3. Chọn trình duyệt làm **Master**.
4. Các hành động: Click, Cuộn chuột, Gõ phím trên Master sẽ được lặp lại ngay
   lập tức trên các Slaves.

---

## 6. Gói cước & Đăng ký (Subscription)

NovaCore cung cấp nhiều cấp độ gói cước dựa trên nhu cầu:

- **Free**: Phù hợp để trải nghiệm.
- **Pro/Agency**: Tăng giới hạn số lượng hồ sơ và số luồng tự động hóa chạy cùng
  lúc.

### Thanh toán VietQR

1. Vào mục **"Pricing"**.
2. Chọn gói cước và nhấn **"Upgrade"**.
3. Quét mã VietQR hiển thị trên màn hình. Hệ thống sẽ tự động kích hoạt gói ngay
   sau khi bạn chuyển khoản thành công (thông qua hệ thống SePay).

---

## 7. Cập nhật & Bảo mật

### 7.1 Tự động cập nhật (Auto-Update)

NovaCore sẽ tự động thông báo khi có bản mới:

- **Silent Update**: Tự động tải ngầm và áp dụng khi bạn khởi động lại ứng dụng.
- **Mandatory Update**: Một số bản cập nhật quan trọng (nhằm vá lỗi nhân trình
  duyệt) sẽ yêu cầu bạn nâng cấp ngay để đảm bảo an toàn.

### 7.2 Bảo mật vân tay

Mọi thông số về phần cứng (CPU, Card đồ họa, Font chữ) đều được NovaCore giả lập
ở lớp sâu nhất, giúp bạn "vượt rào" các hệ thống kiểm tra gắt gao nhất hiện nay.

---

## 🆘 Hỗ trợ

Nếu gặp bất kỳ khó khăn nào, vui lòng liên hệ đội ngũ hỗ trợ qua:

- **Cộng đồng**: [Link Group]
- **Tài liệu kỹ thuật**: [Link Documentation]

_Chúc bạn có những trải nghiệm tuyệt vời cùng NovaCore Browser!_
