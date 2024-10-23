Để tạo mã nhân viên theo một quy luật cụ thể như "nv+số" (ví dụ: "nv001", "nv002", ...), bạn cần thực hiện một số bước bổ sung để đảm bảo rằng mã này là duy nhất và tuân theo quy luật bạn mong muốn.

Có thể sử dụng một trong các phương pháp sau:

### 1. **Tự Tạo Mã Nhân Viên Bằng Cách Đếm Số Nhân Viên Hiện Tại**

Bạn có thể đếm số lượng nhân viên hiện tại trong Firebase Realtime Database và dựa vào đó để tạo mã nhân viên mới. Đây là một phương pháp đơn giản và thường được sử dụng.

#### Ví dụ:
```java
DatabaseReference databaseReference = FirebaseDatabase.getInstance().getReference("nhanvien");

// Đếm số lượng nhân viên hiện tại
databaseReference.addListenerForSingleValueEvent(new ValueEventListener() {
    @Override
    public void onDataChange(DataSnapshot dataSnapshot) {
        long employeeCount = dataSnapshot.getChildrenCount(); // Số lượng nhân viên hiện tại
        String maNhanVien = "nv" + String.format("%03d", employeeCount + 1); // Tạo mã nhân viên "nv001", "nv002", ...

        // Đặt mã nhân viên cho đối tượng NhanVien
        nhanVien.setMaNV(maNhanVien);

        // Lưu nhân viên vào Firebase với mã mới
        databaseReference.child(maNhanVien).setValue(nhanVien)
            .addOnSuccessListener(aVoid -> {
                Toast.makeText(getContext(), "Thêm nhân viên thành công", Toast.LENGTH_SHORT).show();
            })
            .addOnFailureListener(e -> {
                Toast.makeText(getContext(), "Thêm nhân viên thất bại: " + e.getMessage(), Toast.LENGTH_SHORT).show();
            });
    }

    @Override
    public void onCancelled(DatabaseError databaseError) {
        Toast.makeText(getContext(), "Lỗi: " + databaseError.getMessage(), Toast.LENGTH_SHORT).show();
    }
});
```

#### Giải thích:
1. **`getChildrenCount()`**: Lấy số lượng nhân viên hiện tại trong Firebase.
2. **Tạo mã nhân viên**: Dùng hàm `String.format("%03d", employeeCount + 1)` để định dạng số thành chuỗi 3 chữ số (ví dụ: `001`, `002`,...).
3. **Lưu dữ liệu**: Thay vì dùng `push()` để tạo khóa ngẫu nhiên, bạn sẽ sử dụng mã nhân viên này làm khóa (`child(maNhanVien)`).

### 2. **Tạo Mã Nhân Viên Dựa Trên Thời Gian Hiện Tại**

Bạn cũng có thể sử dụng thời gian hệ thống để tạo mã nhân viên theo một cách khác, chẳng hạn như "nv" + timestamp.

#### Ví dụ:
```java
long timestamp = System.currentTimeMillis();
String maNhanVien = "nv" + timestamp;

// Đặt mã nhân viên cho đối tượng NhanVien
nhanVien.setMaNV(maNhanVien);

// Lưu nhân viên vào Firebase
databaseReference.child(maNhanVien).setValue(nhanVien)
    .addOnSuccessListener(aVoid -> {
        Toast.makeText(getContext(), "Thêm nhân viên thành công", Toast.LENGTH_SHORT).show();
    })
    .addOnFailureListener(e -> {
        Toast.makeText(getContext(), "Thêm nhân viên thất bại: " + e.getMessage(), Toast.LENGTH_SHORT).show();
    });
```

#### Giải thích:
- **`System.currentTimeMillis()`**: Lấy timestamp hiện tại (là số millisecond kể từ năm 1970), đảm bảo tính duy nhất và không bị trùng lặp.
- Cách này đảm bảo mã nhân viên là duy nhất, nhưng không theo dạng số tăng dần dễ nhìn như phương pháp đếm số nhân viên.

### 3. **Sử Dụng Cloud Functions Hoặc Transaction**

Nếu bạn lo ngại về việc có thể tạo ra các mã nhân viên trùng lặp (ví dụ khi nhiều người dùng thêm nhân viên cùng lúc), bạn có thể sử dụng **Firebase Transactions** hoặc **Cloud Functions** để đảm bảo rằng mã nhân viên được tạo ra một cách an toàn và không bị trùng lặp.

Trong trường hợp này, mỗi lần thêm nhân viên, bạn cần sử dụng transaction để tăng giá trị của một bộ đếm toàn cục trong Firebase, sau đó dùng giá trị đó để tạo mã nhân viên.

#### Ví dụ:
```java
DatabaseReference counterRef = FirebaseDatabase.getInstance().getReference("counter_nhanvien");

// Tăng giá trị của counter_nhanvien
counterRef.runTransaction(new Transaction.Handler() {
    @Override
    public Transaction.Result doTransaction(MutableData mutableData) {
        Long currentCount = mutableData.getValue(Long.class);
        if (currentCount == null) {
            currentCount = 0L;
        }
        mutableData.setValue(currentCount + 1);
        return Transaction.success(mutableData);
    }

    @Override
    public void onComplete(DatabaseError databaseError, boolean committed, DataSnapshot dataSnapshot) {
        if (committed) {
            long newCount = dataSnapshot.getValue(Long.class);
            String maNhanVien = "nv" + String.format("%03d", newCount);

            // Lưu nhân viên với mã mới
            nhanVien.setMaNV(maNhanVien);
            databaseReference.child(maNhanVien).setValue(nhanVien)
                .addOnSuccessListener(aVoid -> {
                    Toast.makeText(getContext(), "Thêm nhân viên thành công", Toast.LENGTH_SHORT).show();
                })
                .addOnFailureListener(e -> {
                    Toast.makeText(getContext(), "Thêm nhân viên thất bại: " + e.getMessage(), Toast.LENGTH_SHORT).show();
                });
        } else {
            Toast.makeText(getContext(), "Giao dịch thất bại", Toast.LENGTH_SHORT).show();
        }
    }
});
```

#### Giải thích:
- **Transaction**: Đảm bảo rằng chỉ có một quy trình có thể tăng giá trị của `counter_nhanvien` tại một thời điểm.
- **counter_nhanvien**: Đây là một giá trị bộ đếm trong Firebase, nó sẽ tăng lên mỗi khi bạn thêm một nhân viên mới.

1. **Tự tạo mã nhân viên bằng cách đếm số nhân viên hiện tại**:
   - Nếu hiện có 10 nhân viên, mã mới sẽ là: `nv011`.

2. **Tạo mã nhân viên dựa trên thời gian hiện tại**:
   - Mã có thể là: `nv1698058939123`.

3. **Sử dụng transaction với bộ đếm toàn cục**:
   - Nếu bộ đếm hiện là 15, mã mới sẽ là: `nv016`.
