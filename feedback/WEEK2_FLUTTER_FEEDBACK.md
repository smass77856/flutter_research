# Đánh giá Fresher Flutter - Tuần 2

## Fresher score: 8/10

So với tuần 1 (7/10), bạn đã có bước tiến rõ rệt. Codebase đã được tổ chức lại theo Clean Architecture với các tầng `data`, `domain`, `presentation`, dependency được quản lý tập trung, token chuyển sang `SecureStorage`, và có interceptor xử lý refresh token. Startup flow đã hoàn chỉnh với `SplashScreen`, form login có validation, `DetailPage` đã được thống nhất dùng `ProductProvider`. Code thừa từ tuần 1 đã được xóa sạch, structure gọn gàng hơn hẳn.

Điểm cần cải thiện chủ yếu nằm ở `SignUpForm` chưa gọi API thật, `ThemeProvider` vẫn tự tạo dependency, và `MainScreen` còn placeholder.

## Chấm điểm theo từng phần

| Hạng mục            | Điểm tuần 1 | Điểm tuần 2 | Nhận xét                                                                                                 |
| ------------------- | ----------: | ----------: | -------------------------------------------------------------------------------------------------------- |
| Flutter UI          |         7.5 |         8.5 | Thêm SplashScreen, MainScreen bottom nav, SignUp, Verify. UI sạch hơn, dùng `SafeArea` nhất quán.        |
| Gọi API             |           7 |         8.5 | Clean architecture với DataSource → Repository → UseCase. Có interceptor refresh token.                  |
| Quản lý state       |           7 |         8.5 | DetailPage thống nhất dùng ProductProvider. Có favorite state. Dependency inject qua constructor.        |
| Tổ chức code        |         6.5 |           9 | Clean architecture đúng chuẩn: `data/domain/presentation`. `core/` cho infrastructure. Code thừa đã xóa. |
| Clean code          |         6.5 |           8 | Tên class rõ ràng, model tách entity, không còn code comment thừa. Ít section styling lặp lại.           |
| Lifecycle và UX     |           6 |           8 | Form validate, mounted check, PageController dispose, loading/error/empty states.                        |
| Hoàn thiện ứng dụng |         6.5 |           8 | Splash → Login/Home flow hoàn chỉnh. Favorite feature hoạt động. Bottom nav scaffold.                    |

## Những điểm cải thiện đáng kể so với tuần 1

- **Clean Architecture**: Tái cấu trúc toàn bộ theo `data/domain/presentation` với repository pattern, use case, abstract repository interface.
- **Dependency Injection**: Tạo `Injection` class tập trung, dependency inject qua constructor thay vì tạo trực tiếp trong provider.
- **Token → SecureStorage**: Chuyển token từ `SharedPreferences` sang `flutter_secure_storage`.
- **Auth Interceptor**: Viết `QueuedInterceptor` xử lý tự động attach token và refresh khi 401, có retry logic.
- **SplashScreen**: Kiểm tra token khi khởi động, điều hướng tới `LoginScreen` hoặc `MainScreen`.
- **Form Validation**: Login form dùng `GlobalKey<FormState>` + `validator` cho cả username và password.
- **DetailPage thống nhất**: Không còn tự tạo service riêng, dùng `ProductProvider` giống `HomeScreen`.
- **Product entity sạch**: Bỏ `isFavorite` và `likeCount` khỏi entity, quản lý favorite riêng trong provider.
- **Image error handling**: Cả `ProductCard` và `DetailPage` đều có `errorBuilder` và `loadingBuilder`.
- **PageController dispose**: `BannerSlider` đã `dispose()` controller đúng cách.
- **Code thừa xóa sạch**: Không còn `fake_data.dart`, không còn `pages/`, `services/`, `widgets/`, `models/` cũ.

## Feedback chi tiết

### 1. `SignUpForm` gọi `login()` thay vì `signUp()`

**File:** `lib/features/auth/presentation/widgets/sign_up_form.dart:33`

```dart
await context.read<AuthProvider>().login(username, password);
```

Form signUp đang gọi `login()` thay vì API đăng ký. `AuthRepository` cũng chưa có method `signUp`. Nếu đây là placeholder thì nên comment rõ; nếu là feature thật thì cần thêm use case và API endpoint tương ứng.

### 2. `SignUpForm` không dispose `confirmPasswordController`

**File:** `lib/features/auth/presentation/widgets/sign_up_form.dart:60-63`

```dart
@override
void dispose() {
  usernameController.dispose();
  passwordController.dispose();
  super.dispose();
}
```

