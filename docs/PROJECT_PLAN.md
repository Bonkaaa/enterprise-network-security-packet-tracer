# KẾ HOẠCH TRIỂN KHAI BÀI TẬP LỚN (PROJECT PLAN)
## MÔN: THIẾT KẾ, CÀI ĐẶT VÀ QUẢN TRỊ MẠNG
**Đề tài:** Thiết kế và mô phỏng hệ thống mạng doanh nghiệp an toàn sử dụng VLAN, Inter-VLAN Routing và ACL trên Cisco Packet Tracer  
**Quy mô nhóm:** 3 thành viên  

---

## 1. TỔNG QUAN VỀ ĐỀ TÀI & MỤC TIÊU CỐT LÕI

### 1.1. Bản chất đề tài là gì?
Đề tài giải quyết bài toán mạng cho một doanh nghiệp vừa (100–200 nhân viên) với nhiều phòng ban cần dùng chung một hạ tầng mạng nhưng phải đảm bảo an toàn tuyệt đối:
* **Phân vùng mạng (VLAN Segmentation):** Tách biệt các bộ phận (Nhân sự - HR, Kế toán - Finance, CNTT - IT, Ban Giám đốc - Management) để hạn chế Broadcast domain và ngăn ngừa lây lan mã độc nội bộ.
* **Định tuyến Inter-VLAN (Layer 3 Routing via SVI):** Cho phép các phòng ban giao tiếp với nhau khi cần thông qua Switch Layer 3 (Cisco 3560).
* **Kiểm soát truy cập (Access Control List - ACL):** Thiết lập chính sách bảo mật:
  * HR không được phép truy cập sang Finance và ngược lại.
  * Chỉ IT và Management được quyền kết nối SSH quản trị Switch/Router.
  * Phân vùng Attacker (VLAN 99) bị cô lập hoàn toàn, không thể chọc phá Server nội bộ hay các máy trạm.
* **Mở rộng kết nối Internet & Dịch vụ:** Bổ sung Router biên, cấu hình NAT/PAT để nhân viên truy cập được ra ngoài Internet (ISP), triển khai các dịch vụ mô phỏng Web (HTTP), DNS, FTP, DHCP.

### 1.2. Các sản phẩm cần nộp (Deliverables)
1. **File mô phỏng Cisco Packet Tracer (`.pkt`):** 01 file duy nhất hoàn chỉnh đại diện cho toàn bộ hệ thống mạng của nhóm.
2. **File kịch bản cấu hình (`Configs/*.txt`):** Toàn bộ câu lệnh CLI được phân chia theo từng thiết bị (Core-SW, Access-SW1, Access-SW2, Edge-Router) kèm chú thích tiếng Việt rõ ràng.
3. **Báo cáo kỹ thuật (Word / PDF):** Độ dài khoảng 30–45 trang, trình bày chuẩn format học thuật: Khảo sát, Thiết kế sơ đồ mạng, Bảng địa chỉ IP & VLAN, Hướng dẫn cấu hình chi tiết, Kịch bản kiểm thử (kèm ảnh chụp màn hình minh chứng có timestamp/hostname) và Kết luận.
4. **Slide thuyết trình (PowerPoint / Canva):** 15–20 slide tóm tắt để nhóm bảo vệ đồ án trước giảng viên.
5. **Kịch bản Demo & Vấn đáp trực tiếp (Live Demo & Q&A):** Bảng phân công ai nói phần nào, chuẩn bị sẵn câu trả lời cho các câu hỏi giảng viên hay xoáy vào.

---

## 2. DANH MỤC CÔNG CỤ CẦN TẢI & CÀI ĐẶT (TOOLSET)

