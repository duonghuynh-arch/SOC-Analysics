#                    Cyber Defender _ Network Easy 2

##                            Before the ransomware deployment, the attackers established initial access through a misconfigured CI/CD server running in a Docker container within Wowza's development network. Security monitoring detected unusual outbound connections from the container subnet to a suspicious external IP address. A packet capture was initiated automatically but was terminated when the attacker discovered and killed the monitoring process. Your task is to analyze this network traffic to understand how the attackers gained their initial foothold and moved laterally within the containerized environment.
- "Trước khi triển khai mã độc tống tiền, tin tặc đã thiết lập quyền truy cập ban đầu thông qua một máy chủ CI/CD cấu hình sai đang chạy trong một container Docker bên trong mạng phát triển của Wowza. Hệ thống giám sát an ninh đã phát hiện các kết nối đi ra bất thường từ mạng con container đến một địa chỉ IP bên ngoài đáng ngờ. Quá trình bắt gói tin được khởi chạy tự động nhưng đã bị chấm dứt khi kẻ tấn công phát hiện và tắt tiến trình giám sát. Nhiệm vụ của bạn là phân tích lưu lượng mạng này để hiểu cách tin tặc giành được quyền truy cập ban đầu và di chuyển ngang trong môi trường container hóa"
### 1. Initial Access & Reconnaissance
- Security monitoring flagged suspicious HTTP traffic targeting the container subnet. Identifying the first system that received malicious requests is essential for establishing the initial point of compromise. What is the IP address of the first compromised system?Hệ thống giám sát an ninh đã phát hiện lưu lượng HTTP đáng ngờ nhắm vào mạng con container. Việc xác định hệ thống đầu tiên nhận được các yêu cầu độc hại là rất quan trọng để xác định điểm xâm nhập ban đầu. Địa chỉ IP của hệ thống bị xâm nhập đầu tiên là gì?
- Identifying attacker IP is critical for threat intelligence and blocking future connections. What is the attacker's command and control (C2) IP address?Việc xác định địa chỉ IP của kẻ tấn công rất quan trọng đối với việc thu thập thông tin tình báo về mối đe dọa và chặn các kết nối trong tương lai. Địa chỉ IP máy chủ điều khiển (C2) của kẻ tấn công là gì?
- What web application and version was exploited for initial access?Ứng dụng web và phiên bản nào đã được sử dụng để truy cập lần đầu?
- ![Initial Access & Reconnaisanse](picture/Initial%20Access%20&%20Reconnaisanse_q3.jpg)
#### Q4
-  Before fully exploiting a vulnerability, attackers often perform a proof-of-concept test to confirm code execution capabilities. What file did the attacker initially read to test the vulnerability? (Provide full path)Trước khi khai thác triệt để lỗ hổng, kẻ tấn công thường thực hiện kiểm thử chứng minh khái niệm (proof-of-concept) để xác nhận khả năng thực thi mã. Kẻ tấn công đã đọc tệp nào ban đầu để kiểm tra lỗ hổng? (Cung cấp đường dẫn đầy đủ)
- Đây là bước "Proof of Concept" — attacker cần xác nhận lệnh thực thi được trước khi triển khai payload nặng hơn. /etc/passwd là file readable bởi mọi user, nên nếu output trả về list user → RCE confirmed!
- /etc/passed                                                                                                                                                                                                                           
#### Q5: 
Identifying this vulnerable endpoint helps understand the attack vector and informs remediation efforts. What is the URI path of the vulnerable endpoint exploited by the attacker?Việc xác định điểm cuối dễ bị tổn thương này giúp hiểu rõ phương thức tấn công và định hướng các nỗ lực khắc phục. Đường dẫn URI của điểm cuối dễ bị tổn thương mà kẻ tấn công khai thác là gì?
Đáp án: POST /script HTTP/1.1
Host: 172.16.10.10:8080
Jenkins Script Console (/script) là endpoint cho phép chạy Groovy code tùy ý trên server. Nếu không được bảo vệ hoặc bị brute-force credentials → attacker có full RCE ngay lập tức. Đây là một trong những attack vector phổ biến nhất với Jenkins exposed ra internet.



### 2. Execution
- After confirming code execution, the attacker established a reverse shell connection back to their C2 infrastructure. What port number did the attacker use for the initial reverse shell listener?Sau khi xác nhận việc thực thi mã, kẻ tấn công đã thiết lập kết nối shell severse trở lại cơ sở hạ tầng C2 của chúng. Kẻ tấn công đã sử dụng số cổng nào cho trình lắng nghe shell reverse ban đầu?
- http.request.method == POST
- ip.src == 172.16.10.10 && tcp.flags.syn == 1
- Tìm victim ra C2  "outbound connection"
ip.dst == 185.220.101.50 && ip.src == 172.16.10.10
- Đáp án: 4444
Nhìn vào packet detail:

Source Port: 44172 (victim)
Destination Port: 4444 (C2 attacker)

Traffic 172.16.10.10 → 185.220.101.50 trên port 4444 — đây là reverse shell connection!
Và trong hex dump còn thấy rõ shell output: /bin/sh, /bin/bash, /usr/bin
- Port 4444 là port reverse shell mặc định của Metasploit (msfvenom). Đây là dấu hiệu attacker dùng Metasploit framework hoặc copy payload mà không đổi port
### 3. Discovery
- Once inside the compromised container, the attacker uploaded a well-known enumeration script to identify privilege escalation vectors. What privilege escalation enumeration script did the attacker download after gaining shell access?Sau khi xâm nhập vào container bị chiếm quyền, kẻ tấn công đã tải lên một kịch bản liệt kê quen thuộc để xác định các phương thức leo thang đặc quyền. Vậy kẻ tấn công đã tải xuống kịch bản liệt kê leo thang đặc quyền nào sau khi giành được quyền truy cập shell?
### 4. Credential Access
- What file did the attacker read to obtain lateral movement credentials? (Provide full path)Kẻ tấn công đã đọc tập tin nào để lấy được thông tin xác thực di chuyển ngang? (Cung cấp đường dẫn đầy đủ)






### 5. Lateral Movement
### 6. Privilege Escalation
### 7. Defense Evasion - Container Escape
### 8. Persistence & Impact
### 9. Defense Evasion - Anti-Forensics
