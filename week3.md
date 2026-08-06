# Tuần 3: Main Project và các kỹ thuật nâng cao

**Thời lượng:** Ngày 13 - Ngày 19  
**Chủ đề:** Khởi tạo Main Project, xây dựng nền tảng UI, tích hợp phần cứng, thông báo đẩy và deep linking.

## Mục tiêu tuần

- Biết phân tích một đề bài từ yêu cầu đến danh sách task.
- Khởi tạo project theo Clean Architecture và convention của team.
- Xây dựng Design System và Custom Widget có thể tái sử dụng.
- Tích hợp camera/gallery, upload file và cấp quyền thiết bị.
- Tích hợp Firebase Cloud Messaging.
- Hiểu WebView và nguyên lý Deep Link.
- Có bản build thử nghiệm đầu tiên của Main Project.

## Kế hoạch theo ngày

### Ngày 13: Phân tích và setup Main Project

#### Mục tiêu
Biến yêu cầu nghiệp vụ thành kế hoạch phát triển có thể theo dõi.

#### Công việc thực hiện

1. Đọc đề bài và xác định nhóm người dùng, luồng chính, dữ liệu và ràng buộc.
2. Chia tính năng thành Epic, User Story và task nhỏ.
3. Tạo WBS hoặc Trello board, gắn độ ưu tiên và ước lượng sơ bộ.
4. Khởi tạo repository và quy ước branch.
5. Setup Clean Architecture, Base Theme, Base Network và môi trường chạy.
6. Tạo README ban đầu và ghi rõ cách cài đặt project.

#### Sản phẩm bàn giao

- Link repository.
- Board phân chia công việc.
- Tài liệu phân tích yêu cầu và phạm vi phiên bản đầu tiên.

### Ngày 14: Core UI và Custom Widgets

#### Mục tiêu
Xây dựng nền tảng giao diện nhất quán trước khi phát triển các màn hình nghiệp vụ.

#### Công việc thực hiện

1. Tạo Custom Button, Custom TextField, Loading Dialog và Error View.
2. Cấu hình Light/Dark Theme, màu chủ đạo, kiểu chữ và khoảng cách cơ bản.
3. Xác định các trạng thái disabled, loading, error và validation của component.
4. Đảm bảo component có tên rõ ràng, API đơn giản và không phụ thuộc màn hình cụ thể.
5. Tạo một màn hình mẫu để kiểm tra toàn bộ Design System.

#### Sản phẩm bàn giao

- PR Base UI và Design System.
- Màn hình mẫu hoặc catalog component.

### Ngày 15: Device Hardware và Permissions

#### Mục tiêu
Tích hợp tính năng chọn ảnh/chụp ảnh và upload lên server an toàn.

#### Công việc thực hiện

1. Tích hợp `image_picker` cho Camera và Gallery.
2. Xử lý quyền bị từ chối, quyền bị từ chối vĩnh viễn và người dùng hủy chọn ảnh.
3. Kiểm tra định dạng, kích thước và dung lượng file trước khi upload.
4. Upload file/ảnh bằng `FormData` qua API.
5. Hiển thị tiến trình upload, trạng thái thành công và lỗi.
6. Không ghi log dữ liệu nhạy cảm trong file ảnh hoặc response.

#### Sản phẩm bàn giao

- Tính năng đổi Avatar hoặc upload ảnh chứng từ.
- PR kèm danh sách permission đã cấu hình.

### Ngày 16: Push Notification với FCM

#### Mục tiêu
Nhận và xử lý thông báo trong các trạng thái hoạt động khác nhau của app.

#### Công việc thực hiện

1. Tích hợp Firebase vào dự án Flutter.
2. Cấu hình nhận notification ở Foreground, Background và Terminated state.
3. Xử lý permission notification theo nền tảng.
4. Đọc payload và điều hướng đến màn hình tương ứng khi người dùng bấm thông báo.
5. Kiểm tra token thiết bị và cách xử lý khi token được refresh.
6. Ghi chú khác biệt giữa notification message và data message.

#### Sản phẩm bàn giao

- Nhận thành công notification thử nghiệm từ Firebase Console.
- Video demo các trạng thái nhận thông báo.
- PR tích hợp FCM.

### Ngày 17: WebView và Deep Linking

#### Mục tiêu
Hiển thị nội dung web trong app và hiểu cách mở app từ URL bên ngoài.

#### Công việc thực hiện

1. Tích hợp WebView cho Điều khoản, Chính sách hoặc cổng thanh toán.
2. Xử lý loading, lỗi tải trang, nút back và giới hạn URL được phép mở.
3. Nghiên cứu App Links trên Android và Universal Links trên iOS.
4. Xác định route khi app đang mở, chạy nền hoặc chưa được khởi động.
5. Viết tài liệu tóm tắt payload và luồng điều hướng Deep Link.

#### Sản phẩm bàn giao

- PR WebView.
- Tài liệu tóm tắt Deep Link và các giới hạn triển khai.

### Ngày 18: Tối ưu Main Project giai đoạn 1

#### Công việc thực hiện

- Tích hợp các tính năng từ ngày 13 đến ngày 17 vào Main Project.
- Kiểm tra luồng chính từ khởi động app đến các tính năng đã hoàn thành.
- Xử lý edge case trên màn hình nhỏ, lớn, xoay màn hình và dữ liệu dài.
- Kiểm tra permission, offline, timeout và điều hướng từ notification/deep link.
- Tạo bản build APK/IPA thử nghiệm giai đoạn 1.

#### Sản phẩm bàn giao

- Bản build thử nghiệm.
- Changelog ngắn.
- Danh sách lỗi đã biết và hướng xử lý dự kiến.

### Ngày 19: Review tuần 3

#### Công việc thực hiện

1. Mentor test Demo app theo checklist.
2. Review cấu trúc code, UX, khả năng xử lý lỗi và tính nhất quán giao diện.
3. Ghi nhận feedback thành các task có mức độ ưu tiên.
4. Chọn các hạng mục cần fix/refactor trong tuần cuối.

#### Sản phẩm bàn giao

- Biên bản review.
- Danh sách Fix/Refactor cho tuần 4.

## Checklist cuối tuần

- [ ] Main Project có repository, README và board công việc.
- [ ] Có nền tảng Clean Architecture và Base UI.
- [ ] Upload ảnh xử lý được permission và lỗi.
- [ ] FCM hoạt động ở các trạng thái chính.
- [ ] WebView có xử lý loading và lỗi.
- [ ] Có tài liệu Deep Link.
- [ ] Có bản build thử nghiệm và danh sách việc cần cải thiện.
