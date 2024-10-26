# I - Cơ Bản
Fragment là một phần tử giao diện trong Android dùng để xây dựng giao diện linh hoạt và có thể tái sử dụng trong nhiều hoạt động khác nhau. Dưới đây là các bước hướng dẫn cách sử dụng Fragment trong Android Studio với Java.

### 1. **Tạo Fragment mới**
   - Click chuột phải vào thư mục `java` trong Android Studio > `New` > `Fragment` > `Fragment (Blank)`.
   - Đặt tên Fragment, ví dụ: `ExampleFragment`.
   - Sau khi tạo, Android Studio sẽ tạo ra hai tệp:
     - `ExampleFragment.java`: chứa logic của Fragment.
     - `fragment_example.xml`: chứa giao diện của Fragment.

### 2. **Code trong Fragment**
   Ở đây, chúng ta sẽ thiết lập giao diện và các chức năng bên trong `ExampleFragment.java`.

   ```java
   // ExampleFragment.java
   package com.example.app;

   import android.os.Bundle;
   import android.view.LayoutInflater;
   import android.view.View;
   import android.view.ViewGroup;
   import androidx.annotation.NonNull;
   import androidx.annotation.Nullable;
   import androidx.fragment.app.Fragment;

   public class ExampleFragment extends Fragment {
       @Nullable
       @Override
       public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
           // Kết nối fragment_example.xml với Fragment
           return inflater.inflate(R.layout.fragment_example, container, false);
       }
   }
   ```

### 3. **Thêm Fragment vào Activity**
   Có hai cách để thêm Fragment vào Activity: bằng XML và bằng Java code.

   #### a. **Thêm bằng XML**
   - Mở tệp XML của `Activity` mà bạn muốn thêm Fragment, ví dụ: `activity_main.xml`.
   - Thêm `Fragment` vào XML bằng cách sử dụng thẻ `<fragment>`.
   
     ```xml
     <!-- activity_main.xml -->
     <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
         android:layout_width="match_parent"
         android:layout_height="match_parent">
         
         <fragment
             android:id="@+id/fragment_example"
             android:name="com.example.app.ExampleFragment"
             android:layout_width="match_parent"
             android:layout_height="match_parent"/>
         
     </RelativeLayout>
     ```

   #### b. **Thêm bằng Java Code**
   - Mở tệp `MainActivity.java` và sử dụng `FragmentManager` để thêm `ExampleFragment` vào `FrameLayout` trong `activity_main.xml`.

     ```java
     // MainActivity.java
     package com.example.app;

     import android.os.Bundle;
     import androidx.appcompat.app.AppCompatActivity;
     import androidx.fragment.app.Fragment;
     import androidx.fragment.app.FragmentManager;
     import androidx.fragment.app.FragmentTransaction;

     public class MainActivity extends AppCompatActivity {
         @Override
         protected void onCreate(Bundle savedInstanceState) {
             super.onCreate(savedInstanceState);
             setContentView(R.layout.activity_main);

             // Tạo instance của ExampleFragment
             Fragment exampleFragment = new ExampleFragment();

             // Sử dụng FragmentManager để thêm Fragment vào Activity
             FragmentManager fragmentManager = getSupportFragmentManager();
             FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();
             fragmentTransaction.add(R.id.fragment_container, exampleFragment);
             fragmentTransaction.commit();
         }
     }
     ```

   - Trong `activity_main.xml` cần thêm một `FrameLayout` có `id` là `fragment_container`.

     ```xml
     <!-- activity_main.xml -->
     <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
         android:layout_width="match_parent"
         android:layout_height="match_parent">
         
         <FrameLayout
             android:id="@+id/fragment_container"
             android:layout_width="match_parent"
             android:layout_height="match_parent"/>
         
     </RelativeLayout>
     ```

### 4. **Truyền Dữ Liệu vào Fragment**
   Để truyền dữ liệu vào Fragment khi khởi tạo, bạn có thể sử dụng `Bundle`.

   ```java
   // Trong MainActivity.java
   ExampleFragment exampleFragment = new ExampleFragment();
   Bundle bundle = new Bundle();
   bundle.putString("key", "Giá trị truyền vào");
   exampleFragment.setArguments(bundle);

   FragmentTransaction transaction = getSupportFragmentManager().beginTransaction();
   transaction.replace(R.id.fragment_container, exampleFragment);
   transaction.commit();
   ```

   - Nhận dữ liệu trong Fragment:

     ```java
     // Trong ExampleFragment.java
     @Override
     public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
         View view = inflater.inflate(R.layout.fragment_example, container, false);
         
         Bundle bundle = getArguments();
         if (bundle != null) {
             String data = bundle.getString("key");
             // Sử dụng dữ liệu ở đây
         }
         
         return view;
     }
     ```

# II - Vòng đời `Fragment`
Vòng đời của `Fragment` trong Android khá phức tạp, bao gồm nhiều trạng thái khác nhau. Dưới đây là các phương thức chính trong vòng đời của `Fragment`, từ khi được tạo cho đến khi bị hủy:

