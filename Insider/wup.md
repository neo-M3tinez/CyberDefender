# Insider 

> After Karen started working for 'TAAUSAI,' she began to do some illegal activities inside the company. 'TAAUSAI' hired you as a soc analyst to kick off an investigation on this case.
You acquired a disk image and found that Karen uses Linux OS on her machine. Analyze the disk image of Karen's computer and answer the provided questions.

> Tools:
  FTK Imager

## Q1: What distribution of Linux is being used on this machine?

=> Anwser: kali 

![361150817-0d3f9ee0-3cf3-484a-902f-e8cdc3d99e0f](https://github.com/user-attachments/assets/99f8cd88-f4c3-4f12-b0aa-cf9267c3e198)

+ chứa thông tin boot image là file kali 

## Q2: What is the MD5 hash of the apache access.log?

+ xác định nơi chứa tệp tin apache2 thường nằm ở thư mục /var/log

  ![361150987-124e2b19-839f-4e1c-90ec-54f5be2f270b](https://github.com/user-attachments/assets/03e3a8cb-e536-4518-a8eb-d6a9334d3298)

=> khi export hash list csv ta được thông tin về md5 hash của file này 

![361151090-5df7bd75-6be2-4b4c-b849-71193e3a78dc](https://github.com/user-attachments/assets/dd64b776-9d35-4cca-9a1c-573c8af3666f)

=> Anwser: d41d8cd98f00b204e9800998ecf8427e

## Q3: It is believed that a credential dumping tool was downloaded? What is the file name of the download?

+ đề bài gợi ý là file name download nên có thể nó sẽ nằm ở thư mục download

 ![361151287-5f677bb7-1c62-47be-8d01-45dd9231a4d5](https://github.com/user-attachments/assets/e9848388-75a7-4f3a-9afe-059c945d4a92)


+ mimikazt là 1 công cụ có khả năng khai thác thông tin credentials từ windows

=> Anwser: mimikatz_trunk.zip

## Q4: There was a super-secret file created. What is the absolute path?

+ sau 1 hồi tìm kiếm ta chưa thấy kết quả nào trả về dòng này

+ tuy nhiên chúng ta còn có thể xem được tệp bash_history lưu trữ nhật kí câu lệnh từ người dùng

![361151903-102acd92-4990-4779-89e1-d618b2ceee98](https://github.com/user-attachments/assets/ba7a1589-545b-4a5e-aedf-f02b36217ca8)

=> Anwser : /root/Desktop/SuperSecretFile.txt

## Q5: What program used didyouthinkwedmakeiteasy.jpg during execution?

+ check tiếp tại bash_history

![361152028-677ec0df-594b-40a8-b3dd-b5a61d64a7e0](https://github.com/user-attachments/assets/4de62394-c723-406e-aa50-a9ca5c9a13e5)


=> Anwser : binwalk

## Q6: What is the third goal from the checklist Karen created?

+ thường checklist sẽ hay để ở desktop

![361152341-b45826a1-9502-41bc-9646-3b8f4b15344d](https://github.com/user-attachments/assets/f26e8643-6097-47a1-862c-9b9076cecb7c)


=> Anwser: Profit

## Q7: How many times was apache run?

+ check lại ở Question 2 thì ta có thể thấy access.log của apache2 empty

=> Anwser: 0

## Q8: It is believed this machine was used to attack another. What file proves this?

+ 


## Q9: Within the Documents file path, it is believed that Karen was taunting a fellow computer expert through a bash script. Who was Karen taunting?
