## Phần 1 `Activity`
Dưới đây là tổng hợp kiến thức từ cơ bản đến nâng cao liên quan

---

### 1. **Kiến thức cơ bản về Activity**
   - **Khái niệm Activity**:
     - `Activity` là thành phần cốt lõi của một ứng dụng Android, đại diện cho một màn hình với giao diện người dùng. Mỗi ứng dụng Android phải có ít nhất một `Activity`.
     - Các `Activity` tương tác với người dùng và là điểm chính mà người dùng trải nghiệm ứng dụng.

   - **Vòng đời Activity**:
     Android quản lý các `Activity` thông qua các sự kiện trong vòng đời (`lifecycle`). Đây là các phương thức chính của vòng đời một `Activity`:
     1. **`onCreate()`**:
        - Được gọi khi `Activity` được khởi tạo lần đầu tiên.
        - Đây là nơi bạn thường thiết lập giao diện và khởi tạo các biến.
        - Ví dụ: `setContentView()` để liên kết giao diện với mã nguồn.
     2. **`onStart()`**:
        - Được gọi ngay sau `onCreate()` khi `Activity` sẵn sàng hiển thị với người dùng.
     3. **`onResume()`**:
        - Được gọi sau `onStart()` khi `Activity` bắt đầu tương tác với người dùng.
     4. **`onPause()`**:
        - Được gọi khi `Activity` chuẩn bị chuyển sang trạng thái dừng hoặc bị gián đoạn (ví dụ có cuộc gọi đến).
     5. **`onStop()`**:
        - Được gọi khi `Activity` không còn hiển thị trên màn hình, nhưng nó vẫn còn tồn tại trong bộ nhớ.
     6. **`onDestroy()`**:
        - Được gọi khi `Activity` bị hủy hoàn toàn, giải phóng tài nguyên.

   - **Ví dụ về vòng đời Activity**:
   ```java
   @Override
   protected void onCreate(Bundle savedInstanceState) {
       super.onCreate(savedInstanceState);
       setContentView(R.layout.activity_main);
   }

   @Override
   protected void onResume() {
       super.onResume();
       // Activity đang tương tác với người dùng
   }

   @Override
   protected void onPause() {
       super.onPause();
       // Activity sắp bị gián đoạn
   }

   @Override
   protected void onDestroy() {
       super.onDestroy();
       // Giải phóng tài nguyên
   }
   ```

### 2. **Intent và truyền dữ liệu giữa các Activity**
   - **Intent**:
     - `Intent` là cơ chế chính để khởi động các `Activity` khác hoặc thực hiện các hành động trong ứng dụng.
     - **Explicit Intent**: Được sử dụng để khởi động một `Activity` cụ thể.
       ```java
       Intent intent = new Intent(MainActivity.this, SecondActivity.class);
       startActivity(intent);
       ```
     - **Implicit Intent**: Dùng để yêu cầu một hành động mà không chỉ định chính xác `Activity` sẽ thực hiện hành động đó. Ví dụ: mở trang web, chụp ảnh, gọi điện thoại.
       ```java
       Intent intent = new Intent(Intent.ACTION_VIEW);
       intent.setData(Uri.parse("http://www.example.com"));
       startActivity(intent);
       ```

   - **Truyền dữ liệu giữa các Activity**:
     - Sử dụng `putExtra()` để truyền dữ liệu giữa các `Activity`.
       ```java
       Intent intent = new Intent(MainActivity.this, SecondActivity.class);
       intent.putExtra("KEY", "Giá trị");
       startActivity(intent);
       ```
     - Nhận dữ liệu từ `Intent` trong `Activity` mới.
       ```java
       String data = getIntent().getStringExtra("KEY");
       ```

   - **startActivityForResult()** (API cũ):
     - Dùng để nhận kết quả từ `Activity` khác.
     ```java
     Intent intent = new Intent(MainActivity.this, SecondActivity.class);
     startActivityForResult(intent, REQUEST_CODE);
     ```
     - Nhận kết quả trong `onActivityResult()`.
     ```java
     @Override
     protected void onActivityResult(int requestCode, int resultCode, Intent data) {
         if (requestCode == REQUEST_CODE && resultCode == RESULT_OK) {
             String result = data.getStringExtra("result_key");
         }
     }
     ```

   - **ActivityResultLauncher** (API mới):
     - Cách hiện đại hơn để khởi chạy `Activity` và nhận kết quả:
     ```java
     ActivityResultLauncher<Intent> activityLauncher = registerForActivityResult(
         new ActivityResultContracts.StartActivityForResult(),
         result -> {
             if (result.getResultCode() == Activity.RESULT_OK) {
                 Intent data = result.getData();
                 // Xử lý dữ liệu
             }
         });
     ```

