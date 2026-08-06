# Tuần 2: Clean Architecture và Authentication Flow

**Thời lượng:** Ngày 6 - Ngày 12  
**Chủ đề:** Tổ chức code theo Clean Architecture, xác thực người dùng, quản lý token và đồng bộ online/offline.

## Mục tiêu tuần

- Hiểu vai trò của Data, Domain và Presentation Layer.
- Tổ chức lại project theo cấu trúc dễ mở rộng và kiểm thử.
- Xây dựng luồng Đăng nhập, Đăng ký và Quên mật khẩu.
- Lưu token an toàn và tự động đính kèm token vào request.
- Xử lý refresh token khi phiên đăng nhập hết hạn.
- Lưu dữ liệu offline và đồng bộ lại khi có mạng.
- Hoàn thiện Mini Project 2 và đưa code vào nhánh chính sau review.

## Quy ước thực hiện

- Mỗi tính năng tạo branch riêng, không làm trực tiếp trên `main`.
- PR cần nêu rõ mục tiêu, phạm vi thay đổi, cách kiểm thử và ảnh hưởng tới tính năng cũ.
- Không lưu password, token thật hoặc secret trong source code.
- Ưu tiên xử lý lỗi ở một lớp dùng chung thay vì lặp lại ở từng màn hình.

## Kế hoạch theo ngày

### Ngày 6: Kiến trúc dự án

#### Mục tiêu
Hiểu cách chia trách nhiệm giữa các layer và áp dụng vào project hiện tại.

#### Công việc thực hiện

1. Nghiên cứu Data Layer, Domain Layer và Presentation Layer.
2. Xác định vị trí của API service, repository, entity, use case và state management.
3. Tổ chức lại folder theo chuẩn Clean Architecture của team.
4. Di chuyển một feature mẫu từ cấu trúc cũ sang cấu trúc mới.
5. Viết ghi chú giải thích trách nhiệm của từng thư mục.

#### Sản phẩm bàn giao

- Cấu trúc folder mới.
- Sơ đồ hoặc tài liệu ngắn mô tả luồng dữ liệu.
- PR refactor feature mẫu.

#### Tiêu chí hoàn thành

- UI không phụ thuộc trực tiếp vào implementation của API.
- Repository là điểm trung gian giữa Domain và Data.
- Tên file, folder và class thống nhất với convention của team.

### Ngày 7: Xây dựng Authentication Flow

#### Mục tiêu
Xây dựng đầy đủ UI và logic cơ bản cho người dùng xác thực tài khoản.

#### Công việc thực hiện

1. Dựng màn hình Đăng nhập, Đăng ký và Quên mật khẩu.
2. Validate Email, Password, Confirm Password và trạng thái bắt buộc nhập.
3. Hiển thị lỗi ngay tại field, không chỉ thông báo chung.
4. Kết nối API Login/Register thực tế hoặc mock API theo hướng dẫn.
5. Xử lý trạng thái submit, thành công, thất bại và disable nút khi đang gửi.
6. Điều hướng đúng sau khi đăng nhập hoặc đăng ký thành công.

#### Sản phẩm bàn giao

- PR Authentication Flow.
- Video demo các trường hợp hợp lệ và không hợp lệ.
- Tài liệu ngắn về các rule validate.

### Ngày 8: Quản lý Token và Dio Interceptor

#### Mục tiêu
Duy trì phiên đăng nhập an toàn và giảm code lặp trong việc gửi token.

#### Công việc thực hiện

1. Lưu Auth Token bằng `flutter_secure_storage`.
2. Tạo `Dio Interceptor` tự động thêm token vào Header.
3. Xử lý lỗi `401 Unauthorized`.
4. Thực hiện refresh token nếu API hỗ trợ.
5. Nếu refresh thất bại, xóa phiên và đưa người dùng về màn hình đăng nhập.
6. Tránh gửi nhiều request refresh token đồng thời.
7. Kiểm tra token không xuất hiện trong log production.

#### Sản phẩm bàn giao

- Module Auth duy trì phiên đăng nhập.
- PR kèm sơ đồ xử lý token và lỗi 401.

### Ngày 9: Cơ sở dữ liệu Local

#### Mục tiêu
Lưu được dữ liệu nghiệp vụ đơn giản để app có thể sử dụng khi không có mạng.

#### Công việc thực hiện

1. So sánh nhanh Hive, Isar và SQLite/Sqflite theo nhu cầu project.
2. Chọn một giải pháp và ghi rõ lý do lựa chọn.
3. Thiết kế model lưu sản phẩm yêu thích hoặc lịch sử tìm kiếm.
4. Thực hiện thêm, đọc, cập nhật và xóa dữ liệu local.
5. Xử lý trường hợp dữ liệu cũ không còn đúng format.

#### Sản phẩm bàn giao

- PR tính năng cache dữ liệu offline.
- Tài liệu lựa chọn giải pháp local storage.
- Video demo dữ liệu vẫn hiển thị khi tắt mạng.

### Ngày 10: Đồng bộ Online - Offline

#### Mục tiêu
Đảm bảo trải nghiệm sử dụng ổn định khi mạng chập chờn hoặc bị ngắt.

#### Công việc thực hiện

1. Theo dõi trạng thái mạng bằng `connectivity_plus`.
2. Ưu tiên dữ liệu cache khi không có mạng.
3. Đồng bộ dữ liệu mới khi kết nối trở lại.
4. Hiển thị No Internet Connection Banner nhưng không che nội dung chính.
5. Xử lý timeout, retry và tránh gọi API lặp vô hạn.
6. Kiểm tra các tình huống tắt mạng giữa lúc đang gọi API.

#### Sản phẩm bàn giao

- App vẫn hiển thị dữ liệu gần nhất khi offline.
- PR logic đồng bộ online/offline.
- Danh sách tình huống đã kiểm thử.

### Ngày 11: Hoàn thiện Mini Project 2

#### Công việc thực hiện

- Ghép nối Login -> Lưu Token -> Lấy Data -> Cache Local.
- Kiểm tra logout, token hết hạn và mở lại app.
- Format code, chạy analyzer và kiểm tra các subscription/controller.
- Kiểm tra memory leak đơn giản và các màn hình điều hướng nhiều lần.
- Cập nhật README và tạo PR hoàn chỉnh.

#### Sản phẩm bàn giao

- PR Mini Project 2.
- Video demo online/offline.
- Checklist kiểm thử authentication và cache.

### Ngày 12: Review và thống nhất Main Project

#### Công việc thực hiện

1. Mentor review code qua PR.
2. Nhân viên phân loại feedback thành Convention, Logic, UI và Performance.
3. Sửa các lỗi được yêu cầu và cập nhật PR.
4. Thống nhất đề tài, phạm vi, tính năng và tiêu chí hoàn thành Main Project.
5. Chuyển các đầu việc của Main Project thành task có người phụ trách và deadline.

#### Sản phẩm bàn giao

- Code Mini Project 2 đã được merge vào nhánh chính.
- Tài liệu yêu cầu và phạm vi Main Project.

## Checklist cuối tuần

- [ ] Project có cấu trúc layer rõ ràng.
- [ ] Authentication xử lý được validate và lỗi API.
- [ ] Token được lưu an toàn, không hard-code.
- [ ] Có xử lý 401 và logout khi phiên hết hạn.
- [ ] Dữ liệu quan trọng có cache offline.
- [ ] App có thông báo trạng thái mạng.
- [ ] PR đã được review và merge.