| STT | Tên công cụ / Phần mềm | Mục đích sử dụng | Thành viên cần cài | Link tải / Cách lấy |
|:---:|---|---|:---:|---|
| **1** | **Cisco Packet Tracer** *(bản 8.2 trở lên)* | Dựng sơ đồ, cấu hình thiết bị qua CLI và mô phỏng truyền gói tin. | **Cả 3 thành viên** | Tải miễn phí từ [Cisco Skills For All](https://skillsforall.com/) hoặc NetAcad. |
| **2** | **VS Code** hoặc **Notepad++** | Soạn thảo kịch bản lệnh CLI (`.txt`), kiểm tra cú pháp trước khi dán vào Packet Tracer. | **Cả 3 thành viên** | [code.visualstudio.com](https://code.visualstudio.com/) *(Cài thêm Extension: `Cisco IOS Syntax`)* |
| **3** | **draw.io** *(Diagrams.net)* | Vẽ sơ đồ kiến trúc mạng logic & vật lý sắc nét (chuẩn icon Cisco) để chèn vào Báo cáo & Slide. | **Thành viên 1 & 3** | Dùng trực tiếp trên web [app.diagrams.net](https://app.diagrams.net/) |
| **4** | **Snipping Tool** / **Lightshot** | Chụp ảnh màn hình CLI, kết quả Ping, Simulation Mode, đóng khung đỏ/ghi chú kết quả kiểm thử. | **Thành viên 3** (và cả nhóm) | Có sẵn trên Windows (`Win + Shift + S`) |
| **5** | **Microsoft Word / Google Docs** | Soạn thảo báo cáo kỹ thuật 30–45 trang (sử dụng Header, Style, Mục lục tự động). | **Thành viên 3** (và cả nhóm) | Bộ cài Office hoặc Google Docs |
| **6** | **PowerPoint / Canva** | Thiết kế slide thuyết trình bảo vệ đồ án trước giảng viên. | **Thành viên 3** (và cả nhóm) | Bộ cài Office hoặc [canva.com](https://www.canva.com/) |
| **7** | **Google Drive / Zalo Cloud** | Lưu trữ và đồng bộ file `.pkt`, tài liệu nhóm, tránh bị ghi đè mất file. | **Cả 3 thành viên** | Tạo 1 folder chung trên Google Drive |

---

## 3. CƠ CHẾ DÙNG CHUNG VÀ PHỐI HỢP FILE PACKET TRACER (.PKT) TRONG NHÓM

### 3.1. Giải đáp: "Nhóm có mở chung và cùng sửa 1 file trực tiếp được không?"
* **Câu trả lời là: KHÔNG.**
* Khác với Google Docs hay Figma, Cisco Packet Tracer **không hỗ trợ** tính năng cho nhiều người cùng mở và chỉnh sửa trực tiếp trên 1 file cùng lúc theo thời gian thực.
* File `.pkt` là file dữ liệu nhị phân (binary) lưu trên ổ cứng của từng máy. Cuối kỳ, nhóm sẽ **chỉ nộp DUY NHẤT 01 file `.pkt` hoàn chỉnh** cho thầy cô chấm.

---

### 3.2. Hai phương án phối hợp làm việc trong nhóm (Chọn 1 trong 2)

#### 🌟 PHƯƠNG ÁN A: Quy trình "Chuyền tay theo phiên bản" (Khuyên dùng nhất – Cực kỳ an toàn)
Vì đồ án mạng có tính chất kế thừa từng tầng (phải có Core và IP trước -> mới có Access Switch và ACL -> mới có hệ thống để kiểm thử), nhóm thực hiện theo dây chuyền:
1. **Giai đoạn 1 (TV1 làm):**
   * Lấy file gốc [`NETWORK SECURITY.pkt`](../NETWORK%20SECURITY.pkt), đổi tên thiết bị thành `CoreSW-Nhom8`, kéo thêm Router biên, cấu hình xong Core Switch và IP Gateway (SVI).
   * Lưu thành file: `NETWORK_NHOM8_v1_Core.pkt`.
   * Đẩy file `v1` này lên Google Drive chung của nhóm.
2. **Giai đoạn 2 (TV2 làm):**
   * Tải file `NETWORK_NHOM8_v1_Core.pkt` từ Google Drive về máy tính của mình.
   * Mở lên và làm tiếp phần của mình: Gán cổng Access Switch, viết rule Extended ACL, cấu hình SSHv2, bật các dịch vụ trên Server (Web, DNS).
   * Lưu thành file: `NETWORK_NHOM8_v2_Security.pkt`.
   * Đẩy file `v2` này lên lại Google Drive của nhóm.
3. **Giai đoạn 3 (TV3 làm):**
   * Tải file `NETWORK_NHOM8_v2_Security.pkt` về máy tính của mình.
   * Mở lên kiểm tra toàn diện, chạy 10 kịch bản test (Ping, Simulation Mode bắt gói tin), chụp ảnh minh chứng làm báo cáo và slide.
   * Lưu thành bản chốt cuối cùng: `NETWORK_NHOM8_FINAL.pkt`.

> **Ưu điểm của Phương án A:** Rõ ràng, không bao giờ lo bị người này ghi đè xóa nhầm bài của người kia, mỗi bước đều có file backup trên Drive.

---

#### 🚀 PHƯƠNG ÁN B: "Làm việc độc lập qua File Text lệnh CLI" (Chuẩn phương pháp Kỹ sư mạng)
Bản chất mọi cấu hình trong thiết bị Cisco đều là các đoạn mã lệnh text (CLI Script). Nhóm có thể làm song song mà không cần đợi nhau:
1. Cả 3 thành viên đều tải file [`NETWORK SECURITY.pkt`](../NETWORK%20SECURITY.pkt) về máy riêng để tự do thử nghiệm độc lập.
2. Khi làm việc, mỗi người viết và lưu các đoạn lệnh cấu hình của mình vào các file text riêng trong thư mục `Configs/`:
   * **TV1** phụ trách viết file text: [`Configs/CORE-SW.txt`](../Configs/CORE-SW.txt) và `Configs/EDGE-ROUTER.txt`.
   * **TV2** phụ trách viết file text: [`Configs/Access-SW1.txt`](../Configs/Access-SW1.txt), `Configs/Access-SW2.txt` và `Configs/ACL-SECURITY.txt`.
3. Đến buổi họp nhóm (hoặc gọi Discord/Google Meet chia sẻ màn hình):
   * Một bạn đại diện mở một file Packet Tracer sạch.
   * Lần lượt mở CLI của từng Switch/Router, copy toàn bộ nội dung trong các file `.txt` của TV1 và TV2 dán (paste) vào.
   * Chỉ mất **2 phút** copy-paste là cả hệ thống tự động nhận toàn bộ cấu hình và chạy hoàn hảo!

---

### 3.3. Yêu cầu bắt buộc khi đi thi / vấn đáp bảo vệ đồ án:
* Mặc dù nhóm chỉ nộp 01 file `.pkt`, nhưng **CẢ 3 THÀNH VIÊN ĐỀU BẮT BUỘC PHẢI LƯU BẢN `FINAL.pkt` TRÊN LAPTOP CÁ NHÂN CỦA MÌNH**.
* Khi bảo vệ đồ án trước giảng viên, thầy cô sẽ gọi ngẫu nhiên bất kỳ bạn nào mở máy lên:
  * *"Em Thành viên 3 mở Packet Tracer lên máy em, ping thử từ máy HR sang Finance cho thầy xem tại sao lại bị chặn?"*
  * *"Em Thành viên 2 mở CLI của Core Switch gõ lệnh `show ip access-lists` giải thích từng dòng cho thầy nghe?"*
* Do đó, sau khi hoàn thành file `FINAL.pkt`, **cả 3 bạn phải ngồi lại với nhau, cùng mở file trên máy mình và tự tay gõ thử toàn bộ các lệnh kiểm thử**.

---

## 4. PHÂN CHIA CÔNG VIỆC VÀ HƯỚNG DẪN CÔNG CỤ CHO TỪNG THÀNH VIÊN

---

### 👤 THÀNH VIÊN 1: Trưởng nhóm – Kỹ sư Thiết kế Mạng & Định tuyến Core (Core Network & Routing Lead)

* **Vai trò:** Phụ trách hạ tầng mạng xương sống, sơ đồ phân tầng và định tuyến Core / Router biên.
* **Công cụ cần dùng:** Cisco Packet Tracer, VS Code, draw.io.

#### 🛠️ Hướng dẫn cách làm chi tiết từng bước:
1. **Bước 1 - Mở & Khởi tạo Topology (trên Packet Tracer):**
   * Mở file có sẵn [`NETWORK SECURITY.pkt`](../NETWORK%20SECURITY.pkt).
   * Đổi tên thiết bị mang dấu ấn Nhóm 8: Click vào thiết bị > Tab **Config** > Đổi Display Name & Hostname (ví dụ: `CoreSW-Nhom8`).
   * Kéo thêm 01 **Router 2911** (hoặc 1941) đóng vai trò Router biên (`EdgeRouter-Nhom8`) và 01 Router đóng vai trò mạng ngoài (`ISP-Internet`).
   * Dùng cáp thẳng (Copper Straight-Through) nối từ port `GigabitEthernet` của Core Switch lên Router biên.
2. **Bước 2 - Viết kịch bản cấu hình Core Switch (trên VS Code):**
   * Mở VS Code, tạo/chỉnh sửa file [`Configs/CORE-SW.txt`](../Configs/CORE-SW.txt).
   * Soạn thảo các lệnh kích hoạt định tuyến L3 và cấu hình các SVI (Switch Virtual Interface) đóng vai trò Gateway cho từng VLAN:
     ```cisco
     enable
     configure terminal
     hostname CoreSW-Nhom8
     ip routing
     
     ! Tạo VLAN và gán SVI Gateway
     vlan 10
      name HR
     interface Vlan10
      ip address 192.168.10.1 255.255.255.0
      no shutdown
     
     vlan 20
      name Finance
     interface Vlan20
      ip address 192.168.20.1 255.255.255.0
      no shutdown
     ! (Tương tự cho VLAN 30, 40, 50, 99)
     
     ! Cấu hình cổng Trunk nối xuống Access Switch
     interface range GigabitEthernet0/1 - 2
      switchport trunk encapsulation dot1q
      switchport mode trunk
      no shutdown
     ```
   * Mở tab **CLI** trên Core Switch trong Packet Tracer, dán đoạn lệnh trên vào và gõ lệnh `write memory` để lưu.
3. **Bước 3 - Cấu hình mở rộng Router & NAT/PAT ra Internet (trên VS Code & Packet Tracer):**
   * Soạn kịch bản cấu hình cho Router biên để biên dịch dải mạng nội bộ ra ngoài Internet:
     ```cisco
     hostname EdgeRouter-Nhom8
     ! Cổng nối về Core Switch
     interface GigabitEthernet0/0
      ip address 10.0.0.1 255.255.255.252
      ip nat inside
      no shutdown
     
     ! Cổng nối ra ISP Internet
     interface GigabitEthernet0/1
      ip address 203.0.113.2 255.255.255.252
      ip nat outside
      no shutdown
     
     ! Default Route đẩy toàn bộ dữ liệu ra ISP
     ip route 0.0.0.0 0.0.0.0 203.0.113.1
     
     ! Cấu hình NAT/PAT Overload cho toàn bộ dải mạng công ty
     access-list 1 permit 192.168.0.0 0.0.255.255
     ip nat inside source list 1 interface GigabitEthernet0/1 overload
     ```
   * Trên Core Switch thêm default route đẩy ra Router biên: `ip route 0.0.0.0 0.0.0.0 10.0.0.1`.
4. **Bước 4 - Vẽ lại sơ đồ mạng chuyên nghiệp (trên draw.io):**
   * Truy cập [app.diagrams.net](https://app.diagrams.net/), bật bộ thư viện icon **Cisco** (More Shapes > Tích chọn Cisco).
   * Vẽ lại sơ đồ logic gồm đầy đủ Core Switch, 2 Access Switch, Router biên, ISP, Server và các PC.
   * Xuất file ảnh PNG độ nét cao (300 DPI) gửi cho Thành viên 3 để chèn vào Báo cáo và Slide.
5. **Bước 5 - Viết phần Báo cáo Word:**
   * Viết **Chương 1**: Tổng quan bài toán, phân tích yêu cầu doanh nghiệp 100–200 nhân viên.
   * Viết **Chương 2**: Thiết kế kiến trúc mạng, bảng phân hoạch IP/Subnet và cấu hình Core Switch + Router.

---

### 👤 THÀNH VIÊN 2: Kỹ sư Bảo mật & Dịch vụ Mạng (Security & Services Engineer)

* **Vai trò:** Phụ trách cấu hình bảo mật trên Access Switch, các bộ lọc ACL, quản trị SSH và máy chủ dịch vụ.
* **Công cụ cần dùng:** Cisco Packet Tracer, VS Code / Notepad++.

#### 🛠️ Hướng dẫn cách làm chi tiết từng bước:
1. **Bước 1 - Cấu hình Access Switch 1 & 2 (trên VS Code & Packet Tracer):**
   * Mở file cấu hình [`Configs/Access-SW1.txt`](../Configs/Access-SW1.txt) và [`Configs/Access-SW2.txt`](../Configs/Access-SW2.txt).
   * Khai báo các VLAN trên Access Switch.
   * Gán các cổng nối xuống PC theo chế độ Access mode:
     ```cisco
     ! Ví dụ trên Access-SW1 (nối máy HR và Finance)
     interface range FastEthernet0/1 - 10
      switchport mode access
      switchport access vlan 10
     
     interface range FastEthernet0/11 - 20
      switchport mode access
      switchport access vlan 20
     
     ! Cổng nối lên Core Switch
     interface GigabitEthernet0/1
      switchport mode trunk
     ```
   * Dán vào CLI của Switch 1 và Switch 2 trong Packet Tracer.
2. **Bước 2 - Viết và triển khai chính sách bảo mật Extended ACL (trên VS Code):**
   * Mở VS Code soạn thảo các rule lọc lưu lượng:
     ```cisco
     ! 1. Ngăn HR truy cập vào dải Finance
     ip access-list extended HR-ACL
      deny ip 192.168.10.0 0.0.0.255 192.168.20.0 0.0.0.255
      permit ip any any
     
     ! 2. Ngăn Finance truy cập vào dải HR
     ip access-list extended FIN-ACL
      deny ip 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255
      permit ip any any
     
     ! 3. Cô lập hoàn toàn Attacker (VLAN 99) khỏi mạng nội bộ
     ip access-list extended BLOCK-ATTACKER
      deny ip 192.168.99.0 0.0.0.255 192.168.0.0 0.255.255
      deny ip 192.168.0.0 0.255.255 192.168.99.0 0.0.0.255
      permit ip any any
     ```
   * Áp dụng vào các cổng SVI tương ứng trên Core Switch:
     ```cisco
     interface Vlan10
      ip access-group HR-ACL in
     interface Vlan20
      ip access-group FIN-ACL in
     interface Vlan99
      ip access-group BLOCK-ATTACKER in
     ```
3. **Bước 3 - Cấu hình SSHv2 Quản trị an toàn (trên Core Switch):**
   * Cấu hình tạo khóa RSA và giới hạn dải IP quản trị:
     ```cisco
     ip domain-name nhom8.internal
     username itadmin privilege 15 secret Cisco@Nhom8
     crypto key generate rsa modulus 1024
     ip ssh version 2
     
     ! Chỉ cho phép IP thuộc dải IT (VLAN 30) và Management (VLAN 40) SSH
     ip access-list standard SSH-MGMT
      permit 192.168.30.0 0.0.0.255
      permit 192.168.40.0 0.0.0.255
      deny any
     
     line vty 0 15
      transport input ssh
      login local
      access-class SSH-MGMT in
     ```
4. **Bước 4 - Cấu hình máy chủ Dịch vụ (trên giao diện Packet Tracer):**
   * Nhấp đúp vào biểu tượng thiết bị **Server** trong Packet Tracer > Chọn tab **Services**:
     * **Mục HTTP:** Chọn `ON`. Bấm nút `edit` file `index.html` sửa tiêu đề thành: `"HE THONG MANG NOI BO - NHOM 8"`.
     * **Mục DNS:** Chọn `ON`. Nhập tên miền (ví dụ: `portal.nhom8.internal`), chọn Type `A Record`, gán địa chỉ IP Server (`192.168.50.10`) > Bấm `Add`.
     * **Mục DHCP (Tùy chọn ghi điểm):** Thiết lập cấp IP tự động cho các VLAN (Default Gateway, DNS IP, IP Pool).
5. **Bước 5 - Viết phần Báo cáo Word:**
   * Viết **Chương 3**: Phân tích cơ chế hoạt động của ACL (chuẩn Extended vs Standard, chiều Inbound/Outbound), nguyên lý SSHv2 và tài liệu cấu hình máy chủ dịch vụ.

---

### 👤 THÀNH VIÊN 3: Kỹ sư Kiểm thử, Soạn thảo Báo cáo & Slide (QA, Testing & Documentation Lead)

* **Vai trò:** Kiểm thử toàn bộ hệ thống, chụp ảnh minh chứng, viết Báo cáo Word hoàn chỉnh và thiết kế Slide bảo vệ.
* **Công cụ cần dùng:** Cisco Packet Tracer (Chế độ Realtime & Simulation), Snipping Tool, Microsoft Word, PowerPoint / Canva.

#### 🛠️ Hướng dẫn cách làm chi tiết từng bước:
1. **Bước 1 - Kiểm thử bằng Command Prompt trên PC ảo (Realtime Mode):**
   * Mở file `.pkt` đã hoàn thiện từ TV1 và TV2.
   * Nhấp vào một PC bất kỳ (ví dụ máy phòng HR) > Chọn tab **Desktop** > Mở **Command Prompt**:
     * *Kiểm tra cùng VLAN:* Gõ `ping 192.168.10.3` -> Phải nhận được `Reply from ...` (Thành công).
     * *Kiểm tra chặn chéo:* Gõ `ping 192.168.20.2` (máy Finance) -> Phải nhận được `Destination host unreachable` (ACL chặn thành công).
     * *Kiểm tra dịch vụ:* Mở **Web Browser** trên máy trạm, gõ `http://portal.nhom8.internal` -> Trang web phải hiển thị.
     * *Kiểm tra SSH:* Trên máy IT, gõ lệnh `ssh -l itadmin 192.168.30.1` -> Nhập pass `Cisco@Nhom8` -> Vào được chế độ CLI. Thử lệnh tương tự từ máy Finance -> Báo `Connection refused`.
2. **Bước 2 - Bắt gói tin và chứng minh bằng Simulation Mode (Packet Tracer):**
   * Ở góc dưới bên phải màn hình Packet Tracer, bấm vào nút **Simulation** (hoặc nhấn tổ hợp phím `Shift + S`).
   * Bấm vào nút **Show All/None**, sau đó bấm **Edit Filters**: Chỉ tick chọn `ICMP`, `TCP`, `DNS`, `HTTP` để sơ đồ không bị rối.
   * Lấy gói tin PDU (hình phong bì màu xanh, phím tắt `P`), cắm từ máy HR sang máy Finance.
   * Nhấn nút **Play** hoặc **Capture/Forward**: Quan sát gói tin chạy từ PC -> Access Switch -> Core Switch. Tại Core Switch, gói tin xuất hiện biểu tượng **chữ X màu đỏ** (bị hủy).
   * Nhấp đúp vào chữ X màu đỏ để mở bảng **In Layers / Out Layers**, hiển thị rõ dòng: *"Packet matched ACL deny rule"*.
3. **Bước 3 - Chụp ảnh màn hình minh chứng chất lượng cao (bằng Snipping Tool):**
   * Nhấn tổ hợp phím `Windows + Shift + S` để quét vùng màn hình cần chụp (cửa sổ Command Prompt hoặc màn hình Simulation).
   * Dùng công cụ bút đỏ hoặc vẽ hình chữ nhật đóng khung các dòng kết quả then chốt (như dòng thông báo bị từ chối hoặc hostname thiết bị).
   * Lưu ảnh theo thứ tự rõ ràng vào một thư mục: `Hinh4.1_Test_CungVLAN.png`, `Hinh4.2_ACL_HR_Finance.png`...
4. **Bước 4 - Soạn thảo & Định dạng Báo cáo kỹ thuật (trên Microsoft Word):**
   * Tạo khung đồ án chuẩn:
     * Trang bìa (Tên trường, khoa, môn, đề tài, danh sách thành viên nhóm kèm MSSV).
     * Mục lục tự động (Table of Contents), Danh mục hình vẽ (List of Figures), Danh mục bảng biểu.
     * Thu thập nội dung Chương 1, 2 từ TV1 và Chương 3 từ TV2.
     * Tự chắp bút **Chương 4 (Kết quả kiểm thử & Phân tích)**: Lập bảng 10 kịch bản test, chèn từng ảnh minh chứng đã chụp kèm lời bình luận giải thích.
     * Tự chắp bút **Chương 5 (Kết luận & Hướng phát triển)**: Nêu ưu điểm hệ thống, tồn tại và hướng mở rộng (như bổ sung VPN cho nhân viên làm việc từ xa, dựng Firewall cứng Cisco ASA).
   * Kiểm tra lỗi chính tả, căn lề (Justify), giãn dòng 1.3 - 1.5, font Times New Roman hoặc Arial. Xuất ra file **PDF**.
5. **Bước 5 - Thiết kế Slide thuyết trình (trên PowerPoint hoặc Canva):**
   * Thiết kế từ 15 đến 20 slide với các phần trọng tâm:
     * *Slide 1-3:* Giới thiệu nhóm & Bối cảnh đề tài.
     * *Slide 4-6:* Sơ đồ kiến trúc mạng (chèn ảnh draw.io của TV1) & Bảng phân chia VLAN/IP.
     * *Slide 7-10:* Giải pháp bảo mật (Cơ chế ACL, chặn tấn công, bảo vệ SSH).
     * *Slide 11-15:* Kết quả kiểm thử thực tế (ảnh minh chứng packet bị drop).
     * *Slide 16-18:* Kết luận và phần Q&A.

---

## 5. LỘ TRÌNH TRIỂN KHAI 4 TUẦN (ACTION ROADMAP)

| Tuần | Giai đoạn | Công việc chi tiết | Người phụ trách chính | Sản phẩm đầu ra |
|---|---|---|---|---|
| **Tuần 1** | **Khởi động & Nắm bắt** | - Cài đặt Cisco Packet Tracer, VS Code cho cả 3 người.<br>- Mở file `NETWORK SECURITY.pkt` có sẵn để đọc hiểu cách hoạt động.<br>- Thảo luận nhóm, chốt danh sách phòng ban, bảng IP và tên thiết bị mang dấu ấn nhóm.<br>- Tạo khung dàn ý Báo cáo Word & phân bổ chương. | Cả 3 thành viên | - Biên bản họp nhóm.<br>- Bảng phân bổ IP & VLAN Excel/Docs.<br>- Khung mục lục Báo cáo Word. |
| **Tuần 2** | **Xây dựng & Mở rộng mô hình** | - Dựng lại hoặc chỉnh sửa topology: Đổi tên thiết bị (`CoreSW-Nhom8`,...).<br>- Bổ sung Router kết nối ISP + cấu hình NAT/PAT.<br>- Cấu hình lại các Switch, SVI, Trunking, Port Access.<br>- Triển khai đầy đủ các rule ACL, SSH và cấu hình Server. | Thành viên 1 & 2 | - File `.pkt` hoàn thiện đầy đủ tính năng.<br>- Các file `.txt` cấu hình đã chú thích tiếng Việt.<br>- Sơ đồ draw.io xuất ảnh nét. |
| **Tuần 3** | **Kiểm thử & Soạn thảo Báo cáo** | - Chạy tuần tự 10 kịch bản kiểm thử, chụp ảnh đầy đủ màn hình CMD và Simulation.<br>- Viết chi tiết các chương trong Báo cáo Word.<br>- Chèn bảng biểu, sơ đồ và ảnh minh chứng vào báo cáo. | Thành viên 3 (Chính), TV 1 & 2 (Hỗ trợ nội dung kỹ thuật) | - Bản thảo Báo cáo kỹ thuật (Word) hoàn thiện 90%.<br>- Bộ ảnh kiểm thử phân loại theo kịch bản. |
| **Tuần 4** | **Hoàn thiện, Làm Slide & Luyện Demo** | - Rà soát format, chính tả báo cáo, xuất file PDF.<br>- Thiết kế Slide thuyết trình PowerPoint/Canva.<br>- Nhóm họp tổng duyệt: Luyện tập thuyết trình và demo trực tiếp trên Packet Tracer.<br>- Chuẩn bị sẵn các câu hỏi vấn đáp của giảng viên. | Cả 3 thành viên | - Báo cáo hoàn chỉnh (Word + PDF).<br>- Slide thuyết trình PowerPoint.<br>- Video demo backup (nếu có). |

---

## 6. BỘ CÂU HỎI VẤN ĐÁP THẦY CÔ HAY HỎI (CHUẨN BỊ TRƯỚC ĐỂ ĐẠT ĐIỂM CAO)

1. **Về VLAN & Trunking:**
   * *Câu hỏi:* Tại sao cần phân chia VLAN? Chuẩn 802.1Q là gì và hoạt động ở tầng nào?
   * *Gợi ý trả lời:* Phân chia VLAN giúp chia nhỏ Broadcast domain, tăng cường an ninh bằng cách cô lập lưu lượng giữa các phòng ban. Chuẩn 802.1Q hoạt động ở tầng Data Link (Layer 2), nó chèn một tag 4-byte (chứa VLAN ID 12-bit) vào Ethernet frame để phân biệt gói tin thuộc VLAN nào khi đi qua đường Trunk.
2. **Về Định tuyến Inter-VLAN & SVI:**
   * *Câu hỏi:* SVI (Switch Virtual Interface) là gì? Khác gì với kỹ thuật Router-on-a-Stick?
   * *Gợi ý trả lời:* SVI là cổng ảo Layer 3 trên Switch Multi-layer đại diện cho một VLAN và đóng vai trò làm Default Gateway cho subnet đó. So với Router-on-a-Stick (dùng sub-interface trên 1 Router vật lý), SVI định tuyến bằng phần cứng (ASIC) bên trong Core Switch nên tốc độ chuyển mạch dây (wire-speed) nhanh hơn rất nhiều và không bị nghẽn cổ chai ở đường link vật lý.
3. **Về ACL:**
   * *Câu hỏi:* Sự khác nhau giữa Standard ACL và Extended ACL? ACL nên đặt ở chiều Inbound hay Outbound?
   * *Gợi ý trả lời:* Standard ACL chỉ lọc dựa trên địa chỉ IP nguồn (Source IP). Extended ACL có thể lọc dựa trên IP nguồn, IP đích, giao thức (TCP, UDP, ICMP) và số hiệu cổng dịch vụ (Port 80, 443, 22...). Về vị trí: Extended ACL nên đặt càng gần nguồn càng tốt (thường là chiều `in` trên cổng SVI tương ứng) để loại bỏ gói tin không hợp lệ ngay lập tức, tiết kiệm băng thông mạng.
4. **Về SSH & Quản trị:**
   * *Câu hỏi:* Tại sao lại dùng SSH thay cho Telnet? Cách giới hạn chỉ cho IT SSH vào Switch?
   * *Gợi ý trả lời:* Telnet truyền dữ liệu (kể cả username/password) ở dạng văn bản thuần (plaintext) nên dễ bị bắt gói tin (sniffing). SSH mã hóa toàn bộ phiên làm việc (dùng thuật toán RSA/AES). Để giới hạn, tạo 1 Access-list chỉ cho phép Subnet IT (192.168.30.0/24) và áp dụng vào đường `line vty 0 15` bằng lệnh `access-class SSH-MGMT in`.
5. **Về NAT/PAT:**
   * *Câu hỏi:* NAT/PAT dùng để làm gì trong mô hình này?
   * *Gợi ý trả lời:* Các IP trong công ty là IP Private (dải 192.168.x.x) không thể định tuyến trực tiếp trên Internet công cộng. NAT/PAT (Port Address Translation / Overload) giúp chuyển đổi nhiều địa chỉ IP Private thành 1 địa chỉ IP Public trên cổng ngoài của Router để ra mạng Internet, đồng thời giúp tiết kiệm địa chỉ IPv4.
