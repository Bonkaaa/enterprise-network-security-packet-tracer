# KẾ HOẠCH CHI TIẾT CHO THÀNH VIÊN 1 (TRƯỞNG NHÓM)
## Vai trò: Trưởng nhóm – Kỹ sư Thiết kế Mạng & Định tuyến Core (Core Network & Routing Lead)
**Đề tài:** Thiết kế và mô phỏng hệ thống mạng doanh nghiệp an toàn sử dụng VLAN, Inter-VLAN Routing và ACL trên Cisco Packet Tracer  
**Tài liệu tham chiếu:** [`PROJECT_PLAN.md`](PROJECT_PLAN.md) | File gốc: [`NETWORK SECURITY.pkt`](NETWORK%20SECURITY.pkt)

---

## TỔNG QUAN TRÁCH NHIỆM CỦA BẠN

Là **Thành viên 1 (Trưởng nhóm)**, bạn là người **khởi động dự án đầu tiên** và dựng toàn bộ "khung xương" của hệ thống mạng trước khi chuyền tay cho Thành viên 2 (Bảo mật/Server) và Thành viên 3 (Kiểm thử/Báo cáo).

Công việc của bạn gồm **6 Giai đoạn** theo đúng thứ tự từ lúc bắt đầu đến lúc đi bảo vệ:
1. **Giai đoạn 0:** Chuẩn bị công cụ, điều phối nhóm & đọc hiểu hệ thống gốc.
2. **Giai đoạn 1:** Quy hoạch bảng IP, VLAN, cổng kết nối (Port Mapping) và đặt tên thiết bị.
3. **Giai đoạn 2:** Chỉnh sửa Topology trên Cisco Packet Tracer (đổi tên + lắp thêm Router biên & ISP).
4. **Giai đoạn 3:** Cấu hình Core Switch (`CoreSW-Nhom8`), Router biên (`EdgeRouter-Nhom8`) và `ISP-Internet` (Inter-VLAN Routing + Default Route + NAT/PAT).
5. **Giai đoạn 4:** Tự kiểm tra thông tuyến Core & Internet, lưu file `v1` bàn giao cho Thành viên 2.
6. **Giai đoạn 5:** Vẽ sơ đồ kiến trúc mạng bằng `draw.io` & Viết **Chương 1 + Chương 2** của Báo cáo Word.
7. **Giai đoạn 6:** Ghép bài cùng nhóm, chuẩn bị phần Demo & trả lời vấn đáp trước giảng viên.

---

## GIAI ĐOẠN 0: CHUẨN BỊ MÔI TRƯỜNG & ĐIỀU PHỐI NHÓM (NGÀY 1–2)

