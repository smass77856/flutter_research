# Tuần 1: REST API, State Management và Mini Project 1

**Thời lượng:** Ngày 3 - Ngày 5  
**Chủ đề:** Kết nối API, quản lý trạng thái, lưu trữ cục bộ và hoàn thiện ứng dụng mini E-commerce.

## Mục tiêu tuần

Sau tuần này, nhân viên có thể:

- Kết nối và sử dụng RESTful API trong Flutter bằng `Dio`.
- Chuyển đổi dữ liệu JSON thành Model an toàn, dễ bảo trì.
- Hiển thị dữ liệu lên UI với đầy đủ trạng thái Loading, Success, Error và Empty.
- Tách Business Logic khỏi Widget UI bằng BLoC hoặc Provider theo chuẩn của team.
- Lưu các cấu hình đơn giản bằng `shared_preferences`.
- Hoàn thiện quy trình cơ bản: Login -> List API -> Detail -> Save Local.

## Quy ước thực hiện

- Tạo branch riêng theo định dạng: `feature/week1-api-state`.
- Commit ngắn gọn, có ý nghĩa, ví dụ: `feat: connect product list api`.
- Không commit API key, token thật hoặc thông tin nhạy cảm.
- Tạo Pull Request kèm mô tả thay đổi, ảnh/video demo và các vấn đề còn tồn tại.
- Chạy `dart format .` và `flutter analyze` trước khi mở PR.

## Kế hoạch theo ngày

### Ngày 3: Tương tác RESTful API và Networking

#### Mục tiêu
Xây dựng được một màn hình lấy danh sách dữ liệu từ API và xử lý các tình huống mạng phổ biến.

#### Kiến thức cần nắm

- HTTP method, status code và cấu trúc response JSON.
- Cách cấu hình `Dio`, Base URL, headers, timeout.
- Interceptor và mục đích của việc log request/response.
- Phân biệt lỗi mạng, lỗi server, lỗi parse dữ liệu và dữ liệu rỗng.
- Nguyên tắc không gọi API trực tiếp trong Widget UI.

#### Công việc thực hiện

1. Nghiên cứu `Dio`, tập trung vào Interceptors, timeout và xử lý exception.
2. Tạo lớp gọi API cho danh sách sản phẩm hoặc dữ liệu tương đương.
3. Chuyển JSON sang Model bằng `json_serializable` hoặc `freezed`.
4. Hiển thị dữ liệu lên UI.
5. Xử lý bốn trạng thái: Loading, Success, Error và Empty.
6. Thêm thao tác retry khi request thất bại.

#### Sản phẩm bàn giao

- Pull Request kết nối API danh sách.
- Model và lớp API tương ứng.
- Video hoặc ảnh minh họa bốn trạng thái UI.
- Ghi chú ngắn về cách xử lý lỗi mạng.

#### Tiêu chí hoàn thành

- API được gọi đúng và không làm treo UI.
- UI có trạng thái rõ ràng khi đang tải, thành công, lỗi hoặc không có dữ liệu.
- Code có phân tách tối thiểu giữa API, Model và UI.
- Không có lỗi từ `flutter analyze`.

### Ngày 4: State Management và Shared Preferences

#### Mục tiêu
Refactor phần đã làm ngày 3 theo kiến trúc quản lý trạng thái thống nhất của team.

#### Công việc thực hiện

1. Chọn BLoC hoặc Provider theo convention của dự án.
2. Đưa Business Logic ra khỏi Widget UI.
3. Định nghĩa state rõ ràng, tối thiểu gồm Initial, Loading, Success, Error và Empty.
4. Xử lý các sự kiện như tải dữ liệu, retry và refresh.
5. Sử dụng `shared_preferences` để lưu Theme mode và một Token giả định.
6. Đảm bảo controller, stream hoặc provider được giải phóng đúng cách.
7. Vẽ sơ đồ luồng dữ liệu từ UI đến API và ngược lại.

#### Sản phẩm bàn giao

- Code refactor theo chuẩn State Management của team.
- Sơ đồ State Flow.
- Ghi chú giải thích trách nhiệm của từng lớp hoặc thành phần.

#### Tiêu chí hoàn thành

- Widget chỉ tập trung vào hiển thị và nhận tương tác người dùng.
- Logic gọi API và xử lý state không nằm trực tiếp trong UI.
- Có thể thay đổi dữ liệu hoặc gọi lại API mà không phải sửa nhiều Widget.
- Dữ liệu cấu hình được lưu và đọc lại chính xác khi mở lại app.

### Ngày 5: Hoàn thiện Mini Project 1 và Review

#### Mục tiêu
Tổng hợp kiến thức tuần 1 thành một ứng dụng mini có luồng sử dụng hoàn chỉnh.

#### Công việc thực hiện

1. Hoàn thiện luồng Login -> List API -> Detail -> Save Local.
2. Rà soát loading, empty state, error state và retry ở các màn hình.
3. Dọn dẹp import, tên biến, file dư thừa và code lặp.
4. Chạy `dart format .`, `flutter analyze` và kiểm tra build debug.
5. Cập nhật README ngắn gồm cách chạy project và API đang sử dụng.
6. Tạo Pull Request hoàn chỉnh.
7. Chuẩn bị bản tự đánh giá và các câu hỏi cần trao đổi trong buổi 1-on-1.

#### Sản phẩm bàn giao

- Link Pull Request hoàn chỉnh.
- Video demo luồng chính.
- README cập nhật.
- Bản tự đánh giá tuần 1.

#### Checklist cuối tuần

- [ ] Luồng chính chạy được từ đầu đến cuối.
- [ ] Có xử lý lỗi mạng và dữ liệu rỗng.
- [ ] Không có lỗi `flutter analyze`.
- [ ] Code đã được format.
- [ ] PR có mô tả, ảnh/video và hướng dẫn kiểm thử.
- [ ] Đã ghi nhận feedback sau buổi review.

## Kết quả kỳ vọng

Một ứng dụng mini E-commerce có thể đăng nhập, tải danh sách sản phẩm, xem chi tiết và lưu dữ liệu cục bộ; code được tổ chức sạch, có thể mở rộng cho các tuần tiếp theo.
