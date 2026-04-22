#       SOC Analysic buổi 5
## Giới thiệu







1. Phishing
    - Là kỹ thuật thuộc social engineering
    - Nó là dạng thường dùng nhất

2.  Mục tiêu 
    - Đánh cắp thông tin nhạy cảm
    - Cài mã độc
    - Bước đầu xâm nhập hệ thống

3.  Có các loại
    - email phishing
    - Spear phishing
    - Vising  - Voice
    - Mishing - SMS
    - Evil Twin attack  - wifi giả (PCAP, dump, pass)

4. Bây h có TLS/SSL nên đẫ hạn chế rất nhiều
   - Mã hóa dữ liệu qua đường truyền

---------------------//---------------------------------------------------|
                   

##   BASIC DETECTION



1. Mục tiêu
     - Email này có đáng nghi không
     - EMail này độc hại chỗ nào
     - Có cần Escalate hay điều tra  sâu hơn ko
2. CHECK
     - Online emlreader
     -Virustotal
     - Browerling
     => cẩn thận thực hiện khi upload file document 
     => Kobaoh được truy cập hoặc thực hiện khi link/file nghi ngờ độc hại trực tiếp trên máy tính
     - Boy của email, body login your device - document
     - eml.analyzer.herokuapp.com, cuối link có dấu bằng thì decode ra base 64
     - urlscan.io
     - search aopy thầy gửi, kéo xuống 3 years, block url, block ip
     - Ktra bao nhiêu user nhận được email này, bn user đã login



## ADVANCED DETECTED
### Email Header analysics 
     
1. SPF - Sender Policy Framework
     - Doamin gửi đi có được đăng kí chưa
     - Được gửi từ nguồn uy tín ko
     - 

3. DKIM - Domain Key Identified Mail
     - Còn tem không
4. DMARC - Domain based message authentication reporting & conformace
     - 
- Build hệ thống infra để phishing xong vẫn còn giữ chỉ đổi nội dung email
- Ở môi trường thật  kêu User tải email xong hãy gửi cho mình or forward nhưng attachment(forward as attachment)

---------------------------------------||----------------------------------------
## LAB - search email (Outation)


1. Virustotal : MAcilious .uue => search  .uue
      - Click vào SPF, click do DKIM (có chữ kí, đc đóng dấu, nhưng DMARC fail)
      -  Tương tự như nhận hàng amazon nhưng tem của shopee
      - Click dô đóng bùi nhùi
      - Check internal  => block IP người gửi, block domain
      - Search eml-analyzer.herokuapp, kéo xuống return path, thầy ví dụ joesandbox.com
## 401 
1. Threat Intel
2. Endpoint lastname IP
3. endpoint search hostname
    - lookfor  network.history
    - Outbound processname nào tạo kết nối này
    - Log management - IP => Logtype  => DNS - CLick ở đây

    Giải thích:

    - Zeek bắt được, call ra domain XYZ
     - Lúc ó đi ra thì ZEEK cho đi ra 
     - Nhưng khi 8.8.8.8 trả về  phân giải thành công
     - Nhận ra domain độc hại, phân giải được nhưn không cho đi tiếp
     - no error, internal_block=true
    => User sẽ không nhận được kết quả 
4. 
     - Endpoint Security -> process history DocuSign_Hr_Update.xlsm
5. Endpoint  -> Process history
7. True Positive _Blocked phishing attack
     - 















