# Đánh giá Fresher Flutter

## Fresher score: 7/10

Đây là một project phù hợp với mức fresher đang học Flutter. Bạn đã làm được một flow khá đầy đủ: đăng nhập bằng API, lưu token, lấy danh sách sản phẩm, xem chi tiết, tìm kiếm và đổi theme.

Điểm đáng ghi nhận là project không chỉ có giao diện tĩnh. Bạn đã biết kết nối API, parse JSON, quản lý trạng thái loading/error và chia code thành nhiều phần. Những điểm cần cải thiện chủ yếu nằm ở việc hoàn thiện flow, xử lý lifecycle và giữ cách tổ chức code nhất quán hơn.

## Chấm điểm theo từng phần

| Hạng mục | Điểm | Nhận xét |
| --- | ---: | --- |
| Flutter UI | 7.5/10 | Có login, banner, grid sản phẩm, search và detail page. |
| Gọi API | 7/10 | Đã dùng `Dio`, có service riêng và model JSON. Cách xử lý lỗi chưa thống nhất. |
| Quản lý state | 7/10 | Biết dùng `Provider` và `UiState`, nhưng detail page chưa đi cùng pattern với home page. |
| Tổ chức code | 6.5/10 | Đã biết chia thư mục, nhưng dependency còn tạo trực tiếp và một số trách nhiệm đang bị trộn. |
| Clean code | 6.5/10 | Tên và cấu trúc cơ bản dễ đọc, nhưng còn code thừa, code comment cũ và một số đoạn chưa nhất quán. |
| Lifecycle và UX | 6/10 | Có `mounted` check tốt, nhưng còn thiếu dispose controller, validation và image error state. |
| Hoàn thiện ứng dụng | 6.5/10 | Có flow chính, nhưng startup flow và trạng thái đăng nhập chưa hoàn chỉnh. |

## Những điểm làm tốt

- Biết chia code thành `models`, `services`, `providers`, `pages` và `widgets`.
- Dùng `Provider` thay vì để toàn bộ logic trong widget.
- Dùng `Dio` để gọi API và có `DioClient` dùng chung.
- Dùng `json_serializable` để parse model.
- Có enum `UiState` cho `initial`, `loading`, `success`, `error`, `empty`.
- Có xử lý loading và error cho danh sách sản phẩm và trang chi tiết.
- Có kiểm tra `mounted` trong các đoạn xử lý bất đồng bộ.
- Có dispose `TextEditingController` trong form login.

## Feedback chi tiết

### 1. Startup flow chưa hoàn chỉnh

**File:** `lib/main.dart:9-19`

Code hiện tại:

```dart
void main() async {
  runApp(
    MultiProvider(
      providers: [
        ChangeNotifierProvider(create: (_) => ProductProvider()),
        ChangeNotifierProvider(create: (_) => ThemeProvider()),
        ChangeNotifierProvider(create: (_) => AuthProvider()),
      ],
      child: const MyApp(),
    ),
  );
}
```

`main()` đang để `async` nhưng chưa `await` gì. Quan trọng hơn, `ThemeProvider.loadTheme()` và `AuthProvider.loadToken()` đã được viết nhưng chưa gọi lúc app khởi động.

Ở bước tiếp theo, nên có startup flow để:

- Khởi tạo Flutter binding nếu cần chạy init bất đồng bộ.
- Load theme đã lưu.
- Load token đã lưu.
- Quyết định mở `LoginPage` hay `HomePage`.

### 2. Token đã lưu nhưng chưa được sử dụng để mở màn hình phù hợp

**File:** `lib/main.dart:29-40`

Code hiện tại:

```dart
return MaterialApp(
  debugShowCheckedModeBanner: false,
  title: 'Shop App',
  theme: ThemeData(
    primarySwatch: Colors.blue,
    fontFamily: 'Roboto',
    scaffoldBackgroundColor: Colors.white,
  ),
  darkTheme: ThemeData.dark(),
  themeMode: themeProvider.themeMode,
  home: const LoginPage(),
);
```

`home` luôn là `LoginPage`, vì vậy dù token đã được lưu thì người dùng vẫn phải đăng nhập lại mỗi lần mở app.

Nên tạo một trạng thái khởi động đơn giản, hoặc dùng token đã load để chọn màn hình đầu tiên. Chưa cần thêm package routing ở giai đoạn này.

### 3. Cách quản lý dữ liệu giữa các màn hình chưa thống nhất

**File:** `lib/pages/home_page.dart:25-26`

