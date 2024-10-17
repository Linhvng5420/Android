# **Bắt sự kiện phổ biến**
Trong Android Studio, ngoài Button, các thành phần giao diện khác cũng có thể bắt nhiều loại sự kiện. Dưới đây là các kiểu bắt sự kiện phổ biến cho các thành phần khác nhau trong giao diện Android:

 1. **EditText (Trường nhập liệu văn bản)**:
   - **onTextChanged**: Được kích hoạt khi có sự thay đổi trong nội dung văn bản.
   - **beforeTextChanged**: Được kích hoạt ngay trước khi nội dung văn bản thay đổi.
   - **afterTextChanged**: Được kích hoạt ngay sau khi nội dung văn bản thay đổi.
   - **onFocusChange**: Kích hoạt khi thành phần nhận hoặc mất focus.
   - **onEditorAction**: Được kích hoạt khi người dùng nhấn nút **"Done"** hoặc **"Enter"** trên bàn phím.

 2. **CheckBox (Hộp kiểm)**:
   - **onCheckedChanged**: Kích hoạt khi trạng thái của CheckBox thay đổi (được chọn hoặc bỏ chọn).
   - **onClick**: Kích hoạt khi người dùng nhấn vào CheckBox.

 3. **RadioButton (Nút chọn)**:
   - **onCheckedChanged**: Được kích hoạt khi RadioButton được chọn hoặc bỏ chọn.
   - **onClick**: Kích hoạt khi người dùng nhấn vào RadioButton.

 4. **Spinner (Trình thả xuống)**:
   - **onItemSelected**: Kích hoạt khi một mục trong Spinner được chọn.
   - **onNothingSelected**: Kích hoạt khi không có mục nào trong Spinner được chọn.

 5. **SeekBar (Thanh trượt)**:
   - **onProgressChanged**: Được kích hoạt khi giá trị của SeekBar thay đổi.
   - **onStartTrackingTouch**: Kích hoạt khi người dùng bắt đầu tương tác với SeekBar (bắt đầu kéo).
   - **onStopTrackingTouch**: Kích hoạt khi người dùng kết thúc tương tác với SeekBar (thả ra).

6. **ImageView (Hình ảnh)**:
   - **onClick**: Kích hoạt khi người dùng nhấn vào ImageView.
   - **onTouch**: Kích hoạt khi có tương tác chạm vào ImageView (nhấn, kéo, thả).

7. **ListView/RecyclerView (Danh sách)**:
   - **onItemClick**: Được kích hoạt khi một mục trong danh sách được nhấn.
   - **onItemLongClick**: Kích hoạt khi một mục trong danh sách được giữ lâu.
   - **onScroll**: Kích hoạt khi người dùng cuộn qua danh sách.
8. **Switch (Nút bật/tắt)**:
   - **onCheckedChanged**: Kích hoạt khi trạng thái của Switch thay đổi (bật hoặc tắt).
   - **onClick**: Kích hoạt khi người dùng nhấn vào Switch.

9. **ProgressBar (Thanh tiến trình)**:
   - **onProgressChanged**: Kích hoạt khi giá trị của ProgressBar thay đổi.
   - **onStartTrackingTouch**: Kích hoạt khi người dùng bắt đầu kéo thanh tiến trình (dùng khi ProgressBar kết hợp với SeekBar).
   - **onStopTrackingTouch**: Kích hoạt khi người dùng thả thanh tiến trình.

10. **WebView (Trình duyệt nhúng)**:
   - **onPageStarted**: Kích hoạt khi trang bắt đầu tải.
   - **onPageFinished**: Kích hoạt khi trang tải xong.
   - **onReceivedError**: Kích hoạt khi có lỗi xảy ra trong quá trình tải trang.

11. **View (Thành phần chung)**:
   - **onClick**: Được kích hoạt khi người dùng nhấn vào bất kỳ thành phần nào có thể tương tác.
   - **onLongClick**: Kích hoạt khi người dùng nhấn và giữ lâu vào thành phần.
   - **onTouch**: Kích hoạt khi có thao tác chạm vào thành phần (nhấn, kéo, vuốt).
   - **onFocusChange**: Kích hoạt khi thành phần nhận hoặc mất focus.

Những sự kiện này cho phép bạn theo dõi và xử lý các tương tác của người dùng với các thành phần giao diện khác nhau trong ứng dụng Android. Việc lựa chọn sự kiện nào sẽ phụ thuộc vào thành phần bạn đang làm việc và hành động bạn muốn thực hiện.

# **XML trong Android**
**View**
   - Là thành phần cơ bản của giao diện người dùng. Một `View` đại diện cho các yếu tố như nút bấm, văn bản, hình ảnh, v.v.
   - Ví dụ: `TextView`, `Button`, `ImageView`.

**ViewGroup**
   - Là một loại đặc biệt của `View` có thể chứa nhiều `View` con hoặc các `ViewGroup` khác.
   - Ví dụ: `LinearLayout`, `RelativeLayout`, `ConstraintLayout`.

**Layout**
   - Là cách bạn tổ chức các `View` và `ViewGroup` trong giao diện. Một số loại layout phổ biến:
     - **LinearLayout**: Sắp xếp các thành phần theo hàng ngang hoặc dọc.
     - **RelativeLayout**: Sắp xếp dựa trên vị trí tương đối của các thành phần.
     - **ConstraintLayout**: Linh hoạt và mạnh mẽ hơn, cho phép định vị các phần tử dựa trên các ràng buộc.

**Attributes (Thuộc tính)**
   - Mỗi `View` và `ViewGroup` có thể có nhiều thuộc tính như kích thước, màu sắc, văn bản, hình ảnh, v.v.
   - Ví dụ:
     - `android:layout_width="wrap_content"`: Đặt chiều rộng vừa với nội dung.
     - `android:layout_height="match_parent"`: Đặt chiều cao chiếm toàn bộ không gian cha.

**Resource**
   - Android quản lý các tài nguyên ngoài (như hình ảnh, màu sắc, văn bản) qua các tệp XML riêng biệt. Ví dụ:
     - `res/drawable`: Hình ảnh và đồ họa.
     - `res/values`: Các giá trị văn bản, màu sắc, kích thước.

**Fragment**
   - Đại diện cho một phần của giao diện người dùng trong một `Activity`. Nó có thể chứa `View` và `ViewGroup`.

Nắm vững những thành phần này sẽ giúp bạn hiểu cách xây dựng giao diện và quản lý chúng trong Android.
