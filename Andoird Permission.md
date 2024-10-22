Trong Android, **permissions** (quyền) là một phần quan trọng để quản lý quyền truy cập của ứng dụng đến các tài nguyên nhạy cảm của thiết bị như camera, microphone, vị trí, và lưu trữ. Kể từ Android 6.0 (API level 23), Android đã chuyển sang mô hình cấp quyền theo thời gian chạy (runtime permissions), yêu cầu người dùng cho phép quyền truy cập khi ứng dụng đang hoạt động, thay vì chỉ khi cài đặt ứng dụng.

### Các loại Permission

1. **Normal Permissions**:
   - Những quyền này không ảnh hưởng đến dữ liệu của người dùng hoặc hoạt động của các ứng dụng khác.
   - Hệ thống tự động cấp phép mà không cần yêu cầu người dùng.
   - Ví dụ: `ACCESS_NETWORK_STATE`, `INTERNET`.

2. **Dangerous Permissions**:
   - Những quyền này có thể ảnh hưởng đến quyền riêng tư hoặc bảo mật của người dùng.
   - Cần phải yêu cầu và nhận được sự chấp thuận của người dùng.
   - Ví dụ: `READ_CONTACTS`, `ACCESS_FINE_LOCATION`, `CAMERA`.

### Cách sử dụng Permissions trong Android

#### 1. Khai báo Permissions trong AndroidManifest.xml

Tất cả các quyền mà ứng dụng yêu cầu cần được khai báo trong tệp `AndroidManifest.xml`:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapp">

    <uses-permission android:name="android.permission.CAMERA"/>
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
    <uses-permission android:name="android.permission.READ_CONTACTS"/>

    <application
        ...
    </application>
</manifest>
```

#### 2. Kiểm tra và yêu cầu Permissions

Trước khi thực hiện một hành động yêu cầu quyền (như mở camera hoặc truy cập vị trí), bạn nên kiểm tra xem quyền đó đã được cấp chưa. Nếu chưa, bạn cần yêu cầu người dùng cấp quyền.

##### Kiểm tra Permissions

Sử dụng `ContextCompat` để kiểm tra xem quyền có được cấp hay không:

```java
if (ContextCompat.checkSelfPermission(this, Manifest.permission.CAMERA) != PackageManager.PERMISSION_GRANTED) {
    // Quyền chưa được cấp
    requestCameraPermission();
}
```

##### Yêu cầu Permissions

Sử dụng `ActivityCompat` để yêu cầu quyền:

```java
private void requestCameraPermission() {
    ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.CAMERA}, REQUEST_CAMERA_PERMISSION);
}
```

##### Xử lý kết quả yêu cầu Permissions

Khi người dùng đã cho phép hoặc từ chối quyền, bạn cần xử lý kết quả trong phương thức `onRequestPermissionsResult`:

```java
@Override
public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
    super.onRequestPermissionsResult(requestCode, permissions, grantResults);
    switch (requestCode) {
        case REQUEST_CAMERA_PERMISSION:
            if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                // Quyền đã được cấp, thực hiện hành động liên quan đến camera
            } else {
                // Quyền đã bị từ chối
                Toast.makeText(this, "Camera permission denied", Toast.LENGTH_SHORT).show();
            }
            break;
    }
}
```

### 3. Yêu cầu Permissions trong Android 11 và cao hơn

Kể từ Android 11 (API level 30), một số quyền như `ACCESS_BACKGROUND_LOCATION` yêu cầu thêm quy trình yêu cầu quyền cho vị trí ở chế độ nền.

1. **Yêu cầu vị trí**: Nếu bạn yêu cầu quyền truy cập vị trí ở chế độ nền, bạn cũng cần phải yêu cầu quyền truy cập vị trí ở chế độ bình thường trước.
  
2. **Chạy thời gian yêu cầu quyền**: Bạn có thể yêu cầu quyền ở chế độ nền bằng cách sử dụng cách tiếp cận tương tự.

### Tài liệu tham khảo

- [Hướng dẫn về Permissions trên Android](https://developer.android.com/guide/topics/permissions/overview)
- [Hướng dẫn về Permissions theo thời gian chạy](https://developer.android.com/training/permissions/requesting)

### Kết luận

Quyền trong Android là một phần quan trọng trong việc phát triển ứng dụng. Việc quản lý và yêu cầu quyền một cách hợp lý giúp bảo vệ quyền riêng tư của người dùng và đảm bảo rằng ứng dụng hoạt động đúng cách. Hãy luôn nhớ kiểm tra và yêu cầu quyền trước khi truy cập vào các tài nguyên nhạy cảm.