### 3. **Fragment trong Activity**
   - **Fragment** là một phần của giao diện người dùng trong `Activity`. Nó có vòng đời riêng và có thể được thêm, thay thế, hoặc loại bỏ khỏi `Activity`.
   - **Quản lý Fragment**:
     - Sử dụng `FragmentManager` để quản lý `Fragment`.
     - Thêm, thay thế hoặc loại bỏ `Fragment` trong `Activity`:
       ```java
       FragmentManager fragmentManager = getSupportFragmentManager();
       FragmentTransaction transaction = fragmentManager.beginTransaction();
       transaction.add(R.id.fragment_container, new ExampleFragment());
       transaction.commit();
       ```

### 4. **Advanced Activity Lifecycle** (Vòng đời nâng cao)
   - **saveInstanceState()** và **restoreInstanceState()**:
     - Lưu và khôi phục trạng thái của `Activity` khi bị tạm dừng hoặc thay đổi cấu hình (xoay màn hình).
     - Trong `onSaveInstanceState()` lưu dữ liệu cần thiết.
       ```java
       @Override
       protected void onSaveInstanceState(Bundle outState) {
           super.onSaveInstanceState(outState);
           outState.putString("KEY", "Giá trị cần lưu");
       }
       ```
     - Khôi phục dữ liệu trong `onCreate()` hoặc `onRestoreInstanceState()`.
       ```java
       @Override
       protected void onCreate(Bundle savedInstanceState) {
           super.onCreate(savedInstanceState);
           if (savedInstanceState != null) {
               String data = savedInstanceState.getString("KEY");
           }
       }
       ```

   - **Configuration Changes**:
     - Khi cấu hình hệ thống thay đổi (ví dụ xoay màn hình), `Activity` có thể bị phá huỷ và khởi động lại. Sử dụng `android:configChanges` trong file `AndroidManifest.xml` để xử lý các thay đổi cấu hình mà không khởi động lại `Activity`.
       ```xml
       <activity android:name=".MainActivity"
           android:configChanges="orientation|screenSize" />
       ```

   - **Back Stack và Quản lý Activity**:
     - Android quản lý các `Activity` trong một `Back Stack`. Khi một `Activity` mới được khởi chạy, nó sẽ được đặt lên đầu của stack, và khi người dùng nhấn nút back, `Activity` sẽ được lấy ra khỏi stack và quay về `Activity` trước đó.
     - Sử dụng cờ `FLAG_ACTIVITY_CLEAR_TOP`, `FLAG_ACTIVITY_NEW_TASK`, v.v. để kiểm soát hành vi khi khởi động `Activity`.

### 5. **Kiến thức nâng cao về Activity**
   - **Service Binding**:
     - `Activity` có thể giao tiếp với `Service` thông qua việc binding, giúp `Activity` sử dụng dịch vụ chạy ngầm để thực hiện các tác vụ dài hạn.
   
   - **Fragment Transactions và Back Stack**:
     - Khi thêm hoặc thay thế `Fragment`, bạn có thể quản lý `Back Stack` của `Fragment` để xử lý khi người dùng nhấn nút back.
     ```java
     transaction.addToBackStack(null);
     ```

   - **Multi-Window Mode**:
     - Kể từ Android 7.0 (API 24), ứng dụng có thể chạy trong chế độ nhiều cửa sổ (multi-window). Bạn cần kiểm tra `Activity` có chạy trong chế độ này hay không và điều chỉnh giao diện phù hợp.
     ```java
     if (isInMultiWindowMode()) {
         // Chế độ multi-window
     }
     ```

   - **Permissions và Activity**:
     - Android yêu cầu cấp quyền từ người dùng cho các tác vụ nhạy cảm (ví dụ: truy cập camera, vị trí). Sử dụng `ActivityCompat.requestPermissions()` để yêu cầu quyền và xử lý kết quả trong `onRequestPermissionsResult()`.
     ```java
     ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.CAMERA}, REQUEST_CODE);
     ```

   - **Dialog và DialogFragment**:
     - `Dialog` là một cửa sổ bật lên mà bạn có thể sử dụng trong `Activity` để hiển thị thông tin hoặc yêu cầu người dùng nhập liệu. Sử dụng `DialogFragment` để quản lý vòng đời của `Dialog` tốt hơn.

   - **Tối ưu hóa hiệu suất của Activity**:
     - **Memory Leaks**: Đảm bảo rằng không có rò rỉ bộ nhớ bằng cách ngắt tất cả các tham chiếu khi `Activity` bị hủy.
     - **AsyncTask và Loader**: Sử dụng `AsyncTask` hoặc `Loader` để thực hiện

 các tác vụ trong nền mà không làm gián đoạn giao diện người dùng.

