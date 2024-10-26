Để bắt sự kiện cho `TextInputEditText` và kiểm tra tính hợp lệ của email (định dạng và tính duy nhất), bạn có thể sử dụng `TextWatcher`. Mỗi khi người dùng nhập, `TextWatcher` sẽ kiểm tra định dạng email và sau đó xác thực tính duy nhất của email qua Firebase. 

Dưới đây là cách thực hiện:

1. **Thiết lập `TextWatcher` cho `TextInputEditText`**:
   Sử dụng `TextWatcher` để lắng nghe các thay đổi trong `TextInputEditText`.

2. **Kiểm tra định dạng email**:
   Sử dụng regex để kiểm tra xem chuỗi có phải là email hợp lệ hay không.

3. **Kiểm tra tính duy nhất qua Firebase**:
   Nếu định dạng email hợp lệ, truy vấn Firebase để xác nhận email chưa tồn tại.

Dưới đây là mã cụ thể trong Java:

```java
TextInputEditText emailEditText = findViewById(R.id.emailEditText);
DatabaseReference databaseReference = FirebaseDatabase.getInstance().getReference("account");

emailEditText.addTextChangedListener(new TextWatcher() {
    @Override
    public void beforeTextChanged(CharSequence s, int start, int count, int after) {
        // Không cần xử lý
    }

    @Override
    public void onTextChanged(CharSequence s, int start, int before, int count) {
        // Kiểm tra định dạng email mỗi khi có thay đổi
        String email = s.toString();
        if (isValidEmail(email)) {
            // Kiểm tra tính duy nhất nếu email hợp lệ
            checkEmailUniqueness(email);
        } else {
            emailEditText.setError("Email không hợp lệ");
        }
    }

    @Override
    public void afterTextChanged(Editable s) {
        // Không cần xử lý
    }
});

// Phương thức kiểm tra định dạng email
private boolean isValidEmail(String email) {
    return android.util.Patterns.EMAIL_ADDRESS.matcher(email).matches();
}

// Phương thức kiểm tra tính duy nhất của email
private void checkEmailUniqueness(String email) {
    databaseReference.orderByChild("email").equalTo(email)
        .addListenerForSingleValueEvent(new ValueEventListener() {
            @Override
            public void onDataChange(@NonNull DataSnapshot snapshot) {
                if (snapshot.exists()) {
                    emailEditText.setError("Email đã tồn tại");
                } else {
                    emailEditText.setError(null); // Xóa lỗi nếu email duy nhất
                }
            }

            @Override
            public void onCancelled(@NonNull DatabaseError error) {
                Log.e("Firebase", "Lỗi khi kiểm tra email: " + error.getMessage());
            }
        });
}
```

### Giải thích mã:

- **`isValidEmail()`**: Hàm kiểm tra định dạng email bằng regex từ `Patterns.EMAIL_ADDRESS`.
- **`checkEmailUniqueness()`**: Nếu định dạng hợp lệ, hàm này sẽ kiểm tra xem email đã tồn tại trong Firebase chưa.
- **`setError()`**: Hiển thị lỗi nếu email không hợp lệ hoặc đã tồn tại.

Với cách này, khi người dùng nhập email, ứng dụng sẽ tự động kiểm tra định dạng và tính duy nhất của email.
