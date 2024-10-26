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

Dưới đây là ví dụ về vòng đời của `Fragment` bằng Java, hiển thị các phương thức chính và cách `Log` từng giai đoạn để thấy rõ thứ tự thực thi.

### Ví dụ minh họa vòng đời của Fragment bằng Java

```java
package com.example.myapp;

import android.content.Context;
import android.os.Bundle;
import android.util.Log;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;

import androidx.annotation.NonNull;
import androidx.annotation.Nullable;
import androidx.fragment.app.Fragment;

public class SampleFragment extends Fragment {

    private static final String TAG = "FragmentLifecycle";

    @Override
    public void onAttach(@NonNull Context context) {
        super.onAttach(context);
        Log.d(TAG, "onAttach");
    }

    @Override
    public void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        Log.d(TAG, "onCreate");
    }

    @Nullable
    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        Log.d(TAG, "onCreateView");
        return inflater.inflate(R.layout.fragment_sample, container, false);
    }

    @Override
    public void onViewCreated(@NonNull View view, @Nullable Bundle savedInstanceState) {
        super.onViewCreated(view, savedInstanceState);
        Log.d(TAG, "onViewCreated");
    }

    @Override
    public void onActivityCreated(@Nullable Bundle savedInstanceState) {
        super.onActivityCreated(savedInstanceState);
        Log.d(TAG, "onActivityCreated");
    }

    @Override
    public void onStart() {
        super.onStart();
        Log.d(TAG, "onStart");
    }

    @Override
    public void onResume() {
        super.onResume();
        Log.d(TAG, "onResume");
    }

    @Override
    public void onPause() {
        super.onPause();
        Log.d(TAG, "onPause");
    }

    @Override
    public void onStop() {
        super.onStop();
        Log.d(TAG, "onStop");
    }

    @Override
    public void onDestroyView() {
        super.onDestroyView();
        Log.d(TAG, "onDestroyView");
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        Log.d(TAG, "onDestroy");
    }

    @Override
    public void onDetach() {
        super.onDetach();
        Log.d(TAG, "onDetach");
    }
}
```

### Giải thích từng phương thức

1. **onAttach**: Gọi khi `Fragment` được gắn vào `Activity`.
2. **onCreate**: Được gọi khi `Fragment` được tạo ra để khởi tạo các thành phần không liên quan đến giao diện.
3. **onCreateView**: Tạo và trả về `View` của `Fragment`. Đây là nơi khai báo layout cho `Fragment`.
4. **onViewCreated**: Được gọi ngay sau `onCreateView`, thích hợp để thiết lập các thành phần UI cụ thể.
5. **onActivityCreated**: Được gọi khi `Activity` chứa `Fragment` đã hoàn tất `onCreate`. Có thể truy cập các thành phần từ `Activity` ở đây.
6. **onStart**: Được gọi khi `Fragment` trở nên hiển thị với người dùng.
7. **onResume**: Được gọi khi `Fragment` ở trạng thái tương tác với người dùng.
8. **onPause**: Được gọi khi `Fragment` không còn tương tác với người dùng.
9. **onStop**: Được gọi khi `Fragment` không còn hiển thị.
10. **onDestroyView**: Gọi khi `View` của `Fragment` bị hủy, giúp giải phóng tài nguyên của `View`.
11. **onDestroy**: Được gọi khi `Fragment` sắp bị hủy hoàn toàn.
12. **onDetach**: Được gọi khi `Fragment` bị tách khỏi `Activity`.

### Kết quả trong Logcat

Các `Log.d` trong mỗi phương thức sẽ giúp bạn thấy được thứ tự chính xác khi các phương thức này được gọi trong vòng đời của `Fragment`.

**Gợi ý khác:**

**a.** Tìm hiểu cách thay thế hoặc loại bỏ Fragment.  
**b.** Thêm giao diện callback giữa Fragment và Activity để truyền dữ liệu hai chiều.
