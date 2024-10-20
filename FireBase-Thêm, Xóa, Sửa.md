Để thêm, xóa, và sửa dữ liệu trong Firebase, bạn có thể sử dụng Firebase Realtime Database hoặc Firestore. Dưới đây là hướng dẫn cho cả hai trường hợp, trong ví dụ này sẽ sử dụng Firebase Realtime Database.

### 1. **Thêm dữ liệu vào Firebase**
```java
// Thêm dữ liệu vào Firebase Realtime Database
DatabaseReference databaseRef = FirebaseDatabase.getInstance().getReference();

// Dữ liệu ví dụ là một đối tượng "NhanVien"
NhanVien nhanVien = new NhanVien("NV01", "Nguyen Van A", "Kho", "0123456789", "a@gmail.com");

// Thêm dữ liệu dưới node "NhanVien"
databaseRef.child("NhanVien").push().setValue(nhanVien)
    .addOnSuccessListener(new OnSuccessListener<Void>() {
        @Override
        public void onSuccess(Void aVoid) {
            // Dữ liệu đã được thêm thành công
            Toast.makeText(getContext(), "Thêm thành công", Toast.LENGTH_SHORT).show();
        }
    })
    .addOnFailureListener(new OnFailureListener() {
        @Override
        public void onFailure(@NonNull Exception e) {
            // Thêm dữ liệu thất bại
            Toast.makeText(getContext(), "Thêm thất bại", Toast.LENGTH_SHORT).show();
        }
    });
```

### 2. **Sửa dữ liệu trong Firebase**
```java
// Giả sử bạn đã biết ID của node bạn muốn sửa
String nhanVienId = "id_of_nhanvien";

// Tham chiếu tới node cần sửa
DatabaseReference databaseRef = FirebaseDatabase.getInstance().getReference("NhanVien").child(nhanVienId);

// Sửa thông tin của nhân viên
Map<String, Object> updates = new HashMap<>();
updates.put("ten", "Nguyen Van B");
updates.put("chucVu", "Giao Hang");

databaseRef.updateChildren(updates)
    .addOnSuccessListener(new OnSuccessListener<Void>() {
        @Override
        public void onSuccess(Void aVoid) {
            // Dữ liệu đã được sửa thành công
            Toast.makeText(getContext(), "Sửa thành công", Toast.LENGTH_SHORT).show();
        }
    })
    .addOnFailureListener(new OnFailureListener() {
        @Override
        public void onFailure(@NonNull Exception e) {
            // Sửa dữ liệu thất bại
            Toast.makeText(getContext(), "Sửa thất bại", Toast.LENGTH_SHORT).show();
        }
    });
```

### 3. **Xóa dữ liệu trong Firebase**
```java
// Giả sử bạn đã biết ID của node bạn muốn xóa
String nhanVienId = "id_of_nhanvien";

// Tham chiếu tới node cần xóa
DatabaseReference databaseRef = FirebaseDatabase.getInstance().getReference("NhanVien").child(nhanVienId);

// Xóa dữ liệu
databaseRef.removeValue()
    .addOnSuccessListener(new OnSuccessListener<Void>() {
        @Override
        public void onSuccess(Void aVoid) {
            // Dữ liệu đã được xóa thành công
            Toast.makeText(getContext(), "Xóa thành công", Toast.LENGTH_SHORT).show();
        }
    })
    .addOnFailureListener(new OnFailureListener() {
        @Override
        public void onFailure(@NonNull Exception e) {
            // Xóa dữ liệu thất bại
            Toast.makeText(getContext(), "Xóa thất bại", Toast.LENGTH_SHORT).show();
        }
    });
```

### 4. **Lưu ý**
- **Push()**: Sử dụng khi bạn muốn Firebase tự động tạo ID duy nhất cho mỗi bản ghi mới.
- **Child()**: Dùng để tham chiếu đến một node con cụ thể.
- **SetValue()**: Thay thế toàn bộ dữ liệu của node.
- **UpdateChildren()**: Chỉ thay thế các trường cụ thể mà bạn muốn cập nhật, các trường khác vẫn giữ nguyên.

Nếu bạn đang làm việc với cơ sở dữ liệu lớn, có thể cần quản lý truy vấn và hiệu suất một cách hiệu quả hơn.
