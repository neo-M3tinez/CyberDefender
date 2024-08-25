# NintendoHunt

> You have been hired as a soc analyst to investigate a potential security breach at a company. The company has recently noticed unusual network activity and suspects that there may be a malicious process running on one of their computers. Your task is identifying the malicious process and gathering information about its activity.

> Tools:
    Volatility 2

## Q1: What is the process ID of the currently running malicious process?

+ ta sẽ sử dụng tool volatility 3 để phân tích file

+ sử dụng plugin pslist để check list process của file: ``vol -f memdump.mem windows.pslist > pslist.txt ``

+ check file process trên txt

![361231648-bcf39b8d-263b-4b8f-94e0-3a315da4b6ea](https://github.com/user-attachments/assets/58e04de0-051f-4d24-aa1a-5e5f9891f688)


=> ta thấy có 1 process bất thường là ở svchost.exe.exe

> svchost.exe (Service Host) là một tiến trình hệ thống quan trọng trong hệ điều hành Windows. Tiến trình này hoạt động như một container để lưu trữ và quản lý các dịch vụ hệ thống chạy từ các thư viện liên kết động (DLL)

> svchost.exe.exe là một dấu hiệu bất thường và rất có thể là một phần mềm độc hại (malware).

+ grep các tiến trình dựa trên ppid 4824

![361243207-bb79d5a6-138c-493b-bfd1-a9ba055d59e9](https://github.com/user-attachments/assets/563ea25e-5045-4456-9a6e-776a66524672)


=> ta có thể dump từng pid liên quan để check 

![361242908-d518f071-190f-45d5-a080-bf100a9d8245](https://github.com/user-attachments/assets/f66ff7d6-0362-4e2b-b4c9-7ed03b6f6d6b)


+ dump tiến trình pid 8560 :  ``vol -f memdump.mem windows.pslist --pid 8560 --``

=> check md5sum của file và check trên virus total ta thấy có thông tin 1 file sublime_text.exe có malware 

=> Anwser: 8560

## Q2: What is the md5 hash hidden in the malicious process memory?

+ Câu hỏi là mã md5 bị giấu trong memory của process nên mình đã sử dụng plugin memmap để dump ra file chứa memory của process 8560

+ lúc trên mình dùng pslist dump thì chỉ thu được các thông tin như là tên process, pid, parent pid, còn memmap dump sẽ trả về những thông tin về bộ nhớ của các process

vol -f memdump.mem windows.memmap.Memmap --pid 8560 --dump

Đoạn này mình dùng volatility trên powershell vì mình thấy chạy nhanh hơn và mình phải chạy đi chạy lại nhiều lần để thử câu lệnh

Sau khi dump ra thì mình được một file dump pid.8560.dmp, mình dùng strings trong linux để đọc file này, sau khi lướt một lúc thì mình tìm thấy một thông tin khả nghi là t.h.e fl.ag.is và một chuỗi có vẻ là base64, mình decode thì ra chuỗi 3a19697f29095bc289a96e4504679680, mình dùng cipher identifier thì đúng là md5


![361243705-89bc6b48-587f-458d-bb6e-a5a6f0902f1f](https://github.com/user-attachments/assets/058618b7-6f11-4fd3-98dc-0d8a2ea9ce7b)


=> Anwser: 3a19697f29095bc289a96e4504679680

## Q3: What is the process name of the malicious process parent?

+ như Q1 ta đã tìm thấy ppid 4824 là explorer.exe

=> Anwser: explorer.exe

