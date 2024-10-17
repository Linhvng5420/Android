# **XML trong Android**
### 1. **View**
   - Là thành phần cơ bản của giao diện người dùng. Một `View` đại diện cho các yếu tố như nút bấm, văn bản, hình ảnh, v.v.
   - Ví dụ: `TextView`, `Button`, `ImageView`.

### 2. **ViewGroup**
   - Là một loại đặc biệt của `View` có thể chứa nhiều `View` con hoặc các `ViewGroup` khác.
   - Ví dụ: `LinearLayout`, `RelativeLayout`, `ConstraintLayout`.

### 3. **Layout**
   - Là cách bạn tổ chức các `View` và `ViewGroup` trong giao diện. Một số loại layout phổ biến:
     - **LinearLayout**: Sắp xếp các thành phần theo hàng ngang hoặc dọc.
     - **RelativeLayout**: Sắp xếp dựa trên vị trí tương đối của các thành phần.
     - **ConstraintLayout**: Linh hoạt và mạnh mẽ hơn, cho phép định vị các phần tử dựa trên các ràng buộc.

### 4. **Attributes (Thuộc tính)**
   - Mỗi `View` và `ViewGroup` có thể có nhiều thuộc tính như kích thước, màu sắc, văn bản, hình ảnh, v.v.
   - Ví dụ:
     - `android:layout_width="wrap_content"`: Đặt chiều rộng vừa với nội dung.
     - `android:layout_height="match_parent"`: Đặt chiều cao chiếm toàn bộ không gian cha.

### 5. **Resource**
   - Android quản lý các tài nguyên ngoài (như hình ảnh, màu sắc, văn bản) qua các tệp XML riêng biệt. Ví dụ:
     - `res/drawable`: Hình ảnh và đồ họa.
     - `res/values`: Các giá trị văn bản, màu sắc, kích thước.

### 6. **Fragment**
   - Đại diện cho một phần của giao diện người dùng trong một `Activity`. Nó có thể chứa `View` và `ViewGroup`.

Nắm vững những thành phần này sẽ giúp bạn hiểu cách xây dựng giao diện và quản lý chúng trong Android.
