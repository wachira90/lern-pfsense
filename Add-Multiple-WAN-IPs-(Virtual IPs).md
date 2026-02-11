# Add Multiple WAN IPs (Virtual IPs)

In **pfSense**, adding multiple WAN IPs and forwarding them to LAN hosts involves two main parts:

1. Add the additional WAN IPs (Virtual IPs)
2. Create NAT port forward rules to internal LAN IPs

Below is a clear step-by-step guide.

---

# ✅ 1️⃣ Add Multiple WAN IPs (Virtual IPs)

If your ISP gave you multiple public IPs (e.g. a /29 block), you must add them as **Virtual IPs**.

### Go to:

**Firewall → Virtual IPs → Add**

### Configure:

* **Type**:

  * 🔹 Use **IP Alias** (most common for additional usable public IPs)
  * Use **Proxy ARP** only if your ISP requires it
* **Interface**: WAN
* **Address(es)**: Add your extra public IP
  Example:

  ```
  203.0.113.10/32
  203.0.113.11/32
  203.0.113.12/32
  ```
* Description: e.g. `WAN extra IP 1`

Click **Save → Apply Changes**

Repeat for each IP (or add multiple in one entry).

---

# ✅ 2️⃣ Create NAT Port Forward Rules

Now forward each public IP to a LAN server.

### Go to:

**Firewall → NAT → Port Forward → Add**

---

## Example 1: Forward entire public IP to internal server (1:1 NAT style via port forward)

Let’s say:

| Public IP    | Internal IP  |
| ------------ | ------------ |
| 203.0.113.10 | 192.168.1.10 |

### Configure:

* Interface: WAN

* Protocol: TCP (or TCP/UDP)

* Destination:

  * Type: Single host
  * Address: `203.0.113.10`

* Destination Port Range: (e.g. HTTP 80 → 80)

* Redirect target IP: `192.168.1.10`

* Redirect target port: 80

* Check:
  ✅ Add associated firewall rule

Save → Apply

---

# ✅ 3️⃣ If You Want FULL 1:1 NAT (All Ports)

If you want one public IP fully mapped to one LAN server (all ports):

Go to:
**Firewall → NAT → 1:1**

Click Add.

### Configure:

* Interface: WAN
* External subnet IP: `203.0.113.10`
* Internal IP: `192.168.1.10`
* Destination: any

Save → Apply

⚠️ Important:
Make sure you do NOT have overlapping port forwards for that same IP.

---

# ✅ 4️⃣ Outbound NAT (Important!)

If you are using multiple public IPs and want LAN servers to reply using their mapped public IP:

Go to:
**Firewall → NAT → Outbound**

Change to:

* Hybrid mode (recommended)
  or
* Manual mode

Then create outbound NAT rules like:

| Source       | Translation Address |
| ------------ | ------------------- |
| 192.168.1.10 | 203.0.113.10        |
| 192.168.1.11 | 203.0.113.11        |

This ensures proper return traffic symmetry.

---

# ✅ 5️⃣ If Using Multiple WAN Interfaces (Multi-WAN)

If you mean multiple WAN interfaces (WAN1, WAN2):

* Create gateway groups
* Configure policy routing
* Set failover/load balancing rules

That is a different setup from multiple IPs on one WAN.

---

# 🔎 Common Problems

| Problem                                  | Cause                                    |
| ---------------------------------------- | ---------------------------------------- |
| NAT works but replies come from wrong IP | Missing outbound NAT rule                |
| Public IP not reachable                  | ISP routing issue or wrong VIP type      |
| ARP issues                               | Should use IP Alias instead of Proxy ARP |
| Firewall blocks traffic                  | Missing WAN rule                         |

---

# 🎯 Typical Scenario Example

ISP gives:

```
203.0.113.8/29
Gateway: 203.0.113.9
Usable: .10 .11 .12 .13 .14
```

pfSense WAN:

```
203.0.113.10
```

Virtual IPs:

```
.11
.12
.13
.14
```

Then forward each to separate internal servers.


# เพิ่ม WAN IP หลายตัว (Virtual IP)


---

# การเพิ่มหลาย WAN IP และทำ NAT Forward ไปยัง LAN ใน pfSense

