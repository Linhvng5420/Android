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

Dưới đây là minh họa bằng Java cho các phương thức `onCreate()`, `onCreateView()`, và `onViewCreated()` trong `Fragment`. Mỗi phương thức này có vai trò và vị trí trong vòng đời của `Fragment`, đảm bảo Fragment được khởi tạo và hiển thị đúng cách.

### 1. Các phương thức và sự khác biệt

- **onCreate()**:
  - Được gọi khi `Fragment` được khởi tạo lần đầu.
  - Chủ yếu dùng để khởi tạo các thành phần không liên quan đến giao diện, như dữ liệu hoặc các biến toàn cục.
  
- **onCreateView()**:
  - Được gọi ngay sau `onCreate()`, dùng để khởi tạo giao diện (View) của `Fragment`.
  - Trả về một `View` cho `Fragment` và có thể sử dụng `LayoutInflater` để kết nối với file layout XML.

- **onViewCreated()**:
  - Được gọi ngay sau khi `onCreateView()` hoàn tất.
  - Phương thức này cho phép bạn thực hiện các thao tác trên `View` đã tạo, như thiết lập sự kiện cho các nút bấm hoặc khởi tạo các thành phần UI bên trong `Fragment`.

### 2. Ví dụ minh họa

Dưới đây là ví dụ về một `Fragment` trong Java để minh họa từng phương thức:

```java
import android.os.Bundle;
import androidx.annotation.NonNull;
import androidx.annotation.Nullable;
import androidx.fragment.app.Fragment;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.util.Log;

public class SampleFragment extends Fragment {

    // Biến toàn cục
    private String data;

    // Được gọi khi Fragment được tạo ra
    @Override
    public void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        Log.d("FragmentLifecycle", "onCreate: Fragment is being created");

        // Khởi tạo biến dữ liệu hoặc các thành phần không giao diện
        data = "Dữ liệu được khởi tạo trong onCreate";
    }

    // Được gọi để tạo View cho Fragment
    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, 
                             @Nullable ViewGroup container, 
                             @Nullable Bundle savedInstanceState) {
        Log.d("FragmentLifecycle", "onCreateView: Inflating layout for Fragment");

        // Liên kết với layout fragment_sample.xml
        return inflater.inflate(R.layout.fragment_sample, container, false);
    }

    // Được gọi sau khi View đã được tạo xong
    @Override
    public void onViewCreated(@NonNull View view, @Nullable Bundle savedInstanceState) {
        super.onViewCreated(view, savedInstanceState);
        Log.d("FragmentLifecycle", "onViewCreated: View created, initializing UI components");

        // Khởi tạo hoặc thiết lập các thành phần giao diện
        // Ví dụ: tìm một TextView trong layout và thiết lập dữ liệu
        TextView textView = view.findViewById(R.id.textView);
        textView.setText(data);
    }
}
```

#### Giải thích chi tiết từng phần

1. **onCreate()**:
   - Khởi tạo biến `data` với nội dung "Dữ liệu được khởi tạo trong onCreate".
   - Dữ liệu này sẽ được sử dụng trong `Fragment`, nhưng không liên quan đến việc hiển thị giao diện.

2. **onCreateView()**:
   - Được gọi để tạo giao diện cho `Fragment`.
   - Trong phương thức này, `LayoutInflater` được sử dụng để liên kết `Fragment` với layout `fragment_sample.xml`.
   - Phương thức trả về `View` này sẽ được hiển thị cho người dùng.

3. **onViewCreated()**:
   - Được gọi sau khi `View` được tạo xong trong `onCreateView()`.
   - Tại đây, bạn có thể truy cập các thành phần của `View` vừa được tạo (như `TextView`) và khởi tạo các thành phần UI hoặc thiết lập sự kiện.
   - Trong ví dụ trên, `TextView` được tìm và thiết lập nội dung bằng biến `data` từ `onCreate()`.

### Tóm tắt sự khác biệt

- **onCreate()**: Dùng để khởi tạo các thành phần không giao diện.
- **onCreateView()**: Dùng để tạo và trả về `View` cho `Fragment`.
- **onViewCreated()**: Dùng để thao tác với các thành phần của `View` sau khi đã được tạo xong.

Hiểu rõ sự khác biệt này giúp tối ưu hóa cách quản lý vòng đời và hiệu suất khi làm việc với `Fragment`.

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
