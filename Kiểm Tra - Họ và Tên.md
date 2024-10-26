Để kiểm tra họ tên của người dùng, bạn có thể áp dụng một số quy tắc thông thường để đảm bảo họ tên nhập vào có định dạng hợp lệ. Các quy tắc phổ biến cho họ tên thường là:

1. Không chứa số hoặc ký tự đặc biệt.
2. Viết hoa chữ cái đầu (không bắt buộc nhưng có thể áp dụng).
3. Dài từ 2 ký tự trở lên (phòng trường hợp họ tên rất ngắn).

Dưới đây là một mẫu regex cơ bản để kiểm tra họ tên người dùng:

```java
String nameRegex = "^[\\p{L} .'-]{2,50}$";
```

### Giải thích mẫu regex:
- **`\\p{L}`**: Ký tự bất kỳ thuộc về bảng chữ cái Unicode (có hỗ trợ cho các ký tự tiếng Việt hoặc các ngôn ngữ khác).
- **`[ .'-]`**: Cho phép các ký tự dấu cách, dấu chấm, dấu gạch ngang, hoặc dấu nháy đơn.
- **`{2,50}`**: Đảm bảo họ tên có độ dài từ 2 đến 50 ký tự.

### Sử dụng regex để kiểm tra họ tên:

```java
String name = "Nguyễn Văn A"; // Họ tên cần kiểm tra

if (name.matches(nameRegex)) {
    Log.d("NameValidation", "Họ tên hợp lệ.");
} else {
    Log.d("NameValidation", "Họ tên không hợp lệ.");
}
```

Với mẫu này, bạn có thể kiểm tra xem tên người dùng có đúng định dạng hay không. Bạn cũng có thể tùy chỉnh regex để phù hợp với yêu cầu cụ thể hơn nếu cần.