---

## Phần 2

### 1. **Fragment**
   - **Khái niệm Fragment**:
     - `Fragment` là một phần của giao diện trong `Activity`, có vòng đời riêng nhưng được liên kết với `Activity`.
     - Có thể tái sử dụng `Fragment` trong nhiều `Activity`, thích hợp cho giao diện phức tạp, linh hoạt (đặc biệt là với màn hình lớn).

   - **Quản lý Fragment**:
     - **FragmentManager**: Dùng để quản lý các thao tác thêm, thay thế, hoặc xóa `Fragment`.
     - **FragmentTransaction**: Thực hiện các thao tác với `Fragment`.
       ```java
       FragmentManager fragmentManager = getSupportFragmentManager();
       FragmentTransaction transaction = fragmentManager.beginTransaction();
       transaction.add(R.id.fragment_container, new ExampleFragment());
       transaction.commit();
       ```

   - **Vòng đời của Fragment**:
     - `Fragment` có vòng đời riêng và tương tự `Activity`, bao gồm các phương thức như: `onCreate()`, `onCreateView()`, `onResume()`, `onPause()`, `onDestroyView()`, v.v.

   - **Giao tiếp giữa Fragment và Activity**:
     - Sử dụng interface để giao tiếp giữa `Fragment` và `Activity`.
     ```java
     public interface OnFragmentInteractionListener {
         void onFragmentInteraction(String data);
     }

     // Trong Activity
     @Override
     public void onFragmentInteraction(String data) {
         // Xử lý dữ liệu từ Fragment
     }
     ```

     ```
     @Nullable
    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        // Khởi tạo View Binding cho Fragment và liên kết với layout fragment_owner_nhanvien.xml
        ownerNhanvienBinding = FragmentOwnerNhanvienBinding.inflate(inflater, container, false);

        // Trả về đối tượng View được tạo từ binding, đây là root view của Fragment
        return ownerNhanvienBinding.getRoot();
    }

    // TODO: HÀM XỬ LÝ CHỨC NĂNG CỦA VIEW APP
    @Override
    public void onViewCreated(@NonNull View view, @Nullable Bundle savedInstanceState) {
        super.onViewCreated(view, savedInstanceState);

        // Nhận emailUser từ Bundle
        if (getArguments() != null) {
            emailUser = getArguments().getString("emailUser");
            Log.d("l.e", "onViewCreated: emailUser = " + emailUser);
        } else Log.d("l.e", "Nhận emailUser từ Bundle: getArguments() = null");

        //RecycleView: Thiết lập layout cho RecyclerView, sử dụng LinearLayoutManager để hiển thị danh sách theo chiều dọc
        ownerNhanvienBinding.ownerRcvNhanVien.setLayoutManager(new LinearLayoutManager(getContext()));
        // Khởi tạo danh sách nhân viên trống và adapter
        List<NhanVien> nhanVienList = new ArrayList<>();
        nhanVienAdapter = new NhanVienAdapter(nhanVienList);
        ownerNhanvienBinding.ownerRcvNhanVien.setAdapter(nhanVienAdapter);

        // Firebase: lấy dữ liệu từ Firebase
        getNhanVienData();

        //nhấn vào recycleview nhân viên
        nhanVaoItemNhanVien();

        // nhấn nút thêm nhân viên
        nhanNutThemNhanVien();

        // Thiết lập tìm kiếm
        timKiemNhanVien();
        // Lắng nghe sự kiện nhấn ra ngoài thanh tìm kiếm để tắt con trỏ và ẩn bàn phím
        ownerNhanvienBinding.getRoot().setOnTouchListener((v, event) -> {
            hideKeyboard(v); // Ẩn bàn phím
            ownerNhanvienBinding.searchView.clearFocus(); // Xóa focus để tắt con trỏ trong SearchView
            return false;
        });
    }
     ```

   - **Back Stack và Fragment**:
     - Quản lý `Back Stack` của `Fragment` khi người dùng điều hướng qua các màn hình khác nhau trong ứng dụng.

