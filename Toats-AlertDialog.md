# **TOAST**
## Để hiển thị thông báo trong ứng dụng Android, `Toast`.
`Toast` trong Android là một thành phần hiển thị thông báo tạm thời cho người dùng. Dưới đây là các kiểu và tùy chọn mà bạn có thể sử dụng với `Toast`:

### 1. **Cách Tạo Toast Cơ Bản**

```java
Toast.makeText(context, "Nội dung thông báo", Toast.LENGTH_SHORT).show();
```

- **context**: Ngữ cảnh của ứng dụng (thường là `getApplicationContext()` hoặc `this`).
- **Nội dung thông báo**: Văn bản bạn muốn hiển thị.
- **Độ dài**: `Toast.LENGTH_SHORT` hoặc `Toast.LENGTH_LONG`.

### 2. **Độ Dài Hiển Thị**

- `Toast.LENGTH_SHORT`: Hiển thị trong khoảng 2 giây.
- `Toast.LENGTH_LONG`: Hiển thị trong khoảng 3.5 giây.

### 3. **Tùy Chỉnh Vị Trí**

Mặc định, `Toast` sẽ hiển thị ở giữa màn hình. Bạn có thể thay đổi vị trí của nó bằng cách sử dụng `setGravity()`:

```java
Toast toast = Toast.makeText(context, "Nội dung thông báo", Toast.LENGTH_SHORT);
toast.setGravity(Gravity.TOP | Gravity.CENTER_HORIZONTAL, 0, 0); // Vị trí trên cùng, giữa
toast.show();
```

**Các giá trị cho `setGravity()`:**

- `Gravity.TOP`: Hiển thị ở trên cùng.
- `Gravity.BOTTOM`: Hiển thị ở dưới cùng.
- `Gravity.CENTER`: Hiển thị ở giữa.
- `Gravity.LEFT`: Hiển thị ở bên trái.
- `Gravity.RIGHT`: Hiển thị ở bên phải.
- `Gravity.CENTER_HORIZONTAL`: Hiển thị ở giữa theo chiều ngang.
- `Gravity.CENTER_VERTICAL`: Hiển thị ở giữa theo chiều dọc.

### 4. **Tùy Chỉnh Hình Dáng và Nội Dung**

Bạn có thể tạo một `Toast` tùy chỉnh bằng cách sử dụng layout XML. Dưới đây là cách làm:

**Bước 1: Tạo Layout XML (custom_toast.xml)**

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:background="@drawable/toast_background"
    android:padding="8dp"
    android:orientation="horizontal">

    <ImageView
        android:id="@+id/image"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/ic_success" />

    <TextView
        android:id="@+id/text"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:paddingStart="8dp"
        android:textColor="@android:color/white" />
</LinearLayout>
```

**Bước 2: Tạo Toast Tùy Chỉnh**

```java
LayoutInflater inflater = getLayoutInflater();
View customView = inflater.inflate(R.layout.custom_toast, null);

TextView text = customView.findViewById(R.id.text);
text.setText("Thông báo tùy chỉnh!");

Toast customToast = new Toast(getApplicationContext());
customToast.setGravity(Gravity.BOTTOM, 0, 100);
customToast.setDuration(Toast.LENGTH_LONG);
customToast.setView(customView);
customToast.show();
```

### 5. **Một Số Lưu Ý**

- **Tránh hiển thị quá nhiều `Toast` cùng lúc**: Điều này có thể làm phiền người dùng. Hãy chỉ hiển thị `Toast` khi cần thiết.
- **Không nên sử dụng `Toast` cho thông báo quan trọng**: Nếu bạn cần hiển thị thông báo mà người dùng phải chú ý (ví dụ: lỗi quan trọng), hãy sử dụng `Dialog` hoặc `Snackbar` thay vì `Toast`.

### Kết Luận

`Toast` là một cách đơn giản và nhanh chóng để thông báo cho người dùng về các sự kiện trong ứng dụng. Tùy thuộc vào nhu cầu của bạn, bạn có thể tùy chỉnh nó để phù hợp với trải nghiệm người dùng của ứng dụng của bạn.

## Các Kiểu Toats Khác
### 1. **Snackbar**
`Snackbar` là một thành phần UI hiển thị thông báo ở đáy màn hình, thường được sử dụng cho các hành động tạm thời. Nó có thể bao gồm một hành động (như "Hoàn tác").

```java
// Hiển thị Snackbar
Snackbar.make(view, "Đã lưu thành công!", Snackbar.LENGTH_SHORT)
    .setAction("Hoàn tác", v -> {
        // Thực hiện hành động hoàn tác nếu cần
    })
    .show();
```

### 2. **Dialog**
Sử dụng một `Dialog` để hiển thị thông báo. Dialog có thể chứa nhiều thông tin hơn và có thể bao gồm nhiều nút tùy chọn.

```java
new AlertDialog.Builder(getContext())
    .setTitle("Thông báo")
    .setMessage("Đã lưu thành công!")
    .setPositiveButton("OK", null)
    .show();
```

### 3. **ProgressDialog**
`ProgressDialog` được sử dụng để hiển thị trạng thái tiến trình trong một tác vụ dài. Tuy nhiên, lưu ý rằng `ProgressDialog` đã bị khuyến cáo không sử dụng vì nó có thể gây mất trải nghiệm người dùng. Thay vào đó, bạn nên sử dụng `ProgressBar` trong UI.

```java
ProgressDialog progressDialog = new ProgressDialog(getContext());
progressDialog.setMessage("Đang lưu...");
progressDialog.setCancelable(false);
progressDialog.show();