Ở home page, dữ liệu đi qua `ProductProvider`:

```dart
WidgetsBinding.instance.addPostFrameCallback((_) {
  context.read<ProductProvider>().getProducts();
});
```

**File:** `lib/pages/detail_page.dart:15-18`

Nhưng detail page lại tự tạo service và tự quản lý state:

```dart
class _DetailPageState extends State<DetailPage> {
  ProductService api = ProductService();
  UiState _state = UiState.initial;
  Product? _product;
```

Hai cách này đều có thể chạy, nhưng trong cùng một project nên thống nhất. Với project hiện tại, có thể đưa logic detail về `ProductProvider` trước; chưa cần xây dựng clean architecture quá lớn.

### 4. Provider tự tạo dependency bên trong

**File:** `lib/providers/auth_provider.dart:8-9`

```dart
final LocalStorageService storage = LocalStorageService();
final AuthService authApi = AuthService();
```

**File:** `lib/providers/product_provider.dart:7`

```dart
final ProductService api = ProductService();
```

Ở mức fresher và project nhỏ, cách này vẫn chấp nhận được. Tuy nhiên provider đang bị phụ thuộc cứng vào service cụ thể, nên sau này khó mock hoặc thay service khi test.

Khi project lớn hơn, có thể truyền dependency qua constructor:

```dart
class ProductProvider extends ChangeNotifier {
  ProductProvider({required ProductService productService})
      : _productService = productService;

  final ProductService _productService;
}
```

Chưa cần làm ngay nếu mục tiêu hiện tại chỉ là hoàn thành bài CRUD.

### 5. Form login chưa validate dữ liệu

**File:** `lib/widgets/auth/login_form.dart:27-32`

```dart
Future<void> onLoginPressed() async {
  final username = usernameController.text;
  final password = passwordController.text;

  try {
    await context.read<AuthProvider>().login(username, password);
```

Hiện tại username/password được lấy trực tiếp và gọi API ngay. Nên kiểm tra tối thiểu:

- Username không được rỗng.
- Password không được rỗng.
- Không gọi API nếu dữ liệu chưa hợp lệ.
- Hiển thị lỗi rõ ràng cho người dùng.

### 6. Context sau khi chuyển màn hình

**File:** `lib/widgets/auth/login_form.dart:36-45`

```dart
Navigator.pushReplacement(
  context,
  MaterialPageRoute(builder: (_) => const HomePage()),
);
ScaffoldMessenger.of(context).showSnackBar(
  const SnackBar(
    content: Text('Login successful!'),
    backgroundColor: Colors.green,
  ),
);
```

Sau `pushReplacement`, context của màn login có thể không còn phù hợp. Nếu cần hiện thông báo thành công, nên hiện trước khi chuyển màn hoặc dùng một `ScaffoldMessenger` ở cấp app.

### 7. `PageController` chưa được dispose

**File:** `lib/widgets/banner_slider.dart:12-14`

```dart
class _BannerSliderState extends State<BannerSlider> {
  final PageController _controller = PageController();
  int _currentIndex = 0;
```

Controller cần được giải phóng trong `dispose()`:

```dart
@override
void dispose() {
  _controller.dispose();
  super.dispose();
}
```

Đây là lỗi lifecycle nhỏ nhưng nên tạo thói quen xử lý từ sớm.

### 8. Có `setState` rỗng trong product card

**File:** `lib/widgets/product_card.dart:22-31`

```dart
Future<void> _openDetail() async {
  await Navigator.push(
    context,
    MaterialPageRoute(
      builder: (_) => DetailPage(productId: widget.product.id),
    ),
  );

  setState(() {});
}
```

Sau khi quay lại, không có dữ liệu nào được cập nhật trong card. Nếu chưa có favorite hoặc state cần refresh thì đoạn `setState(() {})` này nên xóa.

### 9. Service có `try/catch` không cần thiết

**File:** `lib/services/product_service.dart:7-14`

```dart
Future<ProductResponse> getProducts() async {
  try {
    final response = await DioClient.dio.get('/products');
    return ProductResponse.fromJson(response.data);
  } catch (e) {
    rethrow;
  }
}
```


```dart
Future<ProductResponse> getProducts() async {
  final response = await DioClient.dio.get('/products');
  return ProductResponse.fromJson(response.data);
}
```

Hoặc giữ `catch` nếu muốn chuyển lỗi Dio thành một loại lỗi/message rõ ràng hơn cho UI.

### 10. Model đang chứa cả API data và UI state

