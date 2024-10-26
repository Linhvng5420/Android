Để thêm nhân viên mới vào Firebase Realtime Database và kiểm tra xem nhân viên đã được thêm thành công hay chưa, bạn có thể sử dụng `addListenerForSingleValueEvent()` hoặc `addOnSuccessListener()` và `addOnFailureListener()` để xử lý kết quả thêm dữ liệu.

Dưới đây là hướng dẫn cụ thể trong Java:

1. **Thêm Nhân Viên Mới**:
   Đầu tiên, bạn cần chuẩn bị dữ liệu của nhân viên và thêm vào Firebase.

   ```java
   DatabaseReference databaseReference = FirebaseDatabase.getInstance().getReference("NhanVien");

   String nhanVienId = databaseReference.push().getKey(); // Tạo id duy nhất cho nhân viên
   NhanVien nhanVien = new NhanVien(nhanVienId, "Tên NV", "Kho", "0123456789", "email@domain.com");

   databaseReference.child(nhanVienId).setValue(nhanVien)
       .addOnSuccessListener(aVoid -> {
           // Xử lý khi thêm thành công
           Log.d("Firebase", "Nhân viên đã thêm thành công!");
           // Hiển thị thông báo nếu cần
       })
       .addOnFailureListener(e -> {
           // Xử lý khi thêm thất bại
           Log.e("Firebase", "Thêm nhân viên thất bại: " + e.getMessage());
       });
   ```

2. **Kiểm tra dữ liệu đã thêm**:
   Sau khi thêm dữ liệu, bạn có thể sử dụng `addListenerForSingleValueEvent()` để kiểm tra lại.

   ```java
   databaseReference.child(nhanVienId).addListenerForSingleValueEvent(new ValueEventListener() {
       @Override
       public void onDataChange(@NonNull DataSnapshot snapshot) {
           if (snapshot.exists()) {
               Log.d("Firebase", "Xác nhận nhân viên đã tồn tại trong Firebase!");
               // Thực hiện các hành động cần thiết khác
           } else {
               Log.d("Firebase", "Không tìm thấy nhân viên trong Firebase.");
           }
       }

       @Override
       public void onCancelled(@NonNull DatabaseError error) {
           Log.e("Firebase", "Lỗi khi kiểm tra dữ liệu: " + error.getMessage());
       }
   });
   ```

   - Phần đầu tiên (`addOnSuccessListener` và `addOnFailureListener`) sẽ xác nhận xem dữ liệu có được ghi thành công hay không.
   - Phần thứ hai (`addListenerForSingleValueEvent`) sẽ kiểm tra lại để đảm bảo dữ liệu đã có trong Firebase. 

Với cách này, bạn có thể vừa thêm nhân viên mới vào Firebase, vừa kiểm tra xem dữ liệu đã được thêm thành công hay chưa.
