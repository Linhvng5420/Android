Để kiểm tra tính duy nhất của `sdt` và `email` trong Firebase, bạn có thể truy vấn tất cả các `NhanVien` trong nhánh "nhanvien", sau đó so sánh từng `sdt` và `email` để đảm bảo chúng không bị trùng, trừ `NhanVien` hiện tại có `idNhanVien` tương ứng với `key` trong snapshot. 

Dưới đây là cách triển khai logic kiểm tra tính duy nhất:

1. **Truy vấn Firebase** để lấy toàn bộ danh sách `NhanVien`.
2. **Kiểm tra tính duy nhất** của `sdt` và `email` bằng cách lặp qua danh sách.
3. **Bỏ qua** `NhanVien` có `idNhanVien` trùng với `key` trong snapshot (nghĩa là `NhanVien` hiện tại).
4. **Thông báo lỗi** nếu phát hiện trùng lặp.

### Mã triển khai chi tiết

```java
private void kiemTraSDTVaEmailTruocKhiLuu(String idNhanVien, String newSDT, String newEmail) {
    DatabaseReference nhanVienRef = FirebaseDatabase.getInstance().getReference("nhanvien");

    nhanVienRef.addListenerForSingleValueEvent(new ValueEventListener() {
        @Override
        public void onDataChange(@NonNull DataSnapshot dataSnapshot) {
            boolean sdtExists = false;
            boolean emailExists = false;

            for (DataSnapshot snapshot : dataSnapshot.getChildren()) {
                // Bỏ qua nhân viên hiện tại
                String currentId = snapshot.getKey();
                if (currentId.equals(idNhanVien)) {
                    continue;
                }

                // Lấy thông tin NhanVien
                NhanVien nhanVien = snapshot.getValue(NhanVien.class);
                if (nhanVien != null) {
                    // Kiểm tra trùng lặp sdt và email
                    if (newSDT.equals(nhanVien.getSdt())) {
                        sdtExists = true;
                    }
                    if (newEmail.equals(nhanVien.getEmailnv())) {
                        emailExists = true;
                    }
                }
                
                // Dừng vòng lặp sớm nếu đã phát hiện trùng lặp
                if (sdtExists || emailExists) {
                    break;
                }
            }

            if (sdtExists) {
                Toast.makeText(getContext(), "Số điện thoại đã được sử dụng.", Toast.LENGTH_SHORT).show();
            }
            if (emailExists) {
                Toast.makeText(getContext(), "Email đã được sử dụng.", Toast.LENGTH_SHORT).show();
            }

            // Nếu không có trùng lặp, tiến hành cập nhật dữ liệu
            if (!sdtExists && !emailExists) {
                capNhatThongTinNhanVien(idNhanVien, newSDT, newEmail);
            }
        }

        @Override
        public void onCancelled(@NonNull DatabaseError databaseError) {
            Log.e("Firebase", "Lỗi khi kiểm tra trùng lặp: " + databaseError.getMessage());
        }
    });
}

// Hàm cập nhật thông tin nhân viên vào Firebase
private void capNhatThongTinNhanVien(String idNhanVien, String newSDT, String newEmail) {
    DatabaseReference dbRef = FirebaseDatabase.getInstance().getReference("nhanvien").child(idNhanVien);
    
    dbRef.child("sdt").setValue(newSDT);
    dbRef.child("emailnv").setValue(newEmail)
            .addOnSuccessListener(aVoid -> {
                Toast.makeText(getContext(), "Cập nhật thành công", Toast.LENGTH_SHORT).show();
            })
            .addOnFailureListener(e -> {
                Toast.makeText(getContext(), "Cập nhật thất bại: " + e.getMessage(), Toast.LENGTH_SHORT).show();
            });
}
```

### Giải thích

- **`kiemTraSDTVaEmailTruocKhiLuu()`**:
  - Hàm này sẽ kiểm tra tính duy nhất của `sdt` và `email`.
  - Sử dụng `addListenerForSingleValueEvent()` để đọc tất cả các nhân viên một lần.
  - Bỏ qua nhân viên hiện tại có `idNhanVien` trùng với `key` snapshot.
  - Nếu phát hiện `sdt` hoặc `email` trùng lặp, hiển thị thông báo lỗi và không cập nhật.

- **`capNhatThongTinNhanVien()`**:
  - Hàm này cập nhật `sdt` và `email` vào Firebase khi không có trùng lặp.

Với cách này, hệ thống đảm bảo `sdt` và `email` duy nhất, ngoại trừ `NhanVien` hiện tại.
