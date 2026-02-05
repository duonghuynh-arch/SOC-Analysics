#                    SOC DAY 9
##                             Splunk
                           
### 1. Các phương pháp phát hiện                  
#### 1.1. Rule - based Detection (Phát hiện dựa trên luật)


- Phát hiện dựa trên hành vi đã biết và mẫu tấn công có sẵn (known patterns)
- SOC phát hiện nhanh, chính xác nếu rule tốt
- Phù hợp các attack quen thuộc SQL injection, brute force, scan, malware known IOC, ....
##### Trong thực tế :
- Đây là cốt yếu của SIEM
- Dựa trên Signature, regex, threshold, correlation
##### Example:SQL Injection Attempt
index=finova_web uri="* OR * 1=1 *"
| stats count by src_ip
| where count > 5
#### 1.2. Anomaly-based Detection (Phát hiện bất thường)
- Tập trung vào hành vi hiếm or khác baseline
- Dùng để phát hiện attack mới, chưa có Signature
- Mạnh nhưng khó vân hành vì dễ sinh noise
##### Example: login at Unusual Hours
 index=finova_auth action=login_success
 | eval hour=strftime(_time, " %H ")
 | where hour < 6 OR hour > 22
| stats count by user, src_ip, hour
- User đăng nhập ngoài h hành chính 
- Risk signal , cần correlate thêm: IP, endpoint, User privilege
- Đây là kiểu phát hiện dựa trên hành vi thường dùng cho: Account compromise, Inside threat, APT stealthy
#### 1.3. Operation Considerations - Vân hành thực tế 
- Rule-Based: nhẹ, hiệu quả, ít tốn tài nguyên 
- Anomaly-based: tốn công tunnning, tốn resource

#### 2. Detection Lifecycle - Vòng đời một detection
##### 2.1. Business Concern - Why
- Vấn đề kinh doanh 
- Vấn đề an ninh 
##### 2.2. 
##### 2.3. 
##### 2.4. 
##### 2.5. 
#### 3. Common Pitfall in Dectetion - Những lỗi SOC thường gặp 
#### 4. Whitelisting & Threathold - SPL Examples
#### 5. Why Writing Detection Is Still Hard
#### 6. Detection Principles Overview
#### 7. Những điểm nút quan trọng
#### LAB Splunk 
##### USE CASE 1 – WEB ABUSE / SQL INJECTION
- Bối cảnh:
Team Web của FINOVA báo cáo:

 “Website vẫn hoạt động bình thường, nhưng chúng tôi thấy một số request lạ trong log. Không giống scan tự động, có vẻ là thử thủ công.”

---

###### Dấu vết trong log
- URL chứa các ký tự **SQL Injection phổ biến**, ví dụ:
    - %27 (dấu ')
    - %20OR%201%3D1  ( OR 1=1)
- Các request này:
    - Trả về HTTP status **200**
    - Lặp lại nhiều lần từ **cùng một IP**
**Bước 1 – Truy vấn để khám phá dữ liệu Web**

```latex
index=finova_web
| stats count by clientip
| sort -count
```

```latex
index=finova_web
| stats count by useragent
```
**Bước 2 - Thu hẹp hành vi**
![truy vấn khám phá ](picture/day9_1.png)
-  Chỉ tập trung vào request có dấu hiệu SQL Injection:
index=finova_web uri="* OR *"

**Bước 3 - Kêt hợp hành vi + Ngưỡng**

index=finova_web 
| eval uri=urldecode(uri) 
| search uri="*OR 1=1*" 
| stats count by clientip
![ip xuát hiện nhiều lần](picture/day9_3.png)
**Bước 4 - Lưu thành Alter duong.huynh**

![alter1luu](picture/day9_2.png)

##### **USE CASE 2 – SSH BRUTE FORCE**
 **Bối cảnh**

SOC phát hiện:

- Nhiều login thất bại
- Chưa rõ là người dùng thật hay attack

---

**Dấu vết trong log**

- Failed password
- Cùng IP → nhiều user khác nhau
- Có khoảng nghỉ giữa các đợt

**Bước 1 - Truy vấn cơ bản**

index=finova_auth result=Failed 
| stats count by src_ip, user
 
 ![chart](picture/day9_4.png)
 **Bước 2 - Gom theo hành vi - Hoàn thiện Rule**
 ![passwordFaild](picture/day9_5.png)

 BTVN
 