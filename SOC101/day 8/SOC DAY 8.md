#                    SOC DAY 8
                                
   ##                             LOG
                           
### 1. Định nghĩa                             
### 2. Phân loại 
| Logs  |  Definition |   Type|
| :---: |  :-------:   |:-----:|
|Application logs| Do ứng dụng tạo ra để ghi lại hành vi và lỗi của App (request, exception) |  Exchang email |
|System logs| Hệ điều hành ghi lại, gồm tình trạng, boot, driver , CPU, RAM, Disk, service, Start, stop  |    |Windows Event
|Security/Audit logs|  Phục vụ bảo mật, kiểm toán, ai , làm gì, khi nào, từ đâu, ..login, out , quyền truy cập, thay đổi cấu hình, ...... |
|Network logs|    Lưu lượng mạng, kết nối |

- 
### 3. Ví dụ 


Có 3 loại dữ lieu thường dung:
Packet (PCAP)
   - Chi tiết "từng gói tin"    => dung khi cần forensic sâu
   - Từ file PACP có thể suy ra được Flow và Metadata 
   - ai nói với ai, IP SRC- DES
   - Khi nào, tần số, có định kì không ?
   - nói chuyện qua cổng nào, bang giao thức nào
   - có dấu hiệu tải file không, gọi C2 không
Flow
   - Tóm tắt kết nối, ai với ai, qua port nào
   - bytes, duration  => nhanh   hunt pattern
Metadata/ logs
   - DNS, http, TLS, SMB logs
   - pivot nhanh theo domain 
   - URL , SNI
Nhưng: Network evidence không process nào tạo kết nối, (cần endpoint)
 - Tool có Open Source và Commecial
 - Tùy ưu diểm và chi phí từng sản phẩm
 - Chat:   Flow là phần static trong wireshark   => Yes

----------------------------|-------------------------------------------------|

                           ZEEK  _ Tool nào không quan trọng, quan trọng là tư duy
Conn.log
 - ai nói chuyện với ai, nhìn thấy nhau không
 - Cổng, giao thức 
 - bytes, duration
 - status
 Hữu dụng :
 - Scan Beaconing
 - Outbound bất thường

Dns.logs
 - Query                 - Newdomain
 - Answer                - C2 qua DNS
 - NXDOMAIN
 - Loại record 

 Hữu dụng :
 

Http.logs
 - Host, uri             - mime/bytes
 - Method, status
 - User, agent 
Hữu dụng :
 - Xem download, payload
 - Web C2
 - Method : get, post, put
 - Host:
 -uri i theo sau host
 - User agent:/   "curl 17.7.0"    (nó chạy trong cmd)
Zeek.Connlog
 - cách 5p chạy 1 lần, tần suất
 -Orig_bytes và Respon_bytes sấp sỉ process liền trước
 - Khoảng thời gian tưdươnđương 
=> dấu hiệu Beaconing lưu lưdịnhđịnh kì
C2 là gì?????,   search
Check  link    otx.alienvault.com   => nhiễm malware

---------------------------------||-----------------------------------------------|

example: đọc dns.log

   - domain . onion => mạng tor   => dark wedark

example: đọc 2001    Potential beconing Activity Detected
   - Oct 10 2025 3h11pm
   - IP, port,   định kì 5p/ lần, datasize tương dương
   - Gửi đi gửi lại   gọi là (allow <=> success)
   - 2. gửi ra => IP des  => logtype search: zeek.com    5 mins
   - 3. Socradar  => check ip => endpoint search IP src    svchost.ễ là gì  => process injection
   - 5. True   - Khi suspicious need investigation
               - Thì sẽ không chặn kịp nữa
   - 6. Isolate máy IP 86=5.208.253.156

Buổi 4 đọc file PCAP


