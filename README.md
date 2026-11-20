# Hotmail Reader – Website chỉ đọc email

Website tĩnh chạy trên GitHub Pages. Đăng nhập Microsoft bằng OAuth/MSAL và gọi Microsoft Graph trực tiếp từ trình duyệt. Không có backend, không lưu mật khẩu, không có chức năng gửi/xóa/sửa email.

## 1. Tạo ứng dụng Microsoft (miễn phí)

1. Mở https://entra.microsoft.com/ và đăng nhập bằng tài khoản Microsoft.
2. Vào **Identity → Applications → App registrations → New registration**.
3. Đặt tên, ví dụ `Hotmail Reader`.
4. Ở **Supported account types**, chọn **Accounts in any organizational directory and personal Microsoft accounts** (để hỗ trợ Outlook.com/Hotmail cá nhân).
5. Register.
6. Sao chép **Application (client) ID**.
7. Vào **Authentication → Add a platform → Single-page application (SPA)**.
8. Thêm Redirect URI chính xác của GitHub Pages, ví dụ:
   `https://TEN-GITHUB.github.io/hotmail-reader/`
   Thay `TEN-GITHUB` bằng tên tài khoản GitHub và `hotmail-reader` bằng tên repository của bạn.
9. Vào **API permissions → Add a permission → Microsoft Graph → Delegated permissions**.
10. Chỉ thêm `Mail.Read`. Không thêm `Mail.ReadWrite`, `Mail.Send` hay quyền khác.
11. Không tạo Client Secret. Ứng dụng SPA chạy trong trình duyệt không cần secret.

Tài liệu Microsoft:
- Đăng ký ứng dụng: https://learn.microsoft.com/en-us/graph/auth-register-app-v2
- Authority cho tài khoản cá nhân: https://learn.microsoft.com/en-us/entra/identity-platform/msal-client-application-configuration
- SPA và PKCE: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-auth-code-flow

## 2. Điền Client ID

Mở `index.html`, tìm dòng:

`const CLIENT_ID = "PASTE_YOUR_MICROSOFT_CLIENT_ID_HERE";`

Thay phần `PASTE_YOUR_MICROSOFT_CLIENT_ID_HERE` bằng Application (client) ID của bạn. Không cần thêm dấu ngoặc kép mới.

## 3. Đưa lên GitHub Pages

1. Đăng nhập https://github.com/
2. Bấm **New repository**.
3. Đặt tên `hotmail-reader`.
4. Chọn **Public** (GitHub Free hỗ trợ Pages cho repository public), tạo repository.
5. Bấm **Add file → Upload files**.
6. Upload `index.html` (và README nếu muốn), bấm **Commit changes**.
7. Vào **Settings → Pages**.
8. Trong **Build and deployment**, chọn **Deploy from a branch**.
9. Chọn branch `main`, folder `/ (root)`, bấm Save.
10. Chờ GitHub cấp link dạng `https://TEN-GITHUB.github.io/hotmail-reader/`.
11. Quay lại Microsoft Entra → App registrations → ứng dụng → Authentication, kiểm tra Redirect URI SPA đúng y hệt link này (có dấu `/` cuối).
12. Mở link website, bấm đăng nhập Microsoft và chấp thuận quyền đọc thư.

## Bảo mật và giới hạn

- Repository GitHub Pages thường công khai: mã nguồn và Client ID có thể nhìn thấy. Client ID là định danh ứng dụng công khai, không phải mật khẩu.
- Không bao giờ đưa mật khẩu Microsoft, client secret hoặc access token vào mã nguồn.
- Email chỉ được tải trực tiếp từ Microsoft Graph trong trình duyệt sau khi bạn đăng nhập và cấp quyền.
- Token dùng `sessionStorage` của tab trình duyệt; đóng phiên/trình duyệt có thể cần đăng nhập lại.
- Ứng dụng hiển thị nội dung email dạng văn bản thuần để tránh chạy mã HTML độc hại trong email.
- Phiên bản này hỗ trợ hộp thư đến, thư đã gửi, lưu trữ và thư rác; hiển thị từng trang 25 thư.
- Đính kèm được báo hiệu nhưng không tải trong phiên bản này.
- Đây là ứng dụng cá nhân tự triển khai; Microsoft có thể hiển thị cảnh báo ứng dụng chưa được nhà xuất bản xác minh. Chỉ cấp quyền nếu bạn kiểm tra đúng ứng dụng do mình tạo.
