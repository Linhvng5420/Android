`RecyclerView` sử dụng một `Adapter` để kết nối dữ liệu với giao diện, và `ViewHolder` để lưu trữ các view cho các mục dữ liệu. Trong `RecyclerView`, `Adapter` và `ViewHolder` là hai thành phần quan trọng giúp quản lý và hiển thị dữ liệu một cách hiệu quả. Dưới đây là cách chúng hoạt động:

1. **RecyclerView.Adapter**:
   - `Adapter` là lớp trung gian giúp lấy dữ liệu từ nguồn (như mảng, danh sách hoặc cơ sở dữ liệu) và ánh xạ dữ liệu đó vào các view riêng lẻ.
   - Adapter có ba phương thức chính:
     - `onCreateViewHolder`: được gọi khi RecyclerView cần một `ViewHolder` mới. Phương thức này tạo và khởi tạo các view để hiển thị các mục dữ liệu.
     - `onBindViewHolder`: được gọi để gán dữ liệu từ nguồn vào `ViewHolder` hiện tại dựa trên vị trí của nó trong danh sách.
     - `getItemCount`: trả về tổng số mục dữ liệu.

2. **RecyclerView.ViewHolder**:
   - `ViewHolder` là một lớp giữ chỗ cho các view của từng mục dữ liệu trong `RecyclerView`.
   - `ViewHolder` giúp tối ưu hóa hiệu suất của `RecyclerView` bằng cách lưu trữ các tham chiếu đến các view con của từng mục dữ liệu, tránh việc tìm kiếm lại các view mỗi khi cần cập nhật dữ liệu.
   
Dưới đây là ví dụ về cách triển khai `Adapter` và `ViewHolder` trong `RecyclerView`:

```java
// MyAdapter.java
public class MyAdapter extends RecyclerView.Adapter<MyAdapter.MyViewHolder> {

    private List<String> dataList;

    // Constructor của Adapter, nhận vào danh sách dữ liệu
    public MyAdapter(List<String> dataList) {
        this.dataList = dataList;
    }

    // Tạo và khởi tạo ViewHolder khi RecyclerView cần một ViewHolder mới
    @Override
    public MyViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        // Sử dụng View Binding để inflate layout
        ItemLayoutBinding binding = ItemLayoutBinding.inflate(LayoutInflater.from(parent.getContext()), parent, false);
        return new MyViewHolder(binding);
    }

    // Gán dữ liệu từ dataList vào ViewHolder
    @Override
    public void onBindViewHolder(MyViewHolder holder, int position) {
        // Lấy dữ liệu dựa trên vị trí của mục
        String data = dataList.get(position);
        holder.binding.textView.setText(data);  // Gán dữ liệu cho TextView thông qua binding
    }

    // Trả về số lượng mục dữ liệu
    @Override
    public int getItemCount() {
        return dataList.size();
    }

    // Lớp ViewHolder sử dụng View Binding để giữ các view cho từng mục
    public class MyViewHolder extends RecyclerView.ViewHolder {
        private final ItemLayoutBinding binding;

        public MyViewHolder(ItemLayoutBinding binding) {
            super(binding.getRoot());
            this.binding = binding;
        }
    }
}
```

### Giải thích:
- Trong ví dụ trên, `MyAdapter` là `Adapter` chịu trách nhiệm gán dữ liệu từ `dataList` vào `RecyclerView`.
- `MyViewHolder` là lớp chứa các view cho từng mục trong `RecyclerView`, chỉ chứa `TextView` để hiển thị chuỗi văn bản từ `dataList`.
- **ItemLayoutBinding**: Khi bật View Binding, một lớp `ItemLayoutBinding` sẽ tự động được tạo dựa trên tên file `item_layout.xml`. Lớp này chứa tất cả các view có `id` trong file layout, giúp ta truy cập trực tiếp vào các view đó.
- **View Binding trong ViewHolder**: Trong `MyViewHolder`, chúng ta lưu giữ một instance của `ItemLayoutBinding`. Khi cần truy cập `TextView` trong layout, ta chỉ cần gọi `binding.textView` thay vì `findViewById`.

### Lợi ích của View Binding:

- **An toàn với lỗi null**: Do binding luôn ánh xạ trực tiếp từ layout, nên các lỗi `NullPointerException` do không tìm thấy view sẽ được giảm thiểu.
- **Gọn gàng và dễ bảo trì**: Code trở nên ngắn gọn và dễ đọc hơn, không cần phải dùng `findViewById`.
