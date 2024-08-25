# Insider 

> After Karen started working for 'TAAUSAI,' she began to do some illegal activities inside the company. 'TAAUSAI' hired you as a soc analyst to kick off an investigation on this case.
You acquired a disk image and found that Karen uses Linux OS on her machine. Analyze the disk image of Karen's computer and answer the provided questions.

> Tools:
  FTK Imager

## Q1: What distribution of Linux is being used on this machine?

=> Anwser: kali 

![image](https://github.com/user-attachments/assets/0d3f9ee0-3cf3-484a-902f-e8cdc3d99e0f)

+ chứa thông tin boot image là file kali 

## Q2: What is the MD5 hash of the apache access.log?

+ xác định nơi chứa tệp tin apache2 thường nằm ở thư mục /var/log

  ![image](https://github.com/user-attachments/assets/124e2b19-839f-4e1c-90ec-54f5be2f270b)

=> khi export hash list csv ta được thông tin về md5 hash của file này 

![image](https://github.com/user-attachments/assets/5df7bd75-6be2-4b4c-b849-71193e3a78dc)

=> Anwser: d41d8cd98f00b204e9800998ecf8427e

## Q3: It is believed that a credential dumping tool was downloaded? What is the file name of the download?

+ đề bài gợi ý là file name download nên có thể nó sẽ nằm ở thư mục download

  ![image](https://github.com/user-attachments/assets/5f677bb7-1c62-47be-8d01-45dd9231a4d5)

+ mimikazt là 1 công cụ có khả năng khai thác thông tin credentials từ windows

=> Anwser: mimikatz_trunk.zip

## Q4: There was a super-secret file created. What is the absolute path?

+ sau 1 hồi tìm kiếm ta chưa thấy kết quả nào trả về dòng này

+ tuy nhiên chúng ta còn có thể xem được tệp bash_history lưu trữ nhật kí câu lệnh từ người dùng

  ![image](https://github.com/user-attachments/assets/102acd92-4990-4779-89e1-d618b2ceee98)

=> Anwser : /root/Desktop/SuperSecretFile.txt

## Q5: What program used didyouthinkwedmakeiteasy.jpg during execution?

+ check tiếp tại bash_history

![image](https://github.com/user-attachments/assets/677ec0df-594b-40a8-b3dd-b5a61d64a7e0)

=> Anwser : binwalk

## Q6: What is the third goal from the checklist Karen created?

+ thường checklist sẽ hay để ở desktop

  ![image](https://github.com/user-attachments/assets/b45826a1-9502-41bc-9646-3b8f4b15344d)

=> Anwser: Profit

## Q7: How many times was apache run?

+ check lại ở Question 2 thì ta có thể thấy access.log của apache2 empty

=> Anwser: 0

## Q8: It is believed this machine was used to attack another. What file proves this?

+ 


## Q9: Within the Documents file path, it is believed that Karen was taunting a fellow computer expert through a bash script. Who was Karen taunting?
