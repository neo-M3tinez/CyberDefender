# PsExec Hunt 

> Our Intrusion Detection System (IDS) has raised an alert, indicating suspicious lateral movement activity involving the use of PsExec. To effectively respond to this incident, your role as a SOC Analyst is to analyze the captured network traffic stored in a PCAP file.


> Tools:
   Wireshark

## Q1: In order to effectively trace the attacker's activities within our network, can you determine the IP address of the machine where the attacker initially gained access?

+ liệt kê tất ip đang được sử dụng trên traffic search ip có số lần truy cập nhiều nhất trong đó ta thấy

> Tìm kiếm địa chỉ IP khả nghi: Nếu bạn nghi ngờ có một địa chỉ IP đang thực hiện hành vi bất thường, bạn có thể sắp xếp theo cột "Count" để xác định các địa chỉ IP nào có số lượng gói tin lớn.

> Xác định điểm nóng: Các địa chỉ IP có số lượng gói tin cao có thể là các điểm nóng trong mạng hoặc thiết bị đang chịu tải cao.

![361099808-a960cf01-0e0b-4ccd-8dd9-38edfa90032d](https://github.com/user-attachments/assets/8b004e5e-bdbf-4b0d-b2d9-7e103b482d4d)


=> địa chỉ ip 10.0.0.130 đang có số lượng gói tin lớn nên khả năng là máy attacker 

=> Anwser: 10.0.0.130


## Q2: To fully comprehend the extent of the breach, can you determine the machine's hostname to which the attacker first pivoted?

+ sau 1 lần check các gói tin thì vẫn chưa tìm được thông tin gì

+ nhưng nhìn vào gói tin smb2 ta có thể thấy NTLMSSP (NT LAN Manager Security Support Provider) là giao thức xác thực của Windows, sử dụng ba bước (Negotiate, Challenge, Authenticate) để xác thực người dùng

  ![361159038-6640f3cc-ae28-40b6-a565-5f6a87010d01](https://github.com/user-attachments/assets/90d11bcc-c7a6-483a-953e-fd5bbd9b7b42)

=> Anwser: SALES-PC

## Q3: After identifying the initial entry point, it's crucial to understand how far the attacker has moved laterally within our network. Knowing the username of the account the attacker used for authentication will give us insights into the extent of the breach. What is the username utilized by the attacker for authentication?

+ tiếp tục khám phá ta tìm thấy được user trong gói này là username ssales

 ![361159746-fdeee967-e62d-473d-bf80-0e89f3ec4e6e](https://github.com/user-attachments/assets/a579de99-63f5-4b01-873a-be8c1ab6543c)


=> username: ssales 

## Q4: After figuring out how the attacker moved within our network, we need to know what they did on the target machine. What's the name of the service executable the attacker set up on the target?

+ sau khi attacker di chuyển qua mạng thì họ tiến hành tạo 1 file thực thi để attack vào target machine

![361160419-3e2938e8-edd3-4369-8b4c-59c16c7f8c14](https://github.com/user-attachments/assets/2ea6f5fe-43f2-4555-a29c-d50e730ea4b4)


=> Anwser: PSEXESVC.exe

## Q5: We need to know how the attacker installed the service on the compromised machine to understand the attacker's lateral movement tactics. This can help identify other affected systems. Which network share was used by PsExec to install the service on the target machine?

+ network mà được dùng PsExec để truy cập từ xa là ADMIN$ hay còn có kỹ thuật remote admin

+ Psexec là công cụ quản lý do admin sử dụng để trỏ tới C:/ để kết nối share file hay có thể control hệ thống để thuận tiện cho việc quản lí

=> Anwser: ADMIN$

## Q6: We must identify the network share used to communicate between the two machines. Which network share did PsExec use for communication?

+ Quay lại phân tích ở Câu hỏi 1, chúng tôi phát hiện ra rằng 10.0.0.130 đang tấn công 10.0.0.133 thông qua chia sẻ tệp mạng IPC.

=> Anwser:IPC

## Q7: Now that we have a clearer picture of the attacker's activities on the compromised machine, it's important to identify any further lateral movement. What is the machine's hostname to which the attacker attempted to pivot within our network?

+ conversations xác định điểm có lưu lương packet có số lượng nhiều thứ 2 trong traffic

 ![361161757-a134fd33-ef75-482c-8f30-5a7b9139d642](https://github.com/user-attachments/assets/5862943b-ba4e-4aa5-ab41-d973cc1fbecf)


+ LLMNR (Link-Local Multicast Name Resolution) là một giao thức mạng được sử dụng để giải quyết tên máy tính trong một mạng cục bộ (link-local) mà không cần phải sử dụng DNS (Domain Name System)

![361161846-ea8ad27b-fdf7-4c74-b8e6-9b4aef20e4b1](https://github.com/user-attachments/assets/07c461ba-aff1-4033-8bec-d1b04b53d1e2)


=> Anwser: Marketing-PC


