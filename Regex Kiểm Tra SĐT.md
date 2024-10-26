Để tùy chỉnh regex cho số điện thoại, bạn có thể tự định nghĩa các regex tùy thuộc vào yêu cầu, chẳng hạn như độ dài số hoặc mã vùng cụ thể. Dưới đây là một số mẫu regex phổ biến có thể được điều chỉnh cho các định dạng số điện thoại khác nhau:

1. **Regex kiểm tra số điện thoại chuẩn 10 chữ số (Việt Nam)**:
   ```java
   String vietnamPhoneRegex = "^(0?)(3[2-9]|5[6|8|9]|7[0|6-9]|8[1-5]|9[0-9])[0-9]{7}$";
   ```
   - Mẫu này kiểm tra các số điện thoại di động phổ biến tại Việt Nam. Số điện thoại bắt đầu bằng "0" hoặc mã mạng (ví dụ: 03x, 05x, 07x, 08x, 09x), tiếp theo là 7 chữ số.

2. **Regex kiểm tra số điện thoại quốc tế (gồm cả mã quốc gia)**:
   ```java
   String internationalPhoneRegex = "^\\+?[0-9]{1,3}?[0-9]{10,12}$";
   ```
   - Mẫu này cho phép số điện thoại có mã quốc gia (bắt đầu với `+` và mã quốc gia từ 1 đến 3 chữ số), sau đó là từ 10 đến 12 chữ số.

3. **Regex kiểm tra số điện thoại 10 chữ số bất kỳ**:
   ```java
   String genericPhoneRegex = "^[0-9]{10}$";
   ```
   - Đây là mẫu đơn giản để kiểm tra bất kỳ số điện thoại nào có đúng 10 chữ số.

4. **Kiểm tra số điện thoại cố định Việt Nam**:
   ```java
   String landlinePhoneRegex = "^(0[2-9][0-9])[0-9]{7}$";
   ```
   - Mẫu này dành cho số điện thoại cố định ở Việt Nam, bắt đầu bằng "0" và mã vùng từ 2 đến 9, theo sau là 7 chữ số.

### Sử dụng regex để kiểm tra số điện thoại:
Sử dụng các mẫu regex này để kiểm tra tính hợp lệ của số điện thoại bằng phương thức `matches()`:

```java
String phone = "0971234567"; // Số điện thoại cần kiểm tra

if (phone.matches(vietnamPhoneRegex)) {
    // Số điện thoại hợp lệ theo định dạng
    Log.d("PhoneValidation", "Số điện thoại hợp lệ.");
} else {
    // Số điện thoại không hợp lệ
    Log.d("PhoneValidation", "Số điện thoại không hợp lệ.");
}
```

Bạn có thể thay `vietnamPhoneRegex` bằng các regex tùy chỉnh khác, dựa vào yêu cầu về định dạng số điện thoại của bạn.
