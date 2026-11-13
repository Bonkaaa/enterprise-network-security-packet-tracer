# Thiết Kế và Mô Phỏng Hệ Thống Mạng Doanh Nghiệp An Toàn Với VLAN, ACL và Cô Lập Attacker

**Đồ án môn học: Thiết kế, Cài đặt và Quản trị Mạng — Nhóm 8**  
*Mô phỏng hoàn chỉnh trên Cisco Packet Tracer (Bản 8.2 trở lên)*

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco%20Packet%20Tracer-9.0%2B-1E90FF?style=for-the-badge&logo=cisco)
![Network Security](https://img.shields.io/badge/Bảo%20Mật%20Mạng-VLAN%20%7C%20ACL%20%7C%20SSHv2%20%7C%20NAT-FF4500?style=for-the-badge)
![Status](https://img.shields.io/badge/Trạng%20Thái-Hoàn%20Thành%20v1%20Core%20%26%20NAT-32CD32?style=for-the-badge)

---

## 📖 Mục Lục
- [1. Tổng Quan Đề Tài](#1-tổng-quan-đề-tài)
- [2. Cấu Trúc Repository](#2-cấu-trúc-repository)
- [3. Kiến Trúc Mạng Phân Cấp (3-Tier Architecture)](#3-kiến-trúc-mạng-phân-cấp-3-tier-architecture)
- [4. Bảng Quy Hoạch VLAN & Địa Chỉ IP](#4-bảng-quy-hoạch-vlan--địa-chỉ-ip)
- [5. Sơ Đồ Kiến Trúc & Luồng Dữ Liệu (draw.io)](#5-sơ-đồ-kiến-trúc--luồng-dữ-liệu-drawio)
- [6. Các Chính Sách Bảo Mật Cốt Lõi](#6-các-chính-sách-bảo-mật-cốt-lõi)
- [7. Kịch Bản Kiểm Thử Hệ Thống](#7-kịch-bản-kiểm-thử-hệ-thống)
- [8. Hướng Dẫn Sử Dụng & Mở Mô Phỏng](#8-hướng-dẫn-sử-dụng--mở-mô-phỏng)
- [9. Phân Công Trách Nhiệm Thành Viên](#9-phân-công-trách-nhiệm-thành-viên)

---

## 1. Tổng Quan Đề Tài

Dự án tập trung thiết kế và triển khai một hạ tầng mạng hoàn chỉnh, an toàn cho doanh nghiệp quy mô vừa (100–200 nhân viên). Hệ thống giải quyết các bài toán an ninh mạng nội bộ và kết nối biên thông qua:
* **Phân vùng mạng (VLAN Segmentation):** Tách biệt các bộ phận (Nhân sự - HR, Kế toán - Finance, CNTT - IT, Ban Giám đốc - Management, Hệ thống máy chủ - Server Farm) nhằm thu hẹp Broadcast domain và ngăn ngừa mã độc lây lan diện rộng.
* **Định tuyến Inter-VLAN tốc độ cao:** Triển khai SVI (Switch Virtual Interface) trên Switch Layer 3 Core (Cisco Catalyst 3560) giúp tối ưu hóa hiệu năng chuyển mạch phần cứng.
* **Kiểm soát truy cập đa lớp (Extended ACLs):** Ngăn chặn hành vi di chuyển ngang (lateral movement) giữa các phòng ban nhạy cảm (HR ↔ Finance) và cô lập hoàn toàn phân vùng máy tính tấn công (VLAN 99 Attacker).
* **Quản trị an toàn (Management Hardening):** Vô hiệu hóa Telnet, áp dụng chuẩn mã hóa SSHv2 kèm khóa RSA 1024-bit và lọc IP quản trị bằng Standard ACL trên cổng VTY.
* **Kết nối Internet an toàn (Edge Routing & NAT/PAT):** Bổ sung Router biên doanh nghiệp (`EdgeRouter-Nhom8`) thực hiện PAT (Port Address Translation / NAT Overload) cho phép các phòng ban hợp lệ truy cập ra mạng Internet mô phỏng (`8.8.8.8` tại `ISP-Internet`), đồng thời chặn không cho phân vùng nghi nhiễm (Attacker) ra ngoài.

---

## 2. Cấu Trúc Repository

```text
├── Configs/                             # Kịch bản lệnh CLI đã chuẩn hóa theo thiết bị Nhóm 8
│   ├── CORE-SW-Nhom8.txt                # Cấu hình Switch L3 (VLAN, SVI, Trunk, ACL, SSH, Uplink Fa0/24)
│   ├── EdgeRouter-Nhom8.txt             # Cấu hình Router biên (Inside/Outside, Static Route, NAT/PAT)
│   ├── ISP-Internet.txt                 # Cấu hình Router ISP (Gi0/0, Loopback0 8.8.8.8)
│   ├── Access-SW1-Nhom8.txt             # Cấu hình Access Switch 1 (VLAN 10 HR, VLAN 20 Finance)
│   └── Access-SW2-Nhom8.txt             # Cấu hình Access Switch 2 (IT, Mgmt, Servers, Attacker)
├── docs/                                # Tài liệu thiết kế, sơ đồ và kế hoạch phân công
│   ├── PROJECT_PLAN.md                  # Kế hoạch triển khai tổng thể & lộ trình 4 tuần của nhóm
│   ├── PLAN_THANH_VIEN_1.md             # Kế hoạch chi tiết của Trưởng nhóm (Core Network & Routing)
│   ├── Screenshots.md                   # Ảnh chụp bằng chứng cấu hình và kết quả kiểm thử CLI
│   ├── SoDo_1_Topology_TongThe.drawio   # Sơ đồ kiến trúc mạng tổng thể 3 tầng (draw.io)
│   └── SoDo_2_LuongDuLieu.drawio        # Sơ đồ luồng định tuyến và các vùng kiểm soát bảo mật (draw.io)
├── NETWORK SECURITY.pkt                 # File mô phỏng Cisco Packet Tracer gốc
├── NETWORK_NHOM8_v1_Core.pkt            # File mô phỏng hoàn thiện bản v1 của Nhóm 8 (Đã có Router & NAT)
├── README.md                            # Tài liệu giới thiệu tổng quan dự án
└── Thiết kế mạng nhóm 8.pdf             # Đề cương yêu cầu và kịch bản thiết kế của Nhóm 8
```

---

## 3. Kiến Trúc Mạng Phân Cấp (3-Tier Architecture)

Mô hình mạng được xây dựng theo kiến trúc phân tầng chuẩn Cisco:

```text
                          [ ISP-Internet ] (Cisco 2911)
                                 │ Loopback0: 8.8.8.8/24
                                 │
                   WAN Link: 203.0.113.0/30 (Cáp chéo)
                                 │
                        [ EdgeRouter-Nhom8 ] (Cisco 2911)
                                 │ Gi0/0 (10.0.0.1) - NAT Inside / Outside
                                 │
                  Uplink L3: 10.0.0.0/30 (Cáp thẳng)
                                 │ Fa0/24 (10.0.0.2 - no switchport)
                                 │
                         [ CoreSW-Nhom8 ] (Catalyst 3560-24PS)
                        ╱ (Gi0/1)              ╲ (Gi0/2)
                 Trunk 802.1Q                  Trunk 802.1Q
                      ╱                              ╲
            [ AccessSW1-Nhom8 ]              [ AccessSW2-Nhom8 ]
             (Catalyst 2960)                  (Catalyst 2960)
             ├── VLAN 10 (HR)                 ├── VLAN 30 (IT)
             └── VLAN 20 (Finance)            ├── VLAN 40 (Management)
                                              ├── VLAN 50 (Server Room)
                                              └── VLAN 99 (Compromised/Attacker)
```

* **Tầng Biên (Edge & ISP Layer):** 
  * `EdgeRouter-Nhom8`: Đóng vai trò tường lửa/router biên, thực thi định tuyến mặc định ra ISP và dịch địa chỉ mạng (NAT Overload) cho dải mạng nội bộ.
  * `ISP-Internet`: Giả lập nhà cung cấp dịch vụ Internet với cổng `Loopback0` mang IP `8.8.8.8` dùng để kiểm thử kết nối ngoại mạng.
* **Tầng Lõi (Core Layer):** `CoreSW-Nhom8` đảm nhận định tuyến Inter-VLAN thông qua các giao diện ảo SVI với băng thông dây (wire-speed), gắn các bộ lọc Extended ACL kiểm soát lưu lượng giữa các phòng ban. Cổng `Fa0/24` được chuyển đổi thành Routed Port (`no switchport`) để kết nối L3 trực tiếp lên `EdgeRouter-Nhom8`.
* **Tầng Truy Cập (Access Layer):** Gồm `AccessSW1-Nhom8` và `AccessSW2-Nhom8`, cấu hình các cổng Access nối tới máy trạm, máy chủ và thiết bị kiểm thử, bật tính năng `spanning-tree portfast` để tối ưu thời gian hội tụ mạng.

---

## 4. Bảng Quy Hoạch VLAN & Địa Chỉ IP

| Vùng mạng / VLAN | Bộ phận / Chức năng | Dải mạng (Subnet) | Subnet Mask | Default Gateway | Dải IP cấp cho thiết bị | Ghi chú thiết bị |
|:---:|---|---|---|---|---|---|
| **VLAN 10** | Human Resources (HR) | `192.168.10.0/24` | `255.255.255.0` | `192.168.10.1` | `.2` – `.254` | 4 PC / Laptop |
| **VLAN 20** | Finance (Kế toán) | `192.168.20.0/24` | `255.255.255.0` | `192.168.20.1` | `.2` – `.254` | 4 PC / Laptop |
| **VLAN 30** | IT Support (Kỹ thuật) | `192.168.30.0/24` | `255.255.255.0` | `192.168.30.1` | `.2` – `.254` | 4 PC / Laptop (Được quyền SSH) |
| **VLAN 40** | Management (Giám đốc) | `192.168.40.0/24` | `255.255.255.0` | `192.168.40.1` | `.2` – `.254` | 2 PC / Laptop (Được quyền SSH) |
| **VLAN 50** | Critical Servers | `192.168.50.0/24` | `255.255.255.0` | `192.168.50.1` | `.2` – `.254` | Web, DNS, FTP Server |
| **VLAN 99** | Compromised / Attacker | `192.168.99.0/24` | `255.255.255.0` | `192.168.99.1` | `.2` – `.254` | Attacker PC (Bị cô lập hoàn toàn) |
| **Uplink L3** | CoreSW ↔ EdgeRouter | `10.0.0.0/30` | `255.255.255.252` | — | `10.0.0.1` – `10.0.0.2` | Core: `.2` (Fa0/24), Edge: `.1` (Gi0/0) |
| **WAN Link** | EdgeRouter ↔ ISP | `203.0.113.0/30` | `255.255.255.252` | — | `203.0.113.1` – `.2` | Edge: `.2` (Gi0/1), ISP: `.1` (Gi0/0) |
| **Internet** | Simulated Internet (DNS) | `8.8.8.0/24` | `255.255.255.0` | — | `8.8.8.8` | Loopback0 trên ISP-Internet |

---

## 5. Sơ Đồ Kiến Trúc & Luồng Dữ Liệu (draw.io)

Các file sơ đồ định dạng XML mở được trực tiếp trên [draw.io (Diagrams.net)](https://app.diagrams.net/):
1. **[docs/SoDo_1_Topology_TongThe.drawio](docs/SoDo_1_Topology_TongThe.drawio):** Thể hiện chi tiết sơ đồ mạng phân cấp 3 tầng (Edge, Core, Access), toàn bộ cổng giao tiếp (Port labels), địa chỉ IP Point-to-Point `/30` và vùng VLAN màu sắc trực quan.
2. **[docs/SoDo_2_LuongDuLieu.drawio](docs/SoDo_2_LuongDuLieu.drawio):** Thể hiện chi tiết luồng dữ liệu hợp lệ đi qua CoreSW và EdgeRouter (NAT Overload) để ra Internet, cũng như cơ chế hủy gói tin tại các ranh giới ACL Inbound khi có truy cập trái phép.

---

## 6. Các Chính Sách Bảo Mật Cốt Lõi

### 6.1. Ngăn Chặn Di Chuyển Ngang (Lateral Movement Prevention)
* Áp dụng **Extended ACL** theo chiều **Inbound** tại giao diện SVI của từng VLAN trên Core Switch:
  * `HR-ACL` trên `interface Vlan10`: Chặn toàn bộ lưu lượng có đích đến là mạng Finance (`192.168.20.0/24`).
  * `FIN-ACL` trên `interface Vlan20`: Chặn toàn bộ lưu lượng có đích đến là mạng HR (`192.168.10.0/24`).
  * Cho phép các phòng ban này truy cập bình thường tới Server Farm (`VLAN 50`) và Internet (`8.8.8.8`).

### 6.2. Cô Lập Tuyệt Đối Vùng Nghi Nhiễm (Attacker Isolation)
* Áp dụng `BLOCK-ATTACKER` trên `interface Vlan99`:
  ```cisco
  ip access-list extended BLOCK-ATTACKER
   deny ip 192.168.99.0 0.0.0.255 any
   deny ip any 192.168.99.0 0.0.0.255
   permit ip any any
  ```
  Attacker PC hoàn toàn bị chặn khi cố gắng ping, quét cổng hoặc gửi bất kỳ gói tin nào vào hạ tầng nội bộ hoặc ra ngoài Internet.

### 6.3. Bảo Mật Quản Trị Từ Xa Bằng SSHv2
* Thay thế Telnet không an toàn bằng giao thức SSHv2 có mã hóa khóa RSA 1024-bit.
* Áp dụng `access-class SSH-MGMT in` trên `line vty 0 15`:
  ```cisco
  ip access-list standard SSH-MGMT
   permit 192.168.30.0 0.0.0.255
   permit 192.168.40.0 0.0.0.255
   deny any
  ```
  Chỉ có các thiết bị thuộc phân vùng Kỹ thuật IT (`VLAN 30`) và Ban Giám đốc (`VLAN 40`) mới có quyền mở phiên SSH vào quản trị `CoreSW-Nhom8`. Mọi nỗ lực SSH từ phòng ban khác đều bị ngắt kết nối lập tức (`Connection refused`).

### 6.4. Biên Dịch Địa Chỉ Mạng (NAT/PAT Overload)
* Cấu hình trên `EdgeRouter-Nhom8`:
  ```cisco
  access-list 1 permit 10.0.0.0 0.0.0.3
  access-list 1 permit 192.168.10.0 0.0.0.255
  access-list 1 permit 192.168.20.0 0.0.0.255
  access-list 1 permit 192.168.30.0 0.0.0.255
  access-list 1 permit 192.168.40.0 0.0.0.255
  access-list 1 permit 192.168.50.0 0.0.0.255
  ip nat inside source list 1 interface GigabitEthernet0/1 overload
  ```
  Toàn bộ các gói tin hợp lệ từ mạng nội bộ và Core Switch khi đi ra ngoài Internet đều được biên dịch địa chỉ nguồn thành IP Public `203.0.113.2`. Phân vùng `VLAN 99` không nằm trong danh sách NAT nên không thể liên lạc ra thế giới bên ngoài.

---

## 7. Kịch Bản Kiểm Thử Hệ Thống

| STT | Kịch Bản Kiểm Thử | Lệnh Kiểm Tra / Thao Tác | Kết Quả Mong Đợi | Trạng Thái |
|:---:|---|---|---|:---:|
| 1 | Các PC trong cùng VLAN kết nối với nhau | `ping 192.168.10.3` từ máy `HR-PC1` | Thành công 100% (`Reply from...`) | ✅ Đã kiểm chứng |
| 2 | Nhân viên HR truy cập máy phòng Finance | `ping 192.168.20.2` từ máy `HR-PC1` | **Bị chặn 100%** do `HR-ACL` (Destination Host Unreachable) | ✅ Đã kiểm chứng |
| 3 | Nhân viên Finance truy cập máy phòng HR | `ping 192.168.10.2` từ máy `FIN-PC1` | **Bị chặn 100%** do `FIN-ACL` | ✅ Đã kiểm chứng |
| 4 | Máy trạm phòng ban truy cập Internet | `ping 8.8.8.8` từ máy `FIN-PC1` / `IT-PC1` | **Thành công** qua cơ chế PAT trên EdgeRouter | ✅ Đã kiểm chứng |
| 5 | Kiểm tra bảng dịch địa chỉ NAT | `show ip nat translations` trên `EdgeRouter-Nhom8` | Hiển thị bản dịch từ `192.168.x.x` sang `203.0.113.2` | ✅ Đã kiểm chứng |
| 6 | Nhân viên IT kết nối quản trị SSH | `ssh -l ITADMIN 192.168.30.1` từ `IT-PC1` | Đăng nhập thành công vào CLI `CoreSW-Nhom8` | ✅ Đã kiểm chứng |
| 7 | Nhân viên Finance cố kết nối SSH vào CoreSW | `ssh -l ITADMIN 192.168.20.1` từ `FIN-PC1` | **Bị từ chối** (`Connection closed / refused`) | ✅ Đã kiểm chứng |
| 8 | Attacker cố tình ping quét mạng nội bộ | `ping 192.168.50.10` từ `ATTACKER-PC` | **Bị chặn 100%** do `BLOCK-ATTACKER` | ✅ Đã kiểm chứng |
| 9 | Attacker cố tình truy cập ra Internet | `ping 8.8.8.8` từ `ATTACKER-PC` | **Bị chặn 100%** (Không được cấp quyền NAT) | ✅ Đã kiểm chứng |
| 10 | Bắt gói tin và chứng minh bằng Simulation | Chế độ Simulation Mode (bắt gói ICMP/TCP) | Gói tin xuất hiện dấu X đỏ tại SVI trên CoreSW | ✅ Đã kiểm chứng |

---

## 8. Hướng Dẫn Sử Dụng & Mở Mô Phỏng

1. **Yêu cầu môi trường:** Đã cài đặt **Cisco Packet Tracer** (khuyến nghị phiên bản 8.2 hoặc 9.0 trở lên).
2. **Mở file mô phỏng:** Mở trực tiếp file [`NETWORK_NHOM8_v1_Core.pkt`](NETWORK_NHOM8_v1_Core.pkt) trong Packet Tracer.
3. **Cách nạp cấu hình mới qua CLI (nếu cần cấu hình lại từ đầu):**
   * Mở tab **CLI** của từng thiết bị tương ứng.
   * Copy toàn bộ nội dung trong các file text tại thư mục [`Configs/`](Configs/) và dán (Paste) vào cửa sổ dòng lệnh.
   * Chạy lệnh `write memory` để lưu lại cấu hình vào bộ nhớ NVRAM.

---

## 9. Phân Công Trách Nhiệm Thành Viên

* **Thành viên 1 (Trưởng nhóm — Core Network & Routing Lead):**
  * Thiết kế kiến trúc phân cấp, quy hoạch bảng IP/VLAN và sơ đồ cổng nối.
  * Cấu hình tầng mạng Lõi (`CoreSW-Nhom8`), định tuyến L3, Router biên (`EdgeRouter-Nhom8`), ISP và dịch vụ NAT/PAT.
  * Vẽ sơ đồ logic & sơ đồ luồng dữ liệu chuyên nghiệp trên `draw.io`.
  * Soạn thảo **Chương 1** (Tổng quan đề tài) và **Chương 2** (Thiết kế hệ thống & Cấu hình Core) trong Báo cáo Word.
* **Thành viên 2 (Kỹ sư An ninh Mạng & Dịch vụ — Security & Services Engineer):**
  * Cấu hình chuyển mạch tầng Access (`AccessSW1-Nhom8`, `AccessSW2-Nhom8`), chia cổng và thiết lập Trunking 802.1Q.
  * Rà soát và hoàn thiện các chính sách Extended ACL, bảo mật VTY SSHv2.
  * Thiết lập các dịch vụ mạng nội bộ (Web, DNS, FTP) tại Server Room.
  * Soạn thảo **Chương 3** (Chính sách bảo mật ACL & Triển khai dịch vụ máy chủ) trong Báo cáo Word.
* **Thành viên 3 (Kỹ sư Kiểm thử, Báo cáo & Thuyết trình — QA, Testing & Docs Lead):**
  * Chạy toàn bộ 10 kịch bản kiểm thử trên chế độ Realtime Mode và Simulation Mode (bắt gói tin PDU).
  * Chụp ảnh màn hình minh chứng CLI và gói tin bị drop kèm chú thích rõ ràng.
  * Biên tập và tổng hợp toàn bộ quyển Báo cáo kỹ thuật hoàn chỉnh (Chương 4 kiểm thử, Chương 5 kết luận) và thiết kế Slide bảo vệ đồ án.
