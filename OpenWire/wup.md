# OpenWire 

> During your shift as a tier-2 SOC analyst, you receive an escalation from a tier-1 analyst regarding a public-facing server. This server has been flagged for making outbound connections to multiple suspicious IPs. In response, you initiate the standard incident response protocol, which includes isolating the server from the network to prevent potential lateral movement or data exfiltration and obtaining a packet capture from the NSM utility for analysis. Your task is to analyze the pcap and assess for signs of malicious activity.

> Tools: Wireshark


## Q1: By identifying the C2 IP, we can block traffic to and from this IP, helping to contain the breach and prevent further data exfiltration or command execution. Can you provide the IP of the C2 server that communicated with our server?

+ Recon: phân tích những host tham gia traffic mạng => conversations

  ![360735827-4cda4abe-2dc3-4036-b363-67396b89cf33](https://github.com/user-attachments/assets/62331667-2f1e-44c8-9d5f-97a965566db9)


=> conversations tại ip 142.190.21.92 -> 134.209.197.3 đang lưu lượng gói tin đang là nhiều nhất 

![360744015-4b684627-7985-4a61-826f-89a2bbee2583](https://github.com/user-attachments/assets/abf25a52-56dd-4e5e-a372-634c78fa1fb6)


+ following conversations của 2 ip ta check được nó đang tiếp lập kết nối đến openwire protocol

> openwire protocol

> Mục Đích: OpenWire được thiết kế để truyền tải tin nhắn giữa client và message broker (như Apache ActiveMQ) trong môi trường phân phối tin nhắn.

> Định Dạng Nhị Phân: Giao thức này sử dụng định dạng nhị phân, giúp tối ưu hóa băng thông và hiệu suất so với các giao thức văn bản.

> Tính Tương Thích: OpenWire thường được sử dụng trong các ứng dụng Java, đặc biệt là với JMS (Java Message Service).

=> Mặc dù OpenWire chủ yếu là một giao thức nhắn tin được sử dụng trong các hệ thống message broker như Apache ActiveMQ, nếu hệ thống message broker bị xâm nhập, nó có thể bị kẻ tấn công sử dụng như một cơ chế C2. Ví dụ, kẻ tấn công có thể khai thác một hệ thống message broker để gửi lệnh từ xa đến các máy bị nhiễm hoặc bot.

=> ta có thể nhận định ip 142.190.21.92 là ip C2 đang connect port 61616 

=> Anwser: 142.190.21.92 

## Q2: Initial entry points are critical to trace back the attack vector. What is the port number of the service the adversary exploited?

+ entry point của attacker để truy tìm điểm khai thác của máy nạn nhân => port 61616

+ Anwser: 61616

## Q3: Following up on the previous question, what is the name of the service found to be vulnerable?

+ như bài trước server found vulnerbility ở port 61616 dịch truyền tin nhắn giữa client và message broker  như Apache ActiveMQ là bên tạo ra giao thức Openwire 

![360793703-0dee008a-52c5-4368-a92a-d68a5971c571](https://github.com/user-attachments/assets/9e5d8445-218b-47ec-886e-d74e0b3b4517)


=> Anwser: Apache ActiveMQ

## Q4: The attacker's infrastructure often involves multiple components. What is the IP of the second C2 server?

+ xác định C2 server tìm attack ip host 2 ta kiểm tra được ip 128.199.52.72 

![360796579-0ddadeb9-276b-4456-8cb7-f70c8063eba5](https://github.com/user-attachments/assets/83c4b3f2-185a-4592-8266-130390fbb363)

=> như hình trên có vẻ ip attacker giao tiếp với ip victim để connect qua port 80 http để truyền 1 file mã độc gì đó có thể là Docker file 

=> Anwser: 128.199.52.72

## Q5: Attackers usually leave traces on the disk. What is the name of the reverse shell executable dropped on the server?

+ sau khi check qua giao thức truyền file http ta thấy ip 128.199.52.72 đang gửi 1 file docker => có thể là file reverse shell 

![360796339-9232c172-b82a-4adf-b8ce-fffb10a92ab3](https://github.com/user-attachments/assets/b007c9cd-a80a-4129-8e57-3e16e634fdad)


=> sau khi download về ta check virus total đây là 1 file shell code name docker chắc được generate ra từ metasploit 

=> Anwser: docker 

## Q6: What Java class was invoked by the XML file to run the exploit?

+ class java được gọi bởi tệp XML để chạy mã khai thác là class = java.lang.ProcessBuilder

![360935220-6d1ad4dd-3b07-41e5-a7d2-00ea51a9c35a](https://github.com/user-attachments/assets/13888f02-356a-445f-853e-a664bf20ec14)


=> Anwser: java.lang.ProcessBuilder

## Q7: To better understand the specific security flaw exploited, can you identify the CVE identifier associated with this vulnerability?

* Câu hỏi có đề cập đến CVE nên mình đã search về CVE thì nó chính là viết tắt của Common Vulnerabilities and Exposures, ngắn gọn thì đây là một danh sách các lỗ hổng bảo mật
* Mình đã dựa vào các đặc điểm của đoạn mã để search về loại CVE này

![360936304-8906386e-8034-4659-9889-0abf15e9efc6](https://github.com/user-attachments/assets/c3b55d8e-1bb7-460e-97a5-90c3e2a80a50)

* Mình đã tìm thấy github [link](https://github.com/rootsecdev/CVE-2023-46604 "Link Github") và [link](https://github.com/X1r0z/ActiveMQ-RCE "ActiveMQ-RCE")
* Và mình đã tìm thấy lỗ hổng đó là **CVE-2023-46604** RCE reverse shell Apache ActiveMQ
* Đây là một số link mình tìm thấy trên github giải thích về CVE này

  https://exp10it.io/2023/10/apache-activemq-版本-5.18.3-rce-分析

  https://attackerkb.com/topics/IHsgZDE3tS/cve-2023-46604/rapid7-analysis


## Q8: What is the vulnerable Java method and class that allows an attacker to run arbitrary code? (Format: Class.Method)
### Solution 
* Mình đã vào đọc các link ở trong github đó và tìm thấy thông tin class java được sử dụng

![360935938-3f6036e2-de6f-496b-b3e5-2cac88c155a7](https://github.com/user-attachments/assets/a4055599-938f-4d92-9d86-b18764bd06ab)


=> Answer: BaseDataStreamMarshaller.createThrowable
