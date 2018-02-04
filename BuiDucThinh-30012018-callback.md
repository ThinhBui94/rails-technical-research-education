# Callback trong rails.
### 1. Callback hoạt động như thế nào?
**Cơ chế hoạt động của callback:**
- Về cơ bản để một call_back được thực thi có 2 bước là: **Thiết lập callback** và **gọi callback**:
  - **Thiết lập callback:**
    - Các hàm callback (vd: ```before_create```, ```after_save```,...) được tạo ra bởi phương thức ```define_callbacks```.
    - Các hàm callback sau khi được tạo ra sẽ có phương thức ```set_callbacks``` bên trong, ```set_callbacks``` nhận vào các tham số có thể là instance methods, procs, ... --> đây chính là các phương thức mà ta tạo ra để được callback vào một thời điểm cụ thể.
  - **Chạy callback:**
    - Sau khi callback được thiết lập ( có thể coi là callback đã được install) và sẵn sàng để được gọi.
    - Bên trong 1 phươg thức có hỗ trợ callback (vd như: ```save```, ```create```, ```update```,...) có 1 hàm là ```run_callback```, hàm này có nhiệm vụ gọi tất cả các callback đã được thiết lập trước đó và chạy.
    - Sẽ có 3 loại callback được hỗ trợ là before, after và around, được chạy theo thứ tự before -- around -- phương thức chính -- around - after
### 2. Callback nào call transaction, callback nào không call?
* **Transaction là gì?**
  - Transaction là 1 block code bao ngoài các câu lệnh sql (các lệnh tương tác với db) có nhiệm vụ đảm bảo chắc chắn rằng database chỉ được thay đổi khi tất cả các câu lệnh trong block đều thực hiện thành công. Nếu có exception được raise ở bất kì một lệnh nào thì db sẽ rollback lại trạng thái trước đó.
  - Mặc định các action ```save``` và ```destroy``` được protect bởi transaction

* **Callback nào calltransacion?**
  - Vì ```save``` và ```destroy``` được protect bới transaction nên tất cả những lệnh được thực thi bởi callbacks và validation cũng đều nắm trong transaction, do đó có thể sử dụng callback và validation cho raise exception để thực hiện callback
--> Tất cả các callback thông thường ```before```, ```around```, ```after``` với ```save```, ```destroy```, ```create``` đều có thể dùng để call transaction

* **Callback nào không dùng để gọi transaction?**
  - Bởi vì các callback thông thường đều nằm trong transaction nên ko thấy được những thay đổi với database nên cần phải sử dụng các callback đặc biết hơn để thực hiện callback
  --> ```after_commit```, ```after_create_commit```, ```after_destroy_commit```, ```after_rollback```, ```after_update_commit```.
### 3. Callback condition.
  - Sử dụng ```if``` và ```unless``` khi muốn callback chỉ được thực hiện với những điều kiện cụ thể.
  - Có thể kết hợp kết hợp if với unless.
