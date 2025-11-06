Bài tập 3   : môn Phát triển ứng dụng trên nền web
Giảng viên  : Đỗ Duy Cốp
Lớp học phần: 58KTPM
Ngày giao   : 2025-10-24 13:50
Hạn nộp     : 2025-11-05 00:00
--------------------------------------------------
Yêu cầu     : LẬP TRÌNH ỨNG DỤNG WEB trên nền linux
1. Cài đặt môi trường linux: SV chọn 1 trong các phương án
 - enable wsl: cài đặt docker desktop
 - enable wsl: cài đặt ubuntu
 - sử dụng Hyper-V: cài đặt ubuntu
 - sử dụng VMware : cài đặt ubuntu
 - sử dụng Virtual Box: cài đặt ubuntu
2. Cài đặt Docker (nếu dùng docker desktop trên windows thì nó có ngay)
3. Sử dụng 1 file docker-compose.yml để cài đặt các docker container sau: 
   mariadb (3306), phpmyadmin (8080), nodered/node-red (1880), influxdb (8086), grafana/grafana (3000), nginx (80,443)
4. Lập trình web frontend+backend:
 SV chọn 1 trong các web sau:
 4.1 Web thương mại điện tử
 - Tạo web dạng Single Page Application (SPA), chỉ gồm 1 file index.html, toàn bộ giao diện do javascript sinh động.
 - Có tính năng login, lưu phiên đăng nhập vào cookie và session
   Thông tin login lưu trong cơ sở dữ liệu của mariadb, được dev quản trị bằng phpmyadmin, yêu cầu sử dụng mã hoá khi gửi login.
   Chỉ cần login 1 lần, bao giờ logout thì mới phải login lại.
 - Có tính năng liệt kê các sản phẩm bán chạy ra trang chủ
 - Có tính năng liệt kê các nhóm sản phẩm
 - Có tính năng liệt kê sản phẩm theo nhóm
 - Có tính năng tìm kiếm sản phẩm
 - Có tính năng chọn sản phẩm (đưa sản phẩm vào giỏ hàng, thay đổi số lượng sản phẩm trong giỏ, cập nhật tổng tiền)
 - Có tính năng đặt hàng, nhập thông tin giao hàng => được 1 đơn hàng.
 - Có tính năng dành cho admin: Thống kê xem có bao nhiêu đơn hàng, call để xác nhận và cập nhật thông tin đơn hàng. chuyển cho bộ phận đóng gói, gửi bưu điện, cập nhật mã COD, tình trạng giao hàng, huỷ hàng,...
 - Có tính năng dành cho admin: biểu đồ thống kê số lượng mặt hàng bán được trong từng ngày. (sử dụng grafana)
 - backend: sử dụng nodered xử lý request gửi lên từ javascript, phản hồi về json.
 4.2 Web IOT: Giám sát dữ liệu IOT.
 - Tạo web dạng Single Page Application (SPA), chỉ gồm 1 file index.html, toàn bộ giao diện do javascript sinh động.
 - Có tính năng login, lưu phiên đăng nhập vào cookie và session
   Thông tin login lưu trong cơ sở dữ liệu của mariadb, được dev quản trị bằng phpmyadmin, yêu cầu sử dụng mã hoá khi gửi login.
   Chỉ cần login 1 lần, bao giờ logout thì mới phải login lại.
 - hiển thị giá trị mới nhất của các thông số đang giám sát, khi click vào thì hiển thị đồ thị lịch sử quá trình thay đổi (gọi grafana iframe để hiển thị)
 - backend: Sử dụng nodered để đọc dữ liệu từ các cảm biến (có thể dùng api online để lấy dữ liệu theo giời gian thực), 
   nodered sẽ lưu dữ liệu mới nhất (dạng update) vào cơ sở dữ liệu mariadb (sử dụng phpmyadmin để tạp table và quản trị lần đầu)
   nodered sẽ lưu dữ liệu (insert) vào influxdb để lưu giá trị lịch sử, để cho grafana dùng để hiển thị biểu đồ.
5. Nginx làm web-server
 - Cấu hình nginx để chạy được website qua url http://fullname.com  (thay fullname bằng chuỗi ko dấu viết liền tên của bạn)
 - Cấu hình nginx để http://fullname.com/nodered truy cập vào nodered qua cổng 80, (dù nodered đang chạy ở port 1880)
 - Cấu hình nginx để http://fullname.com/grafana truy cập vào grafana qua cổng 80, (dù grafana đang chạy ở port 3000)
-------------------------------------------------------------------------------------


# Bài Làm: 
1. Cài đặt môi trường linux: em sử dụng phương pháp enable wsl để cài đặt ubuntu và docker:<br>
- Dùng lệnh tải wsl về trên CMD:<br>
  <img width="573" height="317" alt="image" src="https://github.com/user-attachments/assets/7c70f660-efbe-4b22-a217-2c421c1cac8f" /><br>
  <img width="1067" height="626" alt="image" src="https://github.com/user-attachments/assets/3c18df99-a81e-425c-b74b-8b755f9bd4f3" /><br>
- Vào turn windows features on or off và tích vào 2 phần sau đó ok và khởi động lại máy: <br>
<img width="558" height="496" alt="image" src="https://github.com/user-attachments/assets/0335ee02-9099-4fe8-bea8-8bd420db7e0a" /><br>

