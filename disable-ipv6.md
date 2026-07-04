# วิธีปิดการใช้งาน IPv6 ใน pfSense 

วิธีปิดการใช้งาน IPv6 ใน pfSense อย่างสมบูรณ์ ต้องตั้งค่าทั้งระบบและพอร์ตเชื่อมต่อ (Interfaces) ต่าง ๆ ดังนี้
1. ตั้งค่าปิดใช้งานทั้งระบบ (System-wide):

* ไปที่เมนู System > Advanced > Networking
* ติ๊กเครื่องหมายถูกออกที่ช่อง Allow IPv6 (ในบางเวอร์ชันอาจเป็นการติ๊กเลือกเพื่อปิด)
* กด Save ด้านล่างสุด

2. ปิดใช้งาน IPv6 บน Interfaces:

* ไปที่เมนู Interfaces > WAN
* เปลี่ยน IPv6 Configuration Type ให้เป็น None
* กด Save และ Apply Changes
* ทำตามขั้นตอนนี้ซ้ำกับเน็ตเวิร์กภายในอื่น ๆ ทั้งหมด (เช่น LAN, OPT)

3. ปิดบริการ IPv6 อื่น ๆ (Services):

* ไปที่ System > Routing > Gateways แล้วลบหรือปิดใช้งาน (Disable) เกตเวย์ที่เป็น IPv6
* ไปที่ Services > DHCPv6 Relay และตรวจสอบว่าปิดใช้งานอยู่
* ไปที่ Services > Router Advertisements และเปลี่ยน Router Mode เป็น Disabled

4. บล็อกทราฟฟิก IPv6 ที่เหลือ (ตัวเลือกเสริม):

* ไปที่ Firewall > Rules เลือกแท็บ Floating
* เพิ่มกฎใหม่: Action = Block, Interface = [เลือกทุก Interface], Address Family = IPv6, Protocol = Any เพื่อป้องกันทราฟฟิกหลุดรอด

ไม่ทราบว่าคุณกำลังปิด IPv6 เพื่อ แก้ปัญหาอินเทอร์เน็ตหลุด/ช้า หรือเพื่อ ลดความรกของ Firewall Log ? แจ้งเพิ่มเติมได้เลยเพื่อที่ผมจะได้แนะนำการปรับแต่งเน็ตเวิร์กส่วนอื่นที่เกี่ยวข้องให้

