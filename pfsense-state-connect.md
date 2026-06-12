# pfsense state connect

หากต้องการจำกัดจำนวนการเชื่อมต่อพร้อมกัน (Concurrent States) ต่อผู้ใช้หรืออุปกรณ์ใน pfSense คุณสามารถตั้งค่าผ่านหัวข้อ "Maximum state entries per host" ในกฎของไฟร์วอลล์ (Firewall Rules) ได้โดยตรง ซึ่งจะช่วยป้องกันไม่ให้มีอุปกรณ์ใดอุปกรณ์หนึ่งดึงทรัพยากรระบบไปใช้จนหมด
## ขั้นตอนการตั้งค่าจำกัด Concurrent States

   1. ไปที่เมนู Firewall > Rules บนหน้า pfSense WebGUI
   2. เลือกแท็บ Interface ที่คุณต้องการจำกัดการใช้งาน (เช่น LAN)
   3. คลิกแก้ไข (Edit) กฎ (Rule) ที่ปล่อยให้ทราฟฟิกนั้นผ่าน หรือสร้างกฎขึ้นมาใหม่
   4. เลื่อนลงมาด้านล่างแล้วคลิก Display Advanced ตรงหัวข้อ Advanced Features
   5. มองหาหัวข้อ State Limits แล้วใส่ตัวเลขที่ต้องการในช่อง Maximum state entries per host
   6. คลิก Save แล้วกด Apply Changes

เมื่อ IP ของอุปกรณ์นั้นๆ ใช้งานเกินจำนวนที่เราตั้งไว้ ระบบจะบล็อกหรือดรอป (Drop) การเชื่อมต่อใหม่ทันทีจนกว่าการเชื่อมต่อเก่าจะปิดตัวลง
## ข้อแตกต่างระหว่างการจำกัดแบบรายอุปกรณ์ กับระบบส่วนกลาง

* จำกัดในกฎไฟร์วอลล์ (รายอุปกรณ์): เป็นการคุมแบบ per-IP เช่น หากตั้งไว้ที่ 100 ในกฎ LAN แปลว่าอุปกรณ์แต่ละเครื่องในวง LAN จะสามารถเชื่อมต่อได้สูงสุดคนละ 100 คอนเนคชันพร้อมกัน (เช่น เว็บไซต์, ดาวน์โหลดไฟล์)
* จำกัดในระบบส่วนกลาง (Global Table Limits): หากต้องการปรับขนาดรวมทั้งหมดของตารางบันทึกสถานะ (State Table) ที่ไฟร์วอลล์รองรับได้ ให้ไปที่ System > Advanced > Firewall & NAT แล้วปรับค่าในช่อง Firewall Maximum States (ค่าเริ่มต้นระบบจะคำนวณจาก 10% ของ RAM เครื่อง โดยประมาณ 1,000 states จะใช้ RAM ราวๆ 1MB)

หากต้องการให้แนะนำเพิ่มเติมในการจัดสรรระบบ สามารถบอกข้อมูลเหล่านี้ได้เลยครับ:

* ประเภททราฟฟิกหรืออุปกรณ์ ที่คุณต้องการจำกัด (เช่น วง Guest Wi-Fi หรือ การดาวน์โหลด Torrent)
* ปัญหาที่กำลังเจอ (เช่น ตาราง State Table ของระบบเต็ม หรือต้องการบล็อกพฤติกรรมการใช้งานบางอย่าง)


# =================================


To limit concurrent states per user or device in pfSense, configure the "Maximum state entries per host" threshold under your specific firewall rules. This restricts how many states a single IP can create in the state table, preventing users from overwhelming the network or triggering DDoS protections. [1, 2, 3] 
## Steps to Limit Concurrent States

   1. Navigate to Firewall > Rules in the pfSense WebGUI.
   2. Select the Interface tab where the rule applies (e.g., LAN).
   3. Edit the specific Rule that allows the traffic, or create a new one.
   4. Scroll down and click Display Advanced (or look for Advanced Features) next to Extra Options.
   5. Locate the State Limits section and enter a value in Maximum state entries per host.
   6. Click Save and Apply Changes. [2, 4, 5, 6, 7] 

Once an IP address reaches this limit, subsequent connection attempts matching this rule are dropped. [8] 
## Global System vs. Rule-Based State Limits

* Rule-Based (per-IP): As configured above, this restricts the specific traffic type. For example, if set to 100 on an "any to any" rule, that single IP can create 100 total concurrent states (like web pages, downloads, etc.) across the firewall. [9] 
* Global Table Limits: If you want to increase or adjust the total number of connections the firewall can hold at once in the entire state table, navigate to System > Advanced > Firewall & NAT. Adjust the Firewall Maximum States value (default is 10% of available system RAM; 1k states ≈ 1MB RAM). [10, 11, 12] 

If you'd like, let me know:

* What specific traffic or devices you are trying to limit (e.g., guest WiFi, torrenting)
* Whether you are experiencing dropped traffic or a maxed-out state table [1, 13] 

I can help fine-tune your firewall rules to balance your network resources.

[1] [https://disloops.com](https://disloops.com/pfsense-state-table-maxed-out-by-nmap/)
[2] [https://forum.netgate.com](https://forum.netgate.com/topic/62133/prevent-state-table-dos-how-to-properly-use-maximum-state-entries-per-host)
[3] [https://docs.netgate.com](https://docs.netgate.com/pfsense/en/latest/firewall/configure.html)
[4] [https://docs.netgate.com](https://docs.netgate.com/pfsense/en/latest/firewall/configure.html)
[5] [https://www.youtube.com](https://www.youtube.com/watch?v=gIvc1qZn5dc)
[6] [https://www.zenarmor.com](https://www.zenarmor.com/docs/network-security-tutorials/pfsense-security-monitoring)
[7] [https://www.ivpn.net](https://www.ivpn.net/setup/router/pfsense-wireguard/)
[8] [https://forum.netgate.com](https://forum.netgate.com/topic/62133/prevent-state-table-dos-how-to-properly-use-maximum-state-entries-per-host)
[9] [https://forum.netgate.com](https://forum.netgate.com/topic/62133/prevent-state-table-dos-how-to-properly-use-maximum-state-entries-per-host)
[10] [https://docs.netgate.com](https://docs.netgate.com/pfsense/en/latest/hardware/size.html)
[11] [https://docs.netgate.com](https://docs.netgate.com/pfsense/en/latest/config/advanced-firewall-nat.html)
[12] [https://redmine.pfsense.org](https://redmine.pfsense.org/issues/6499)
[13] [https://forum.netgate.com](https://forum.netgate.com/topic/135631/pf-states-limit-reached)