### Bước 0.1: Tải và cài đặt các công cụ bạn cần dùng
* [x] **Cisco Packet Tracer (bản 8.2 trở lên):** Tải từ [Cisco Skills For All](https://skillsforall.com/) hoặc NetAcad và đăng nhập sẵn.
* [x] **VS Code:** Tải từ [code.visualstudio.com](https://code.visualstudio.com/) để soạn thảo các file `.txt` cấu hình (có thể cài thêm extension *Cisco IOS Syntax*).
* [x] **draw.io (Diagrams.net):** Không cần cài, đánh dấu trang [app.diagrams.net](https://app.diagrams.net/) để vẽ sơ đồ mạng.
* [x] **Google Drive / Zalo nhóm:** Tạo 1 thư mục chung trên Google Drive tên là `BTL_MangMayTinh_Nhom8` với 3 thư mục con:
  * `01_PacketTracer_Versions/` (chứa các bản `.pkt` theo version `v1`, `v2`, `FINAL`)
  * `02_Configs_CLI/` (chứa các file `.txt` cấu hình)
  * `03_BaoCao_Slide/` (chứa file Word, ảnh chụp, sơ đồ draw.io, PowerPoint)

### Bước 0.2: Họp nhóm chốt quy tắc làm việc (15–30 phút)
* [x] Gửi file [`PROJECT_PLAN.md`](PROJECT_PLAN.md) cho Thành viên 2 và Thành viên 3 đọc.
* [x] Thống nhất dùng **Phương án A (Chuyền tay theo phiên bản)** để không bị ghi đè file `.pkt`:
  * Bạn làm xong **Bản `v1`** (`NETWORK_NHOM8_v1_Core.pkt`) -> đẩy lên Drive -> báo TV2 tải về làm tiếp **Bản `v2`** -> báo TV3 tải về kiểm thử ra **Bản `FINAL`**.
* [x] Chốt deadline nội bộ từng tuần với 2 thành viên còn lại.

### Bước 0.3: Mở và đọc hiểu file dự án gốc có sẵn
* [x] Mở file [`NETWORK SECURITY.pkt`](NETWORK%20SECURITY.pkt) bằng Cisco Packet Tracer để xem bố cục hiện tại (gồm `CORE-SW`, `Access-SW1`, `Access-SW2`, 14 PC/Laptop, 3 Server, 1 Attacker PC).
* [x] Mở đọc 3 file cấu hình mẫu trong thư mục `Configs/` bằng VS Code để biết các cổng đang dùng:
  * [`Configs/CORE-SW.txt`](Configs/CORE-SW.txt): `CORE-SW` là Switch Layer 3 (3560), đang dùng `GigabitEthernet0/1` nối xuống `Access-SW1` và `GigabitEthernet0/2` nối xuống `Access-SW2`.
  * [`Configs/Access-SW1.txt`](Configs/Access-SW1.txt): Quản lý VLAN 10 (HR) và VLAN 20 (Finance).
  * [`Configs/Access-SW2.txt`](Configs/Access-SW2.txt): Quản lý VLAN 30 (IT), VLAN 40 (Management), VLAN 50 (Servers) và VLAN 99 (Attacker).

---

## GIAI ĐOẠN 1: QUY HOẠCH MẠNG, ĐỊA CHỈ IP & ĐẶT TÊN THIẾT BỊ (NGÀY 3–4)

> **Lưu ý quan trọng:** File gốc trên GitHub chỉ có mạng LAN nội bộ, **chưa có Router biên và kết nối Internet**. Phần mở rộng Router biên + NAT/PAT là nhiệm vụ trọng tâm của bạn để bài nhóm đạt điểm cao và đúng mục tiêu trong [`Thiết kế mạng nhóm 8.pdf`](Thiết%20kế%20mạng%20nhóm%208.pdf).

### Bước 1.1: Chốt danh sách đổi tên thiết bị (Tránh bị đánh dấu copy bài mẫu)
* [x] Chốt quy ước đặt tên cho cả nhóm dùng thống nhất trong Packet Tracer, file Config và Báo cáo:
  * `CORE-SW` $\rightarrow$ **`CoreSW-Nhom8`**
  * `Access-SW1` $\rightarrow$ **`AccessSW1-Nhom8`** (TV2 sẽ đổi hoặc bạn đổi tên hiển thị trước)
  * `Access-SW2` $\rightarrow$ **`AccessSW2-Nhom8`**
  * Thiết bị bạn thêm mới 1: **`EdgeRouter-Nhom8`** (Router biên công ty)
  * Thiết bị bạn thêm mới 2: **`ISP-Internet`** (Router giả lập nhà mạng / Internet)

### Bước 1.2: Lập Bảng quy hoạch VLAN & IP (Dùng để dán vào Chương 2 Báo cáo)
* [x] Kiểm tra và chốt bảng phân chia mạng con (Subnet `/24`) dưới đây (giữ nguyên cấu trúc 6 VLAN nội bộ + bổ sung 2 dải IP Point-to-Point `/30` cho đường nối Router):

| Vùng mạng / VLAN | Bộ phận / Chức năng | Dải mạng (Subnet) | Subnet Mask | Default Gateway | Dải IP cấp cho thiết bị |
|:---:|---|---|---|---|---|
| **VLAN 10** | Human Resources (HR) | `192.168.10.0/24` | `255.255.255.0` | `192.168.10.1` | `192.168.10.2` – `192.168.10.254` |
| **VLAN 20** | Finance (Kế toán) | `192.168.20.0/24` | `255.255.255.0` | `192.168.20.1` | `192.168.20.2` – `192.168.20.254` |
| **VLAN 30** | IT Support (Kỹ thuật) | `192.168.30.0/24` | `255.255.255.0` | `192.168.30.1` | `192.168.30.2` – `192.168.30.254` |
| **VLAN 40** | Management (Giám đốc) | `192.168.40.0/24` | `255.255.255.0` | `192.168.40.1` | `192.168.40.2` – `192.168.40.254` |
| **VLAN 50** | Servers (Web, DNS, FTP) | `192.168.50.0/24` | `255.255.255.0` | `192.168.50.1` | `192.168.50.2` – `192.168.50.254` |
| **VLAN 99** | Attacker (Cô lập) | `192.168.99.0/24` | `255.255.255.0` | `192.168.99.1` | `192.168.99.2` – `192.168.99.254` |
| **Uplink L3** | CoreSW $\leftrightarrow$ EdgeRouter | `10.0.0.0/30` | `255.255.255.252` | — | CoreSW: `10.0.0.2`<br>EdgeRouter: `10.0.0.1` |
| **WAN Link** | EdgeRouter $\leftrightarrow$ ISP | `203.0.113.0/30` | `255.255.255.252` | — | EdgeRouter: `203.0.113.2`<br>ISP: `203.0.113.1` |
| **Internet** | Loopback giả lập DNS/Web ngoài | `8.8.8.0/24` | `255.255.255.0` | — | ISP `Loopback0`: `8.8.8.8` |

### Bước 1.3: Chốt sơ đồ nối cổng (Port Mapping) cho phần Core & Router
> **Chú ý kỹ thuật cực kỳ quan trọng:** Switch Layer 3 Cisco 3560 trong Packet Tracer chỉ có **2 cổng GigabitEthernet** (`Gi0/1` và `Gi0/2`), hiện đã dùng hết để nối xuống `Access-SW1` và `Access-SW2`. Vì vậy, đường nối từ `CoreSW-Nhom8` lên `EdgeRouter-Nhom8` sẽ dùng cổng **`FastEthernet0/24`** ở chế độ cổng định tuyến (`no switchport`).

* [x] Xác nhận bảng nối dây:
  * `CoreSW-Nhom8` (`Gi0/1`) $\longleftrightarrow$ `AccessSW1-Nhom8` (`Gi0/1`) *(Trunk 802.1Q - Đã có sẵn)*
  * `CoreSW-Nhom8` (`Gi0/2`) $\longleftrightarrow$ `AccessSW2-Nhom8` (`Gi0/1`) *(Trunk 802.1Q - Đã có sẵn)*
  * `CoreSW-Nhom8` (`Fa0/24`) $\longleftrightarrow$ `EdgeRouter-Nhom8` (`Gi0/0`) *(Dùng cáp thẳng - Copper Straight-Through)*
  * `EdgeRouter-Nhom8` (`Gi0/1`) $\longleftrightarrow$ `ISP-Internet` (`Gi0/0`) *(Dùng cáp chéo - Copper Cross-Over)*

---

## GIAI ĐOẠN 2: CHỈNH SỬA TOPOLOGY TRÊN CISCO PACKET TRACER (NGÀY 5)

### Bước 2.1: Lưu bản sao làm việc riêng
* [x] Mở file [`NETWORK SECURITY.pkt`](NETWORK%20SECURITY.pkt).
* [x] Chọn **File > Save As...** lưu ngay thành **`NETWORK_NHOM8_v1_Core.pkt`** để không làm hỏng file gốc.

### Bước 2.2: Đổi tên thiết bị & sắp xếp lại bố cục
* [x] Nhấp vào từng thiết bị trên màn hình (hoặc tab **Config > Display Name**), đổi tên hiển thị cho gọn: `CoreSW-Nhom8`, `AccessSW1-Nhom8`, `AccessSW2-Nhom8`.
* [ ] Nếu muốn, dùng nút **Draw Rectangle / Fill Color** trên thanh công cụ Packet Tracer để vẽ khung màu phân vùng lại rõ ràng từng phòng ban (HR, Finance, IT, Management, Servers, Attacker, Edge/ISP) kèm dòng chữ ghi tên nhóm: *"Hệ thống mạng doanh nghiệp an toàn - Nhóm 8"*.

### Bước 2.3: Thêm thiết bị Router biên & ISP và cắm dây
* [x] Ở góc dưới bên trái Packet Tracer, chọn **Network Devices > Routers**.
* [x] Kéo thả **2 Router Cisco 2911** (hoặc **1941**) lên phía trên `CoreSW-Nhom8`.
* [x] Đổi Display Name của Router thứ nhất thành **`EdgeRouter-Nhom8`**, Router thứ hai thành **`ISP-Internet`**.
* [x] Chọn mục **Connections** (biểu tượng tia sét):
  * Chọn **Copper Straight-Through** (cáp thẳng nét liền): Bấm vào `CoreSW-Nhom8` chọn **`FastEthernet0/24`**, kéo lên `EdgeRouter-Nhom8` chọn **`GigabitEthernet0/0`**.
  * Chọn **Copper Cross-Over** (cáp chéo nét đứt): Bấm vào `EdgeRouter-Nhom8` chọn **`GigabitEthernet0/1`**, kéo lên `ISP-Internet` chọn **`GigabitEthernet0/0`**.
* [x] *(Tùy chọn)* Thêm 1 biểu tượng đám mây hoặc vẽ khung màu xanh dương bao quanh `ISP-Internet` ghi nhãn `MẠNG INTERNET (ISP - 8.8.8.8)`.

---

## GIAI ĐOẠN 3: VIẾT SCRIPT VÀ CẤU HÌNH CLI CHO CORE & ROUTER (NGÀY 6–7)

> **Nguyên tắc làm việc:** Viết/sửa toàn bộ lệnh trên **VS Code** trước để kiểm tra kỹ rồi mới Copy & Paste vào cửa sổ CLI trong Packet Tracer. Điều này giúp bạn có sẵn file `.txt` sạch sẽ để nộp và chèn vào Báo cáo.

### Bước 3.1: Cấu hình `CoreSW-Nhom8` (Cập nhật file `Configs/CORE-SW-Nhom8.txt`)
* [x] Mở file [`Configs/CORE-SW-Nhom8.txt`](Configs/CORE-SW-Nhom8.txt) trong VS Code, kiểm tra lại phần định tuyến Core và bổ sung phần Uplink + Default Route lên EdgeRouter:
  * Đổi `hostname CORE-SW` thành `hostname CoreSW-Nhom8`.
  * Đảm bảo đã có `ip routing`, các `vlan 10, 20, 30, 40, 50, 99`, các `interface Vlan10`..`Vlan99` với Gateway `.1`, và 2 cổng trunk `Gi0/1`, `Gi0/2`.
  * **Bổ sung thêm đoạn lệnh kết nối lên EdgeRouter ở trước dòng `end`:**
    ```cisco
    ! === Uplink L3 to EdgeRouter-Nhom8 ===
    interface FastEthernet0/24
     description Uplink to EdgeRouter-Nhom8
     no switchport
     ip address 10.0.0.2 255.255.255.252
     no shutdown
    !
    ! Default route đẩy lưu lượng ra Internet qua EdgeRouter
    ip route 0.0.0.0 0.0.0.0 10.0.0.1
    ```
  * *(Lưu ý: Giữ nguyên các phần ACL và SSH có sẵn trong `CORE-SW-Nhom8.txt`, phần đó Thành viên 2 sẽ rà soát và tinh chỉnh lại theo yêu cầu Nhóm 8 ở Bước v2).*
* [x] Mở `CoreSW-Nhom8` trong Packet Tracer > Chọn tab **CLI**:
  * Gõ `enable` $\rightarrow$ `configure terminal`.
  * Copy phần lệnh đổi `hostname`, cấu hình `FastEthernet0/24` và `ip route 0.0.0.0 0.0.0.0 10.0.0.1` dán vào CLI.
  * Gõ `end` rồi gõ **`write memory`** để lưu cấu hình vào NVRAM.

### Bước 3.2: Tạo và cấu hình `EdgeRouter-Nhom8` (File mới `Configs/EdgeRouter-Nhom8.txt`)
* [x] Trong VS Code, tạo file mới tên là `Configs/EdgeRouter-Nhom8.txt` và soạn đoạn lệnh sau:
  ```cisco
  enable
  configure terminal
  hostname EdgeRouter-Nhom8

  ! 1. Cổng hướng về mạng nội bộ công ty (Inside)
  interface GigabitEthernet0/0
   description Link to CoreSW-Nhom8 (Fa0/24)
   ip address 10.0.0.1 255.255.255.252
   ip nat inside
   no shutdown
  !
  ! 2. Cổng hướng ra mạng Internet / ISP (Outside)
  interface GigabitEthernet0/1
   description Link to ISP-Internet (Gi0/0)
   ip address 203.0.113.2 255.255.255.252
   ip nat outside
   no shutdown
  !
  ! 3. Default Route đẩy mọi lưu lượng ra ISP
  ip route 0.0.0.0 0.0.0.0 203.0.113.1
  !
  ! 4. Static Return Routes trỏ về các VLAN nội bộ qua CoreSW (CỰC KỲ QUAN TRỌNG)
  ip route 192.168.10.0 255.255.255.0 10.0.0.2
  ip route 192.168.20.0 255.255.255.0 10.0.0.2
  ip route 192.168.30.0 255.255.255.0 10.0.0.2
  ip route 192.168.40.0 255.255.255.0 10.0.0.2
  ip route 192.168.50.0 255.255.255.0 10.0.0.2
  !
  ! 5. Cấu hình NAT/PAT (Overload) cho 5 VLAN hợp lệ & CoreSW Uplink (Không NAT cho VLAN 99 Attacker)
  access-list 1 permit 10.0.0.0 0.0.0.3
  access-list 1 permit 192.168.10.0 0.0.0.255
  access-list 1 permit 192.168.20.0 0.0.0.255
  access-list 1 permit 192.168.30.0 0.0.0.255
  access-list 1 permit 192.168.40.0 0.0.0.255
  access-list 1 permit 192.168.50.0 0.0.0.255
  ip nat inside source list 1 interface GigabitEthernet0/1 overload
  !
  end
  write memory
  ```
* [x] Mở `EdgeRouter-Nhom8` trong Packet Tracer > Tab **CLI** (nếu hỏi *Continue with configuration dialog? [yes/no]* thì gõ `no` + Enter).
* [x] Paste toàn bộ script trên vào CLI của `EdgeRouter-Nhom8` và kiểm tra không có dòng báo lỗi `% Invalid input`.

### Bước 3.3: Tạo và cấu hình `ISP-Internet` (File mới `Configs/ISP-Internet.txt`)
* [x] Trong VS Code, tạo file mới `Configs/ISP-Internet.txt`:
  ```cisco
  enable
  configure terminal
  hostname ISP-Internet

  ! Cổng kết nối với EdgeRouter của công ty
  interface GigabitEthernet0/0
   description Link to EdgeRouter-Nhom8 (Gi0/1)
   ip address 203.0.113.1 255.255.255.252
   no shutdown
  !
  ! Giao diện ảo giả lập máy chủ ngoài Internet để test Ping
  interface Loopback0
   description Simulated Internet Server (8.8.8.8)
   ip address 8.8.8.8 255.255.255.0
   no shutdown
  !
  end
  write memory
  ```
* [x] Mở `ISP-Internet` trong Packet Tracer > Tab **CLI** (gõ `no` nếu có dialog) > Paste toàn bộ lệnh trên và gõ `write memory`.

---

## GIAI ĐOẠN 4: TỰ KIỂM TRA TẦNG CORE & BÀN GIAO CHO THÀNH VIÊN 2 (NGÀY 8)

Trước khi chuyển file cho Thành viên 2 làm bảo mật & server, bạn **phải tự chạy kiểm tra thông suốt** các hạng mục của mình để chắc chắn hạ tầng Core + NAT không có lỗi:

### Bước 4.1: Checklist kiểm tra kỹ thuật trên CLI
* [x] Đèn nối giữa `CoreSW-Nhom8` $\leftrightarrow$ `EdgeRouter-Nhom8` $\leftrightarrow$ `ISP-Internet` đều chuyển sang **màu xanh lá**.
* [x] Trên `CoreSW-Nhom8`, chạy các lệnh:
  * `show ip interface brief`: Kiểm tra `Vlan10..99` và `FastEthernet0/24` (`10.0.0.2`) đều ở trạng thái `up / up`.
  * `show ip route`: Kiểm tra thấy đủ các tuyến `C` (Connected) cho các VLAN và tuyến `S* 0.0.0.0/0 [1/0] via 10.0.0.1`.
  * `ping 10.0.0.1`: Thành công (`!!!!!`).
  * `ping 8.8.8.8`: Thành công (`!!!!!`).
* [x] Trên `EdgeRouter-Nhom8`, chạy các lệnh:
  * `show ip route`: Thấy tuyến `S* 0.0.0.0/0 via 203.0.113.1` và 5 tuyến `S` trả về `192.168.10.0`..`50.0 via 10.0.0.2`.
  * `ping 8.8.8.8`: Thành công.
* [x] Trên 1 PC bất kỳ (ví dụ PC phòng Finance hoặc IT):
  * Mở **Command Prompt** gõ `ping 192.168.20.1` (ping Gateway) $\rightarrow$ Thành công.
  * Gõ **`ping 8.8.8.8`** (ping ra Internet qua NAT/PAT) $\rightarrow$ **Phải thành công (`Reply from 8.8.8.8`)!**
* [x] Quay lại CLI của `EdgeRouter-Nhom8` gõ **`show ip nat translations`**:
  * Phải nhìn thấy dòng dịch địa chỉ từ `192.168.x.x` (Inside local) thành `203.0.113.2` (Inside global) đi tới `8.8.8.8`! *(Chụp sẵn 1 tấm ảnh này để dùng cho Báo cáo hoặc gửi TV3 tham khảo).*

### Bước 4.2: Lưu và bàn giao cho Thành viên 2
* [x] Bấm `Save` trên Packet Tracer cho file `NETWORK_NHOM8_v1_Core.pkt`.
* [ ] Upload file `NETWORK_NHOM8_v1_Core.pkt` lên Google Drive (`01_PacketTracer_Versions/`) và các file `.txt` (`CORE-SW-Nhom8.txt`, `EdgeRouter-Nhom8.txt`, `ISP-Internet.txt`) vào `02_Configs_CLI/`.
* [ ] Nhắn tin cho Thành viên 2: *"Tớ xong bản v1 (Core, Router biên, NAT ra 8.8.8.8 đã ping thông). Cậu tải `NETWORK_NHOM8_v1_Core.pkt` về làm tiếp Access Switch, ACL, SSH và Web/DNS Server nhé!"*

---

## GIAI ĐOẠN 5: VẼ SƠ ĐỒ MẠNG & VIẾT BÁO CÁO WORD (TUẦN 2–3)

Trong thời gian chờ Thành viên 2 cấu hình bảo mật và Thành viên 3 chạy kiểm thử, bạn làm song song 2 đầu việc tài liệu quan trọng này:

### Bước 5.1: Vẽ sơ đồ kiến trúc mạng chuyên nghiệp trên `draw.io`
* [x] Truy cập [app.diagrams.net](https://app.diagrams.net/) > Chọn **Create New Diagram > Blank Diagram**.
* [x] Ở thanh bên trái phía dưới, bấm **+ More Shapes...** > Tìm mục **Networking** > Tích chọn **Cisco** (hoặc **Cisco 19**) > Bấm **Apply**.
* [x] Vẽ 2 sơ đồ chuẩn để phục vụ Báo cáo & Slide:
  1. **Sơ đồ 1: Sơ đồ kiến trúc tổng thể (Logical Topology)** (`docs/SoDo_1_Topology_TongThe.drawio`): Thể hiện rõ 3 tầng (Edge/ISP $\rightarrow$ Core Layer `CoreSW-Nhom8` $\rightarrow$ Access Layer `AccessSW1-Nhom8`, `AccessSW2-Nhom8` $\rightarrow$ 6 vùng VLAN màu sắc phân biệt + ghi chú Subnet `/24`, Gateway `.1` và đường link `/30`).
  2. **Sơ đồ 2: Sơ đồ luồng định tuyến & kiểm soát bảo mật (Traffic Flow & Security Zones)** (`docs/SoDo_2_LuongDuLieu.drawio`): Chỉ rõ chiều đi của gói tin hợp lệ ra Internet (qua NAT), chiều gói tin bị chặn giữa HR $\leftrightarrow$ Finance và vùng Attacker bị cô lập.
* [x] Lưu trữ file `draw.io` tại thư mục `docs/` (`docs/SoDo_1_Topology_TongThe.drawio`, `docs/SoDo_2_LuongDuLieu.drawio`) phục vụ trích xuất ảnh chèn vào Báo cáo & Slide.

### Bước 5.2: Viết Chương 1 & Chương 2 của Báo cáo kỹ thuật (trên Word / Google Docs)
* [ ] **Chương 1: Tổng quan đề tài & Phân tích yêu cầu hệ thống (khoảng 5–7 trang):**
  * **1.1. Đặt vấn đề & Bối cảnh doanh nghiệp:** Mô tả doanh nghiệp công nghệ quy mô 100–200 nhân viên gồm các phòng ban (HR, Finance, IT, Management) dùng chung hạ tầng dễ gặp rủi ro bảo mật nội bộ, lộ lọt dữ liệu lương/tài chính, broadcast storm.
  * **1.2. Mục tiêu và Phạm vi dự án:** Phân tách VLAN, kiểm soát truy cập bằng ACL, bảo mật quản trị qua SSHv2, kết nối Internet qua NAT/PAT và kiểm thử bằng kịch bản giả lập Attacker.
  * **1.3. Phân tích yêu cầu kỹ thuật & Ma trận quyền truy cập:** Lập bảng ai được phép truy cập ai (ví dụ: HR $\leftrightarrow$ Finance = Cấm; Attacker $\rightarrow$ Toàn mạng = Cấm; IT/Mgmt $\rightarrow$ CoreSW SSH = Cho phép; Hợp lệ $\rightarrow$ Server/Internet = Cho phép).
  * **1.4. Lựa chọn công nghệ & Công cụ:** Giới thiệu lý do dùng Cisco Packet Tracer, Switch L3 3560 (SVI routing), Switch L2 2960, chuẩn 802.1Q, Extended ACL, NAT/PAT.

* [ ] **Chương 2: Thiết kế hệ thống mạng & Triển khai hạ tầng Core (khoảng 8–10 trang):**
  * **2.1. Thiết kế mô hình mạng phân cấp (Core – Access – Edge):** Chèn ảnh sơ đồ `draw.io` đã vẽ ở Bước 5.1, giải thích vai trò của từng thiết bị (`CoreSW-Nhom8`, `AccessSW1/2-Nhom8`, `EdgeRouter-Nhom8`, `ISP-Internet`).
  * **2.2. Quy hoạch VLAN và Địa chỉ IP:** Chèn Bảng quy hoạch VLAN & Subnetting ở Bước 1.2, giải thích cách chia IP hợp lý cho 100–200 nhân viên (`/24` hỗ trợ tối đa 254 host/phòng ban, dư địa mở rộng tốt).
  * **2.3. Thiết kế định tuyến Inter-VLAN (SVI) và Kết nối Internet (NAT/PAT):** Giải thích tại sao chọn SVI trên Switch L3 3560 thay vì Router-on-a-Stick (hiệu năng chuyển mạch phần cứng ASIC cao hơn, tránh nghẽn cổ chai). Giải thích nguyên lý NAT Overload (`ip nat inside/outside`) và Static Route hai chiều.
  * **2.4. Chi tiết cấu hình thiết bị Core & Router biên:** Trích dẫn các đoạn lệnh trong `Configs/CORE-SW.txt`, `Configs/EDGE-ROUTER.txt`, `Configs/ISP-ROUTER.txt` kèm chú thích giải thích ý nghĩa từng lệnh bằng tiếng Việt.

* [ ] Gửi file Word Chương 1 & Chương 2 lên Drive cho Thành viên 3 ghép vào quyển Báo cáo tổng.

---

## GIAI ĐOẠN 6: HOÀN THIỆN, LUYỆN DEMO & VẤN ĐÁP (TUẦN 4)

### Bước 6.1: Tải bản chốt cuối cùng & Tự thực hành trên máy cá nhân
* [ ] Khi Thành viên 3 hoàn thành xong bản **`NETWORK_NHOM8_FINAL.pkt`**, bạn **bắt buộc tải về lưu trên laptop của mình**.
* [ ] Tự mở `NETWORK_NHOM8_FINAL.pkt`, gõ lại các lệnh kiểm tra sau để quen tay khi thầy cô hỏi:
  * `show vlan brief` (trên CoreSW & AccessSW)
  * `show interfaces trunk` (kiểm tra đường trunk 802.1Q)
  * `show ip route` (trên CoreSW & EdgeRouter)
  * `show ip nat translations` (trên EdgeRouter sau khi ping `8.8.8.8` từ PC)
  * `show ip access-lists` (xem các bộ lọc ACL và số packet matched)

### Bước 6.2: Điều phối nhóm tổng duyệt thuyết trình & Demo
* [ ] Với tư cách Trưởng nhóm, bạn nên là người **mở đầu buổi thuyết trình** (trình bày phần Giới thiệu đề tài, Mô hình kiến trúc, Quy hoạch IP và Hạ tầng Core/NAT) trước khi nhường phần Bảo mật ACL cho TV2 và phần Demo/Kết quả kiểm thử cho TV3.
* [ ] Chuẩn bị trả lời trôi chảy **3 câu hỏi tủ thuộc phần của bạn** nếu giảng viên hỏi:
  1. *"Tại sao dùng Switch Layer 3 (SVI) để định tuyến giữa các VLAN mà không dùng Router-on-a-Stick?"*
     > **Trả lời:** SVI định tuyến bằng phần cứng (ASIC) bên trong Core Switch với tốc độ wire-speed, hỗ trợ băng thông cao cho doanh nghiệp 100–200 nhân viên mà không bị nghẽn cổ chai trên 1 sợi cáp uplink như Router-on-a-Stick.
  2. *"Tại sao trên `CoreSW-Nhom8` bạn phải dùng cổng `Fa0/24` với lệnh `no switchport` để nối lên `EdgeRouter`?"*
     > **Trả lời:** Vì Cisco 3560 chỉ có 2 cổng GigabitEthernet (`Gi0/1` và `Gi0/2`) đã nối trunk xuống 2 Access Switch. Lệnh `no switchport` biến cổng Layer 2 `Fa0/24` thành cổng định tuyến Layer 3 (Routed Port) để gán trực tiếp IP `/30` kết nối Point-to-Point với `EdgeRouter`.
  3. *"Tại sao đã có `ip route 0.0.0.0 0.0.0.0` trên Core Switch mà trên `EdgeRouter` vẫn phải thêm static route trả về `192.168.10.0`..`50.0`, và chứng minh NAT/PAT hoạt động như thế nào?"*
     > **Trả lời:** Default route trên Core Switch chỉ giúp gói tin đi ra ngoài; khi gói tin phản hồi từ `8.8.8.8` quay về `EdgeRouter` và được giải NAT (un-NAT) về IP private `192.168.x.x`, `EdgeRouter` bắt buộc phải có tuyến tĩnh trỏ ngược về `10.0.0.2` (CoreSW) thì gói tin mới về được PC. Khi chạy lệnh `show ip nat translations` trên `EdgeRouter`, ta thấy IP private nội bộ đã được ánh xạ sang IP public `203.0.113.2` kèm số port (PAT/Overload) khi giao tiếp với `8.8.8.8`, trong khi `ISP-Internet` hoàn toàn không cần biết đến mạng `192.168.x.x`.