---

### 2. **Layouts và Views**
   - **Layouts cơ bản**:
     - Hiểu rõ các loại layout phổ biến: `LinearLayout`, `RelativeLayout`, `ConstraintLayout`, `FrameLayout`, và cách sử dụng chúng trong thiết kế giao diện.

   - **ConstraintLayout** (nâng cao):
     - Đây là layout linh hoạt và mạnh mẽ nhất hiện tại. Bạn có thể sử dụng nó để tạo giao diện phức tạp mà không cần lồng quá nhiều layout con.
     - Sử dụng **Chains**, **Guidelines**, và **Barriers** để tối ưu hóa giao diện.

   - **RecyclerView**:
     - Là một view quan trọng để hiển thị danh sách dữ liệu lớn hiệu quả hơn so với `ListView`.
     - Hiểu về `ViewHolder` pattern, `Adapter`, và `LayoutManager`.
     ```java
     RecyclerView recyclerView = findViewById(R.id.recycler_view);
     recyclerView.setLayoutManager(new LinearLayoutManager(this));
     recyclerView.setAdapter(new ExampleAdapter());
     ```

   - **CardView** và **RecyclerView ItemDecoration**:
     - `CardView` giúp tạo các phần tử giao diện đẹp hơn, đặc biệt hữu ích khi kết hợp với `RecyclerView`.
     - `ItemDecoration` giúp thêm khoảng cách, đường kẻ giữa các phần tử trong `RecyclerView`.

---

### 3. **ViewModel và LiveData (Architecture Components)**
   - **ViewModel**:
     - Một thành phần của **Android Architecture Components** giúp lưu trữ và quản lý dữ liệu UI trong suốt vòng đời của `Activity` hoặc `Fragment`. `ViewModel` giúp tránh mất dữ liệu khi cấu hình thay đổi (như khi xoay màn hình).
     ```java
     public class ExampleViewModel extends ViewModel {
         private MutableLiveData<String> data;
         
         public MutableLiveData<String> getData() {
             if (data == null) {
                 data = new MutableLiveData<>();
             }
             return data;
         }
     }
     ```

   - **LiveData**:
     - Một lớp dữ liệu có thể quan sát được. Khi dữ liệu trong `LiveData` thay đổi, UI sẽ tự động cập nhật mà không cần phải thêm mã xử lý phức tạp.
     ```java
     viewModel.getData().observe(this, new Observer<String>() {
         @Override
         public void onChanged(String data) {
             // Cập nhật giao diện khi dữ liệu thay đổi
         }
     });
     ```

   - **DataBinding**:
     - `DataBinding` cho phép liên kết dữ liệu trực tiếp từ mô hình tới giao diện mà không cần viết quá nhiều mã xử lý.
     - Kết hợp với `LiveData` để dễ dàng cập nhật giao diện khi dữ liệu thay đổi.
     ```xml
     <layout>
         <data>
             <variable
                 name="viewModel"
                 type="com.example.ExampleViewModel" />
         </data>
         <TextView
             android:text="@{viewModel.data}" />
     </layout>
     ```

---