การตั้งค่ามี 2 ขั้นตอนหลัก:

1️⃣ เพิ่ม WAN IP เพิ่มเติม (Virtual IP)
2️⃣ สร้าง NAT Forward ไปยังเครื่องใน LAN

---

# ✅ 1️⃣ เพิ่ม WAN IP หลายตัว (Virtual IP)

ถ้า ISP ให้ Public IP หลายตัว (เช่น /29) ต้องเพิ่มเป็น Virtual IP ก่อน

ไปที่:
**Firewall → Virtual IPs → Add**

### ตั้งค่าดังนี้

* **Type**

  * ✅ เลือก **IP Alias** (ใช้บ่อยที่สุด)
  * ใช้ **Proxy ARP** เฉพาะกรณี ISP กำหนด

* **Interface**: เลือก WAN

* **Address(es)**: ใส่ Public IP เพิ่มเติม เช่น

```
203.0.113.10/32
203.0.113.11/32
203.0.113.12/32
```

* Description: ตั้งชื่อเช่น `WAN extra IP`

กด Save → Apply

สามารถเพิ่มหลาย IP ในรายการเดียวได้

---

# ✅ 2️⃣ สร้าง NAT Port Forward

ไปที่:
**Firewall → NAT → Port Forward → Add**

---

## ตัวอย่าง: Forward Public IP ไปยัง Server ภายใน

| Public IP    | LAN IP       |
| ------------ | ------------ |
| 203.0.113.10 | 192.168.1.10 |

### ตั้งค่า

* Interface: WAN

* Protocol: TCP (หรือ TCP/UDP)

* Destination:

  * Type: Single host
  * Address: `203.0.113.10`

* Destination Port: เช่น 80 → 80

* Redirect target IP: `192.168.1.10`

* Redirect target port: 80

ติ๊ก ✅ Add associated firewall rule

Save → Apply

---

# ✅ 3️⃣ ถ้าต้องการทำ 1:1 NAT (ทุกพอร์ต)

ถ้าต้องการให้ Public IP 1 ตัวแมปกับ Server ภายใน 1 เครื่องแบบครบทุกพอร์ต

ไปที่:
**Firewall → NAT → 1:1 → Add**

ตั้งค่า:

* Interface: WAN
* External subnet IP: `203.0.113.10`
* Internal IP: `192.168.1.10`
* Destination: any

Save → Apply

⚠️ ห้ามมี port forward ซ้ำกับ IP เดียวกัน

---

# ✅ 4️⃣ ตั้งค่า Outbound NAT (สำคัญมาก)

ถ้ามีหลาย Public IP และต้องการให้ Server ตอบกลับออกด้วย IP ที่ถูกแมปไว้

ไปที่:
**Firewall → NAT → Outbound**

เปลี่ยนเป็น:

* Hybrid mode (แนะนำ)
  หรือ
* Manual mode

แล้วเพิ่ม rule เช่น:

| Source       | Translation Address |
| ------------ | ------------------- |
| 192.168.1.10 | 203.0.113.10        |
| 192.168.1.11 | 203.0.113.11        |

ถ้าไม่ทำขั้นตอนนี้ บางครั้ง reply จะออกผิด IP

---

# 🔎 ปัญหาที่พบบ่อย

| อาการ                        | สาเหตุ                       |
| ---------------------------- | ---------------------------- |
| NAT เข้าได้ แต่ตอบกลับผิด IP | ไม่ได้ตั้ง Outbound NAT      |
| เข้า Public IP ไม่ได้        | ISP ยังไม่ route มา          |
| ARP ผิดปกติ                  | ใช้ Proxy ARP แทน IP Alias   |
| ทราฟฟิกโดนบล็อก              | ไม่มี firewall rule ฝั่ง WAN |

---

# 🎯 ตัวอย่างสถานการณ์จริง

ISP ให้ block:

```
203.0.113.8/29
Gateway: 203.0.113.9
Usable: .10 .11 .12 .13 .14
```

WAN หลัก:

```
203.0.113.10
```

Virtual IP เพิ่ม:

```
.11
.12
.13
.14
```

แล้วทำ NAT แยกไปแต่ละเครื่องใน LAN