`confirmPasswordController` được tạo ở dòng 20 nhưng không được dispose. Đây là leak nhỏ nhưng nên sửa.

### 3. `SignUpForm` xác nhận password nhưng không validate khớp nhau

**File:** `lib/features/auth/presentation/widgets/sign_up_form.dart:185-190`

```dart
validator: (value) {
  if (value == null || value.isEmpty) {
    return 'Please enter confirm password';
  }
  return null;
},
```

Validator chỉ kiểm tra rỗng, không kiểm tra `confirmPassword == password`.

### 4. SnackBar sau `pushReplacement` có thể crash

**File:** `lib/features/auth/presentation/widgets/login_form.dart:35-44`

```dart
Navigator.pushReplacement(
  context,
  MaterialPageRoute(builder: (_) => const HomeScreen()),
);
ScaffoldMessenger.of(context).showSnackBar(
  const SnackBar(
    content: Text('Login successful!'),
    backgroundColor: Colors.green,
  ),
);
```

Sau `pushReplacement`, context cũ có thể đã bị unmount. SnackBar nên được show trước khi push, hoặc dùng `ScaffoldMessenger.of(context).clearSnackBars()` ở màn mới. Tuần 1 đã nhận xét vấn đề này nhưng chưa sửa.

### 5. `AuthProvider` có `rethrow` sau khi set error state

**File:** `lib/features/auth/presentation/providers/auth_provider.dart:47-50`

```dart
} catch (e) {
  _state = UiStateEnum.error;
  notifyListeners();
  rethrow;
}
```

`rethrow` sẽ gây throw ra ngoài widget tree. LoginForm đã catch được ở dòng 45, nhưng nếu có caller nào không catch thì sẽ crash app. Với pattern hiện tại (provider set error state + widget đọc state), có thể bỏ `rethrow` để thống nhất xử lý lỗi chỉ ở layer presentation.

### 6. `ThemeProvider` vẫn tự tạo `LocalStorageService`

**File:** `lib/providers/theme_provider.dart:6`

```dart
final LocalStorageService storage = LocalStorageService();
```

`AuthProvider` và `ProductProvider` đã chuyển sang inject dependency qua constructor, nhưng `ThemeProvider` vẫn tạo trực tiếp. Nên thống nhất bằng cách inject `storage` từ `main()`.

### 7. `ThemeProvider` chưa load theme lúc app khởi động

**File:** `lib/main.dart:15`

```dart
final themeProvider = ThemeProvider();
```

`loadTheme()` đã được viết trong `ThemeProvider` nhưng chưa được gọi ở `main()`. Theme sẽ luôn bắt đầu từ light mode dù người dùng đã toggle trước đó.

### 8. `DioClient` có thể tạo nhiều instance `SecureStorageService`

**File:** `lib/core/network/dio_client.dart:6`

```dart
static final SecureStorageService _storage = SecureStorageService();
```

`SecureStorageService` được tạo trong `DioClient`, trong `AuthProvider` (từ `main.dart:19`), và mỗi lần `Injection` chạy. Với `flutter_secure_storage` thì không nghiêm trọng, nhưng nên xem xét dùng singleton hoặc inject từ `Injection`.

### 9. `MainScreen` có placeholder screens

**File:** `lib/presentation/main_screen.dart:14-22`

```dart
final List<Widget> _screens = const [
  HomeScreen(),
  Center(child: Text('My Order')),
  Center(child: Text('Favorite')),
  Center(child: Text('Profile')),
];
```

3 tab Order, Favorite, Profile vẫn là placeholder. Đây là feature cần làm tiếp, nhưng Favorite đã có data (lưu trong provider + localStorage), nên tab Favorite nên hiển thị danh sách favorites thay vì text rỗng.

### 10. `BannerSlider` thiếu `errorBuilder`

**File:** `lib/features/product/presentation/widgets/banner_slider.dart:37-43`

```dart
child: Image.network(
  widget.images[index],
  fit: BoxFit.cover,
  width: double.infinity,
),
```

`ProductCard` và `DetailPage` đều đã thêm `errorBuilder`, nhưng `BannerSlider` chưa. Nên thống nhất.

### 11. `AuthInterceptor` có tiềm năng gây ra race condition

**File:** `lib/core/network/auth_interceptor.dart:88-100`