**File:** `lib/models/product.dart:6-14`

```dart
class Product {
  final int id;
  final String title;
  final String thumbnail;
  final double price;
  final String description;

  bool isFavorite;
  int likeCount;
```

Các field `isFavorite` và `likeCount` là state của UI/local feature, không phải dữ liệu sản phẩm hiện tại từ API.

Nếu sau này làm favorite thật, nên quản lý phần này ở provider hoặc một state riêng. Với bài hiện tại có thể giữ nguyên nếu đây là phần chuẩn bị cho feature tiếp theo.

### 11. Ảnh network chưa có trạng thái dự phòng

**File:** `lib/widgets/product_card.dart:59-63`

```dart
Image.network(
  product.thumbnail,
  fit: BoxFit.cover,
  width: double.infinity,
),
```

Khi mất mạng hoặc URL ảnh lỗi, UI sẽ không có fallback rõ ràng. Có thể thêm `errorBuilder` đơn giản:

```dart
Image.network(
  product.thumbnail,
  fit: BoxFit.cover,
  width: double.infinity,
  errorBuilder: (_, __, ___) => const Icon(Icons.image_not_supported),
),
```

### 12. Token nên được bảo vệ tốt hơn khi làm app thật

**File:** `lib/services/storage/local_storage_service.dart:8-23`

```dart
Future<void> saveAccessToken(String token) async {
  final prefs = await SharedPreferences.getInstance();
  await prefs.setString(accessTokenKey, token);
}

Future<String?> getAccessToken() async {
  final prefs = await SharedPreferences.getInstance();
  return prefs.getString(accessTokenKey);
}
```

Với bài học thì cách này giúp hiểu persistence và có thể tạm chấp nhận. Nếu làm production, nên chuyển token sang secure storage vì `SharedPreferences` không phù hợp để bảo vệ thông tin nhạy cảm.

### 13. Android cần quyền truy cập internet

**File:** `android/app/src/main/AndroidManifest.xml`

Manifest hiện tại bắt đầu bằng:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android">
```

App có gọi API và tải ảnh từ internet, nên cần khai báo:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

Nên đặt dòng này ngay bên dưới thẻ `<manifest>`.

## Clean Code Score: 6.5/10

Ở mức fresher, code đã có sự phân chia file và tên class tương đối dễ hiểu. Các phần như `AuthProvider`, `ProductProvider`, `AuthService` và `ProductService` giúp người đọc biết mỗi phần đang chịu trách nhiệm gì.

Điểm clean code chưa cao vì:

- Một số dependency được khởi tạo trực tiếp bên trong provider.
- `DetailPage` gọi service trực tiếp trong khi `HomePage` đi qua provider.
- Các đoạn `try/catch { rethrow; }` không thêm xử lý gì.
- `Product` đang chứa cả dữ liệu từ API và state của UI.
- `fake_data.dart` còn nhiều code cũ đã comment.
- `ProductCard` có `setState(() {})` rỗng.
- Một số widget chưa dùng `const` ở những chỗ phù hợp.
- Một số tên như `api` còn khá chung chung; có thể đặt rõ hơn như `productService` hoặc `authService`.

Chưa cần xây dựng architecture quá phức tạp. Trước mắt chỉ cần giữ mỗi class một trách nhiệm rõ ràng, thống nhất cách đi qua provider/service, xóa code thừa và đặt tên cụ thể hơn.

## Thứ tự nên cải thiện

1. Hoàn thiện startup flow: load theme/token và quyết định mở `LoginPage` hay `HomePage`.
2. Thống nhất cách quản lý dữ liệu: đưa `DetailPage` về dùng `ProductProvider`.
3. Thêm validation cho form login.
4. Sửa lifecycle: dispose `PageController` và xóa `setState(() {})` rỗng.
5. Bỏ các đoạn `try/catch { rethrow; }` không cần thiết.
6. Thêm loading/error state cho ảnh network.
7. Xóa code cũ đang comment trong `fake_data.dart`.
8. Đặt tên dependency rõ hơn.
9. Khi project lớn hơn, tách UI state khỏi API model và chuyển token sang secure storage.

## Kết luận

Đã nắm được các kỹ năng cơ bản quan trọng của Flutter: dựng UI, gọi API, parse JSON, quản lý state và điều hướng.

Điểm cần tập trung tiếp theo không phải là thêm nhiều package, mà là làm chắc các phần cơ bản: startup flow, validation, lifecycle, error handling và cách tổ chức state nhất quán.