- Sau khi khởi động lại xong, vào CMD dùng lệnh install để tải Ubuntu về: <br>
<img width="696" height="332" alt="image" src="https://github.com/user-attachments/assets/ae1dd1b3-3831-477e-9daf-6fc82941fc54" /><br>
<br>
- Sau khi cài đặt xong Ubuntu thì cấu hình user và password<br>
  <img width="885" height="290" alt="image" src="https://github.com/user-attachments/assets/563bf8e6-5182-47a9-9e25-cf21a65f01f3" /><br>

  
2. Cài đặt Docker:<br>
- Tải xuống và cài đặt docker desktop qua đường link: https://www.docker.com/products/docker-desktop/<br>
- Chọn tải xuống AMD64:<br>
  <img width="1606" height="968" alt="image" src="https://github.com/user-attachments/assets/93b3d653-6a3a-4a7d-9f8e-ba390ef485cc" /><br>

- Mở file đã tải về và thực hiện cài dặt:<br>
  <img width="737" height="509" alt="image" src="https://github.com/user-attachments/assets/38fde097-8a1d-45fd-bec3-eb62cfff5351" /><br>
  <img width="1704" height="926" alt="image" src="https://github.com/user-attachments/assets/4ed6a612-0a6d-4680-b5b5-57a59c3696b6" /><br>

3. Sử dụng 1 file docker-compose.yml để cài đặt các docker container sau: <br>
   mariadb (3306), phpmyadmin (8080), nodered/node-red (1880), influxdb (8086), grafana/grafana (3000), nginx (80,443):<br>

   - Tạo chứa file docker-compose.yml sau đó mở ra bằng visual code:<br>
    <img width="420" height="354" alt="image" src="https://github.com/user-attachments/assets/fc2451ea-a70b-4238-9d10-7f840bd983b3" /><br>

  - Code cho file yml và chạy để tải xuống các container về docker desktop:<br>
    <img width="1060" height="613" alt="image" src="https://github.com/user-attachments/assets/76e8bf9a-6908-4c5d-9941-0fa703a72163" /><br>
  

   - Kiểm tra trên Docker Desktop:<br>
     <img width="1208" height="422" alt="image" src="https://github.com/user-attachments/assets/88645a79-2a63-4580-b944-60132ac4e091" /><br>

  - Truy cập thử các link:<br>
    <img width="1422" height="644" alt="image" src="https://github.com/user-attachments/assets/d3561847-21fe-4b29-b8b9-22a71c99a0e4" /><br>
<img width="1234" height="787" alt="image" src="https://github.com/user-attachments/assets/81fabc0e-f7a0-40d5-9fbf-bea34ed6610d" /><br>
<img width="1406" height="682" alt="image" src="https://github.com/user-attachments/assets/26db81db-678e-4b9f-807c-20ec9c664d24" /><br>
<img width="1596" height="774" alt="image" src="https://github.com/user-attachments/assets/a2e9fe77-dee7-4318-bc46-3fc34d49007e" /><br>
4. Lập trình web frontend+backend:<br>
 4.1 Web thương mại điện tử:<br><br>
    - Tạo cơ sở dữ liệu:<br>
      <img width="1145" height="508" alt="image" src="https://github.com/user-attachments/assets/d739225c-4274-416d-9012-e5a34c2f27f0" /><br>

    - Thiết kế nodered kết nối tới cơ sở dữ liệu:<br>
      <img width="1458" height="994" alt="image" src="https://github.com/user-attachments/assets/9fd1330a-04e9-4b28-aa6c-1766a3d20d2d" /><br>

  - Thiết kế frontend kết nối tới backend:<br>
  - Cấu hình thêm 1 file conf và 1 file html với cấu trúc dự án như sau để nginx trỏ tới file html:<br>
    <img width="1151" height="543" alt="image" src="https://github.com/user-attachments/assets/b8bc4fda-dd24-4fb5-869d-5b2d8fc4e8c8" /><br>

- Thiết kế front end và test kết nối tới backend:<br>
  <img width="1339" height="908" alt="image" src="https://github.com/user-attachments/assets/d7c0dbf3-5f9d-48ac-a758-428d0111fcd5" /><br>
<img width="877" height="420" alt="image" src="https://github.com/user-attachments/assets/94490ebb-e002-45ca-9367-346ccf5f80a2" /><br>

- Đăng nhập thành công từ lệnh post api gửi đến bảng user trong database:<br>
  <img width="893" height="790" alt="image" src="https://github.com/user-attachments/assets/0d9c598c-9dd0-4d62-ac46-6a1c8507297a" /><br>
<img width="1742" height="911" alt="image" src="https://github.com/user-attachments/assets/34ec96ac-764e-473d-b5fc-57b185478b35" /><br>

5.  Nginx làm web-server:<br>
   - Mở: C:\Windows\System32\drivers\etc\hosts và cấu hình thêm dòng này vào cuối 127.0.0.1   phamtrunghieu.com<br>
 - Sửa file conf ta được kết quả:<br>
   <img width="1566" height="929" alt="image" src="https://github.com/user-attachments/assets/020c87c3-3be4-4751-9602-55d07e8d9963" /><br>
==> Front end gọi backend qua fetch API gửi tới nodered, sau đó api đó sẽ là httpin và được dẫn vào backend mariadb (database)<br>
