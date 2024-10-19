Để hiển thị thông báo trong ứng dụng Android, bạn có thể sử dụng nhiều kiểu thông báo khác nhau ngoài `Toast`. Dưới đây là một số lựa chọn phổ biến:

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
