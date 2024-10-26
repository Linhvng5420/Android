Để thoát ứng dụng nếu biến `loginEmailUser` là `null`, bạn có thể sử dụng phương thức `finishAffinity()` hoặc `finish()` trong Activity. Cả hai cách này sẽ đóng Activity hiện tại và các Activity khác trong stack, giúp thoát ứng dụng một cách triệt để. 

Dưới đây là cách thực hiện:

```java
if (loginEmailUser == null) {
    // Hiển thị thông báo nếu cần
    Toast.makeText(this, "Email đăng nhập không hợp lệ, thoát ứng dụng.", Toast.LENGTH_SHORT).show();
    
    // Thoát ứng dụng
    finishAffinity(); // Đóng tất cả Activity và thoát ứng dụng
}
```

### Giải thích:
- **`finishAffinity()`**: Đóng tất cả các Activity trong ứng dụng, thường được dùng để thoát ứng dụng hoàn toàn.
- **`Toast`**: Hiển thị thông báo ngắn gọn trước khi thoát (tuỳ chọn).

Nếu bạn chỉ cần thoát Activity hiện tại, có thể sử dụng `finish()` thay cho `finishAffinity()`.
