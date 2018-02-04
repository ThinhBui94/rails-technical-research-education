# Tìm hiểu Gem Delayed::Job
### 1. Delayed::Job là gì?
- Delayed::Job là một framework xử lý background processing hoạt động dựa trên sự tồn tại của bảng 'delayed_job' trong db để theo dõi hoạt đông của các job ( dựa vào các trường ```priority```, ```attempts```, ```handler```, ```failed_at``` ...).
- Delayed::Job có 3 cách để thêm job vào hàng đợi:
   +  Gọi ```.delay.method(params)``` với object bất kì
   +  Sử dụng ```#handle_asynchronously``` sau khi khai báo một phương thức, sử dụng phương thức này khi muốn job luôn chạy background
   + Tạo custom job và xếp vào hàng đợi bằng ```Delayed::Job.enqueue```

### 2. Cơ chế gọi job để run
- Sau khi các job được thêm vào hàng đợi để run được job cần chạy : ```RAILS_ENV=production bin/delayed_job start``` ( hoặc các thông số thiết lập khác với từng mục đích cụ thể), lệnh này sẽ start worker ( được hiểu là process), worker access vào database và đồng bộ với thời gian, mỗi 1 worker sẽ kiểm tra database 5 giây 1 lần ( 5s do SLEEP_DELAY thiếp lập, có thể điều chỉnh được thời gian này). Worker sẽ check và chạy các job khi đến thời điểm.
- Các job nếu fail sẽ chạy lại với số lần attemp đã thiết lập, tối đa là 25 lần, sau 25 lần job có thể bị xóa khởi db hoặc vẫn lưu lại với thời gian failed_at
- Có thể thiết lập nhiều worker cho 1 job hoặc mỗi job một worker
- Có thể auto start Delayed::job với gem thứ 3, mặc định delayed::job không cho tự động chạy.