// Sau khi hoàn thành lưu
progressDialog.dismiss();
```

### 4. **Custom Toast**
Bạn cũng có thể tạo một `Toast` tùy chỉnh bằng cách sử dụng một layout riêng cho nó.

```java
LayoutInflater inflater = getLayoutInflater();
View layout = inflater.inflate(R.layout.custom_toast, (ViewGroup) getActivity().findViewById(R.id.custom_toast_container));

TextView text = layout.findViewById(R.id.text);
text.setText("Đã lưu thành công!");

Toast toast = new Toast(getContext());
toast.setDuration(Toast.LENGTH_SHORT);
toast.setView(layout);
toast.show();
```

### 5. **Notification**
Nếu bạn muốn gửi thông báo cho người dùng ngay cả khi ứng dụng không hoạt động, bạn có thể sử dụng `Notification`.

```java
NotificationManager notificationManager = (NotificationManager) getContext().getSystemService(Context.NOTIFICATION_SERVICE);
String channelId = "my_channel_id"; // Đảm bảo bạn đã tạo channel này trong Android 8.0 trở lên

NotificationCompat.Builder builder = new NotificationCompat.Builder(getContext(), channelId)
        .setSmallIcon(R.drawable.ic_notification) // Biểu tượng thông báo
        .setContentTitle("Thông báo")
        .setContentText("Đã lưu thành công!")
        .setPriority(NotificationCompat.PRIORITY_DEFAULT);

notificationManager.notify(1, builder.build());
```

### Kết Luận
Tùy vào ngữ cảnh và cách bạn muốn người dùng tương tác, bạn có thể chọn một trong những phương pháp trên để thông báo cho người dùng. Nếu bạn cần một thông báo tạm thời, hãy sử dụng `Snackbar` hoặc `Toast`. Nếu bạn cần một thông báo cần sự chú ý hơn, hãy sử dụng `Dialog` hoặc `Notification`.

# Trong Android, `AlertDialog` hiển thị thông tin và tương tác với người dùng.

### 1. **AlertDialog Cơ Bản**

Đây là kiểu đơn giản nhất, thường chỉ bao gồm tiêu đề, thông điệp và các nút xác nhận.

```java
new AlertDialog.Builder(context)
    .setTitle("Tiêu đề")
    .setMessage("Thông điệp")
    .setPositiveButton("OK", (dialog, which) -> {
        // Xử lý khi nhấn OK
    })
    .setNegativeButton("Hủy", null)
    .show();
```

### 2. **AlertDialog với Danh Sách (List)**

Được sử dụng khi bạn muốn hiển thị một danh sách các tùy chọn cho người dùng chọn.

```java
String[] items = {"Item 1", "Item 2", "Item 3"};
new AlertDialog.Builder(context)
    .setTitle("Chọn một mục")
    .setItems(items, (dialog, which) -> {
        // Xử lý khi chọn một mục
    })
    .show();
```

### 3. **AlertDialog với Danh Sách Có Checkbox (Multi-choice List)**

Sử dụng khi bạn muốn người dùng có thể chọn nhiều tùy chọn từ danh sách.

```java
String[] items = {"Item 1", "Item 2", "Item 3"};
boolean[] checkedItems = {false, false, false}; // Trạng thái đã chọn
new AlertDialog.Builder(context)
    .setTitle("Chọn các mục")
    .setMultiChoiceItems(items, checkedItems, (dialog, which, isChecked) -> {
        // Xử lý khi chọn hoặc bỏ chọn
    })
    .setPositiveButton("OK", (dialog, which) -> {
        // Xử lý khi nhấn OK
    })
    .show();
```

### 4. **AlertDialog với View Tùy Chỉnh**

Bạn có thể tạo một `AlertDialog` với một layout tùy chỉnh, cho phép bạn thêm các thành phần như `EditText`, `CheckBox`, v.v.

```java
LayoutInflater inflater = getLayoutInflater();
View dialogView = inflater.inflate(R.layout.custom_dialog, null);
new AlertDialog.Builder(context)
    .setView(dialogView)
    .setTitle("Tiêu đề")
    .setPositiveButton("OK", (dialog, which) -> {
        // Xử lý khi nhấn OK
        EditText editText = dialogView.findViewById(R.id.edit_text);
        String inputText = editText.getText().toString();
        // Xử lý inputText
    })
    .setNegativeButton("Hủy", null)
    .show();
```

### 5. **AlertDialog với Icon**

Bạn có thể thêm biểu tượng cho `AlertDialog` để làm nổi bật hơn.

```java
new AlertDialog.Builder(context)
    .setTitle("Tiêu đề")
    .setMessage("Thông điệp")
    .setIcon(android.R.drawable.ic_dialog_alert)
    .setPositiveButton("OK", null)
    .show();
```

### 6. **AlertDialog với Progress Bar**

Bạn có thể hiển thị một hộp thoại với một thanh tiến trình để thể hiện rằng một quá trình nào đó đang diễn ra.

```java
AlertDialog progressDialog = new AlertDialog.Builder(context)
    .setTitle("Đang tải...")
    .setView(new ProgressBar(context))
    .setCancelable(false) // Không cho phép đóng hộp thoại
    .show();
```

### Kết Luận

`AlertDialog` là một công cụ mạnh mẽ để tương tác với người dùng trong ứng dụng Android. Bạn có thể kết hợp các kiểu này để tạo ra trải nghiệm người dùng tốt hơn, dựa trên nhu cầu cụ thể của ứng dụng của bạn.