### 1. Các phương thức vòng đời chính

1. **onAttach(Context context)**: Được gọi khi `Fragment` được gắn vào `Activity`. Ở đây, `Fragment` có thể truy cập vào `Context` của `Activity` mà nó gắn vào.

2. **onCreate(Bundle savedInstanceState)**: Được gọi khi `Fragment` được tạo ra. Sử dụng để khởi tạo các thành phần không liên quan đến giao diện, chẳng hạn như các biến cần lưu trữ.

3. **onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)**: Phương thức này tạo và trả về `View` của `Fragment`. Đây là nơi để bạn gán layout cho `Fragment`.

4. **onViewCreated(View view, Bundle savedInstanceState)**: Được gọi ngay sau `onCreateView()`, nơi này bạn nên thiết lập các `View` cụ thể hoặc sự kiện trên các `View`.

5. **onActivityCreated(Bundle savedInstanceState)**: Được gọi khi `Activity` chứa `Fragment` đã hoàn tất `onCreate()`. Đây là nơi bạn có thể tương tác với `Activity` hoặc truy cập các thành phần của nó.

6. **onStart()**: Được gọi khi `Fragment` có thể nhìn thấy người dùng.

7. **onResume()**: Được gọi khi `Fragment` đã ở trạng thái có thể tương tác với người dùng, như đang hiển thị trên màn hình.

8. **onPause()**: Được gọi khi `Fragment` không còn tương tác với người dùng (ví dụ khi Activity bị tạm dừng hoặc `Fragment` khác được đưa lên trước).

9. **onStop()**: Được gọi khi `Fragment` không còn hiển thị với người dùng.

10. **onDestroyView()**: Được gọi khi `View` của `Fragment` bị hủy. Thường sử dụng để giải phóng tài nguyên liên quan đến `View`.

11. **onDestroy()**: Được gọi khi `Fragment` chuẩn bị bị hủy hoàn toàn. Tại đây, bạn nên giải phóng tất cả tài nguyên còn lại của `Fragment`.

12. **onDetach()**: Được gọi khi `Fragment` được tách ra khỏi `Activity`. Sau khi phương thức này được gọi, `Fragment` không còn liên kết với `Activity`.

### 2. Vòng đời cơ bản của Fragment

Sơ đồ vòng đời của `Fragment` chia thành các trạng thái chính sau đây:

- **Tạo (Created)**: Từ `onAttach()` đến `onActivityCreated()`.
- **Hoạt động (Active)**: Từ `onStart()` đến `onResume()`.
- **Bị ẩn/tạm dừng (Paused)**: Khi `Fragment` chuyển sang `onPause()`.
- **Ngừng hoạt động (Stopped)**: Từ `onStop()` cho đến `onDestroyView()`.
- **Bị hủy (Destroyed)**: Từ `onDestroy()` đến `onDetach()`.

### 3. Minh họa qua ví dụ

```kotlin
class SampleFragment : Fragment() {

    override fun onAttach(context: Context) {
        super.onAttach(context)
        Log.d("FragmentLifecycle", "onAttach")
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        Log.d("FragmentLifecycle", "onCreate")
    }

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        Log.d("FragmentLifecycle", "onCreateView")
        return inflater.inflate(R.layout.fragment_sample, container, false)
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        Log.d("FragmentLifecycle", "onViewCreated")
    }

    override fun onActivityCreated(savedInstanceState: Bundle?) {
        super.onActivityCreated(savedInstanceState)
        Log.d("FragmentLifecycle", "onActivityCreated")
    }

    override fun onStart() {
        super.onStart()
        Log.d("FragmentLifecycle", "onStart")
    }

    override fun onResume() {
        super.onResume()
        Log.d("FragmentLifecycle", "onResume")
    }

    override fun onPause() {
        super.onPause()
        Log.d("FragmentLifecycle", "onPause")
    }

    override fun onStop() {
        super.onStop()
        Log.d("FragmentLifecycle", "onStop")
    }

    override fun onDestroyView() {
        super.onDestroyView()
        Log.d("FragmentLifecycle", "onDestroyView")
    }

    override fun onDestroy() {
        super.onDestroy()
        Log.d("FragmentLifecycle", "onDestroy")
    }

    override fun onDetach() {
        super.onDetach()
        Log.d("FragmentLifecycle", "onDetach")
    }
}
```

Kết quả `Logcat` sẽ cho thấy thứ tự các phương thức trong vòng đời của `Fragment`.

### Tóm tắt

Hiểu rõ vòng đời `Fragment` sẽ giúp quản lý tài nguyên tốt hơn và tránh các lỗi về giao diện hoặc dữ liệu khi `Fragment` bị tạo lại hoặc hủy.

Với các bước trên, bạn đã tích hợp thành công Fragment vào Android project trong Java. 

**Gợi ý khác:**
**a.** Tìm hiểu cách thay thế hoặc loại bỏ Fragment.  
**b.** Thêm giao diện callback giữa Fragment và Activity để truyền dữ liệu hai chiều.