```dart
Future<bool> _refreshAccessToken() async {
  final currentRefresh = _refreshFuture;
  if (currentRefresh != null) {
    return currentRefresh;
  }
  _refreshFuture = _performRefresh();
  try {
    return await _refreshFuture!;
  } finally {
    _refreshFuture = null;
  }
}
```

Logic chống concurrent refresh đã đúng ý tưởng, nhưng `QueuedInterceptor` đã serialize requests nên thực tế ít khi có 2 request cùng lúc trigger refresh. Logic này vẫn tốt để phòng ngừa — không cần thay đổi, nhưng nên comment rõ tại sao cần nó.

### 12. `LoginScreen` / `SignUpScreen` dùng `push` tạo vòng lặp

**File:** `lib/features/auth/presentation/screen/login_screen.dart:58-62`

```dart
Navigator.push(
  context,
  MaterialPageRoute(
    builder: (context) => const SignUpScreen(),
  ),
);
```

Login → SignUp → Login → SignUp có thể tạo stack navigation không giới hạn. Nên dùng `pushReplacement` hoặc đẩy bớt stack.

### 13. SignUp screen styling thiếu `const`

**File:** `lib/features/auth/presentation/screen/sign_up_screen.dart:22-33`

```dart
Text(
  'Create Account',
  style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
),
```

So với `LoginScreen` dùng `const Text(...)`, `SignUpScreen` thiếu `const`. Nên thống nhất.

## So sánh với tuần 1

| Vấn đề tuần 1                              | Trạng thái tuần 2                                     |
| ------------------------------------------ | ----------------------------------------------------- |
| Startup flow chưa hoàn chỉnh               | Đã sửa — SplashScreen load token và điều hướng        |
| Token lưu nhưng chưa dùng để chọn màn hình | Đã sửa — SplashScreen check token, mở Login hoặc Home |
| DetailPage tự tạo service riêng            | Đã sửa — dùng ProductProvider thống nhất              |
| Provider tự tạo dependency                 | Đã sửa cho Auth/Product. ThemeProvider chưa sửa       |
| Form login chưa validate                   | Đã sửa — có Form + validator                          |
| Context sau pushReplacement                | Chưa sửa — vẫn show SnackBar sau pushReplacement      |
| PageController chưa dispose                | Đã sửa                                                |
| setState rỗng trong ProductCard            | Đã sửa — chuyển sang StatelessWidget                  |
| try/catch { rethrow } không cần thiết      | Đã sửa — DataSource không còn try/catch thừa          |
| Model chứa UI state                        | Đã sửa — Product entity chỉ chứa API data             |
| Ảnh network chưa có errorBuilder           | Đã sửa cho ProductCard và DetailPage                  |
| Token dùng SharedPreferences               | Đã sửa — chuyển sang SecureStorage                    |
| Android thiếu INTERNET permission          | Chưa kiểm tra được (cần xem AndroidManifest.xml)      |

## Clean Code Score: 8/10

Tuần 2 có cải thiện lớn về tổ chức code. Clean Architecture được áp dụng đúng cấu trúc `data → domain → presentation`. Repository pattern hoạt động tốt với abstract interface. Dependency inject qua constructor giúp code testable hơn. Tên class và method rõ ràng, responsibility mỗi lớp phân biệt rõ.

Những điểm còn thiếu:

- `SignUpForm` chưa hoạt động thật (gọi login thay vì signUp).
- `ThemeProvider` chưa đồng bộ pattern với các provider khác.
- SnackBar sau pushReplacement vẫn là vấn đề lifecycle.
- `MainScreen` còn placeholder.
- Vẫn không có test.

## Thứ tự nên cải thiện

1. Sửa `SignUpForm`: thêm signUp API hoặc rõ ràng là placeholder.
2. Sửa SnackBar sau `pushReplacement`: show trước khi push.
3. Inject dependency cho `ThemeProvider` và gọi `loadTheme()` ở startup.
4. Thêm validate `password == confirmPassword` trong sign up form.
5. Dispose `confirmPasswordController` trong sign up form.
6. Thêm `errorBuilder` cho `BannerSlider`.
7. Thêm `const` cho các Text widget trong `SignUpScreen`.
8. Bắt đầu viết test cho UseCase và Provider.

## Kết luận

Bước tiến lớn so với tuần 1. Clean Architecture, DI, interceptor, secure storage, startup flow, validation — tất cả đều cho thấy bạn đang đi đúng hướng. Codebase giờ có cấu trúc rõ ràng, dễ mở rộng. Phần còn lại là polish: sửa các edge case nhỏ, hoàn thiện signUp flow, và bắt đầu viết test.
