# Tuần 4: Hiệu năng, đa ngôn ngữ, release và final demo

**Thời lượng:** Ngày 20 - Ngày 25  
**Chủ đề:** Đo lường và tối ưu chất lượng sản phẩm, chuẩn bị bản release, hoàn thiện tài liệu và trình bày dự án.

## Mục tiêu tuần

- Biết đo hiệu năng thay vì tối ưu theo cảm tính.
- Giảm rebuild không cần thiết, memory leak và dung lượng app.
- Tích hợp Localization tiếng Việt/tiếng Anh và Dynamic Theme.
- Hiểu quy trình ký app và tạo bản build Release.
- Rà soát UI/UX, edge case và các tình huống lỗi thực tế.
- Hoàn thiện README, codebase và nội dung Final Demo.

## Kế hoạch theo ngày

### Ngày 20: Tối ưu hiệu năng

#### Mục tiêu
Xác định vấn đề hiệu năng bằng số liệu và chứng minh kết quả sau tối ưu.

#### Công việc thực hiện

1. Dùng Flutter DevTools kiểm tra FPS, CPU, Memory và Widget Rebuild.
2. Ghi nhận vấn đề trước khi chỉnh sửa.
3. Sử dụng `const` constructor ở nơi phù hợp.
4. Giảm `setState` không cần thiết và giới hạn phạm vi rebuild.
5. Tối ưu kích thước, định dạng và cách tải hình ảnh.
6. Kiểm tra việc giải phóng controller, listener, stream và animation.
7. Đo lại sau tối ưu, ghi rõ thay đổi và tác động.

#### Sản phẩm bàn giao

- Báo cáo DevTools trước và sau tối ưu.
- PR performance improvement.

### Ngày 21: Localization và Dynamic Theme

#### Mục tiêu
Cho phép người dùng thay đổi ngôn ngữ và giao diện mà không cần khởi động lại app.

#### Công việc thực hiện

1. Tích hợp `flutter_localizations`.
2. Tách toàn bộ text hiển thị khỏi Widget.
3. Bổ sung bản dịch tiếng Việt và tiếng Anh cho các màn hình chính.
4. Xử lý text dài, plural và trường hợp thiếu key dịch.
5. Cho phép đổi ngôn ngữ trong app.
6. Cho phép chuyển Light/Dark Theme và lưu lựa chọn của người dùng.
7. Kiểm tra layout sau khi đổi ngôn ngữ.

#### Sản phẩm bàn giao

- App chuyển đổi mượt giữa hai ngôn ngữ và hai giao diện.
- PR Localization và Dynamic Theme.

### Ngày 22: Sign App và Build Release

#### Mục tiêu
Tạo được bản build Release có thể cài và chạy ổn định trên thiết bị thật.

#### Công việc thực hiện

1. Nghiên cứu Keystore Android và Provisioning Profile iOS.
2. Phân biệt cấu hình Debug, Profile và Release.
3. Tạo APK/AAB Release theo hướng dẫn của team.
4. Bật R8/ProGuard và obfuscate code khi phù hợp.
5. Kiểm tra app sau khi tối ưu dung lượng.
6. Không đưa certificate, keystore hoặc password vào repository.
7. Ghi lại các bước build để người khác có thể thực hiện lại.

#### Sản phẩm bàn giao

- Bản build Release APK chạy ổn định trên thiết bị thật.
- Tài liệu build/sign không chứa secret.

### Ngày 23: Tối ưu UI/UX và xử lý Edge Case

#### Mục tiêu
Đảm bảo app sử dụng ổn định và giao diện không bị vỡ trong các điều kiện phổ biến.

#### Công việc thực hiện

1. Rà soát màn hình trên nhiều kích thước thiết bị.
2. Kiểm tra text dài, dữ liệu thiếu, danh sách rỗng và ảnh lỗi.
3. Xử lý mất mạng đột ngột, timeout API và lỗi server.
4. Xử lý hết phiên đăng nhập, logout và mở lại app.
5. Kiểm tra bàn phím, safe area, xoay màn hình và thao tác back.
6. Sửa các lỗi crash hoặc lỗi giao diện cơ bản.

#### Sản phẩm bàn giao

- App hoàn thiện, không còn crash nghiêm trọng.
- Checklist kiểm thử UI/UX và edge case.
- Danh sách lỗi còn lại, nếu có.

### Ngày 24: Hoàn thiện Codebase và Documentation

#### Công việc thực hiện

1. Chạy linter, formatter và analyzer.
2. Xóa code dư thừa, import không dùng và comment không còn giá trị.
3. Kiểm tra tên file, tên class, cấu trúc folder và các TODO còn tồn tại.
4. Viết README chất lượng gồm:
   - Mô tả sản phẩm và tính năng chính.
   - Yêu cầu môi trường.
   - Hướng dẫn cài đặt và chạy project.
   - Kiến trúc sử dụng.
   - Danh sách thư viện chính.
   - Cách build và các giới hạn đã biết.
5. Cập nhật changelog hoặc release note ngắn.

#### Sản phẩm bàn giao

- Repo Main Project được làm sạch.
- README đầy đủ.
- PR documentation và cleanup.

### Ngày 25: Code Freeze và Final Demo

#### Mục tiêu
Đóng băng phiên bản cuối và trình bày được quá trình thực hiện dự án.

#### Công việc thực hiện

1. Mentor chốt danh sách tính năng và code quality.
2. Chỉ sửa blocker hoặc lỗi nghiêm trọng sau thời điểm Code Freeze.
3. Chuẩn bị slide/kịch bản Final Demo gồm:
   - Bối cảnh và bài toán.
   - Giải pháp và kiến trúc.
   - Các tính năng chính.
   - Điểm khó và cách giải quyết.
   - Demo luồng sử dụng.
   - Kết quả, hạn chế và hướng phát triển.
4. Chuẩn bị bản build, tài khoản demo và dữ liệu demo.
5. Tự kiểm tra toàn bộ luồng trước buổi trình bày.

#### Sản phẩm bàn giao

- Main Project đã Code Freeze.
- Bản build cuối.
- Slide hoặc kịch bản Final Demo.
- Link repository và README hoàn chỉnh.

## Tiêu chí đánh giá cuối kỳ

- **Chức năng:** Các luồng chính hoạt động đúng, có xử lý lỗi hợp lý.
- **Kiến trúc:** Code phân lớp rõ ràng, dễ đọc và dễ mở rộng.
- **UI/UX:** Giao diện nhất quán, responsive và có trạng thái đầy đủ.
- **Chất lượng:** Không có lỗi analyzer nghiêm trọng, code đã format.
- **Kỹ năng làm việc:** Commit, branch, PR và tiếp nhận feedback đúng quy trình.
- **Trình bày:** Có thể giải thích quyết định kỹ thuật và demo sản phẩm mạch lạc.

## Checklist trước Final Demo

- [ ] Bản Release đã được cài thử trên thiết bị thật.
- [ ] Luồng đăng nhập, API, cache, notification và deep link đã kiểm tra.
- [ ] Không còn secret trong repository.
- [ ] README có hướng dẫn chạy đầy đủ.
- [ ] Slide/kịch bản demo đã hoàn thành.
- [ ] Có dữ liệu và tài khoản demo dự phòng.
- [ ] Đã ghi nhận hạn chế và hướng phát triển tiếp theo.