### 4. **Networking và API**
   - **HTTP Networking**:
     - Sử dụng **Retrofit** hoặc **Volley** để thực hiện các yêu cầu HTTP. Retrofit là một trong những thư viện phổ biến nhất hiện nay do cú pháp đơn giản và mạnh mẽ.
     ```java
     Retrofit retrofit = new Retrofit.Builder()
         .baseUrl("https://api.example.com/")
         .addConverterFactory(GsonConverterFactory.create())
         .build();
     
     ExampleApi api = retrofit.create(ExampleApi.class);
     ```

   - **Room Database (ORM)**:
     - Là một phần của Android Jetpack, giúp lưu trữ dữ liệu cục bộ trên thiết bị dưới dạng cơ sở dữ liệu SQLite, nhưng đơn giản hóa các thao tác với cú pháp ORM.
     ```java
     @Entity
     public class User {
         @PrimaryKey
         public int uid;
         
         @ColumnInfo(name = "user_name")
         public String userName;
     }
     ```

   - **Paging Library**:
     - Giúp tải dữ liệu thành từng phần (paging) trong các ứng dụng cần hiển thị danh sách dữ liệu lớn, giảm tải cho bộ nhớ và giúp ứng dụng mượt mà hơn.

---

### 5. **Advanced UI & Animation**
   - **Custom Views**:
     - Tạo các `View` tuỳ chỉnh để đáp ứng yêu cầu thiết kế đặc biệt. Hiểu cách override các phương thức `onDraw()`, `onMeasure()`.

   - **Canvas và Drawing**:
     - Sử dụng `Canvas` để vẽ các hình ảnh tuỳ chỉnh lên màn hình, tạo hiệu ứng đặc biệt cho giao diện.

   - **Transition và Animation**:
     - Sử dụng `Shared Element Transition` để tạo hiệu ứng chuyển động mượt mà khi chuyển đổi giữa các `Activity` hoặc `Fragment`.
     - Sử dụng `ObjectAnimator`, `AnimatorSet` và `PropertyAnimation` để tạo các hiệu ứng động phức tạp cho UI.

   - **MotionLayout**:
     - Một layout mạnh mẽ cho phép bạn tạo các hiệu ứng chuyển động phức tạp mà không cần viết mã nhiều. Là sự kết hợp giữa `ConstraintLayout` và các bộ điều khiển hoạt cảnh.

---

### 6. **Dependency Injection (Dagger, Hilt)**
   - **Dagger 2**:
     - Một framework mạnh mẽ cho việc tiêm phụ thuộc (Dependency Injection), giúp quản lý các đối tượng phức tạp trong ứng dụng.
     - Giảm thiểu các phụ thuộc cứng và tăng khả năng mở rộng của ứng dụng.

   - **Hilt** (phát triển từ Dagger):
     - Là một phiên bản đơn giản hóa của Dagger, dễ sử dụng hơn cho các dự án Android, đặc biệt khi kết hợp với Jetpack.
     ```java
     @HiltAndroidApp
     public class MyApplication extends Application {}
     ```

---

### 7. **Kotlin Coroutines và Flow**
   - **Coroutines**:
     - Thư viện giúp bạn xử lý các tác vụ bất đồng bộ dễ dàng hơn so với `AsyncTask` hay `Thread`.
     - Giúp viết mã bất đồng bộ mượt mà và không bị chặn.
     ```kotlin
     GlobalScope.launch {
         val result = async { fetchData() }.await()
     }
     ```

   - **Flow**:
     - Là một API mạnh mẽ cho việc xử lý dòng dữ liệu không đồng bộ và phản ứng lại các sự kiện.
     ```kotlin
     val flow = flow {
         emit(fetchData())
     }
     ```

---

### 8. **Testing**
   - **Unit Testing**:
     - Sử dụng JUnit hoặc Mockito để viết unit test cho mã nguồn của bạn.
     - Test các phương thức, luồng logic mà không cần phụ thuộc vào giao diện.

   - **UI Testing**:
     - Sử dụng Espresso hoặc UI Automator để viết các bài kiểm tra tự động cho giao diện người dùng.
     - Giúp phát hiện các lỗi về giao diện và tương tác người dùng mà không cần kiểm tra thủ công.

---

### 9. **Performance Optimization**
   - **Memory Leaks**:
     - Tránh rò rỉ bộ nhớ bằng cách giải phóng tài nguyên không cần thiết. Sử dụng **LeakCanary** để kiểm tra và phát hiện rò rỉ bộ nhớ.
   
   - **Battery Optimization**:
     - Tối ưu hóa mức tiêu thụ pin bằng cách sử dụng `JobScheduler`, `WorkManager` cho các tác vụ nền và
