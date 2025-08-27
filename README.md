# 🛡️ การตั้งค่าความปลอดภัยบน Linux (LAB Assignment)

## 📌 ภาพรวม
เอกสารนี้เป็นการบ้านเชิงปฏิบัติการ (Lab Assignment) เกี่ยวกับการรักษาความปลอดภัยบน Linux  
ครอบคลุมการจัดการผู้ใช้และกลุ่มสิทธิ์ การกำหนดสิทธิ์ sudo การรักษาความปลอดภัย SSH การตั้งค่า Firewall และการตรวจสอบระบบ

---

## 🔑 งานที่ 1: การจัดการ User & Group
- สร้างกลุ่ม: `developers`, `testers`, `dbadmin`  
- สร้างผู้ใช้: `chonl`, `nunt`, `tuser`, `dbuser`  
- ตั้งค่า **นโยบายรหัสผ่าน**:
  - อายุรหัสผ่านสูงสุด: 90 วัน  
  - ขั้นต่ำก่อนเปลี่ยนใหม่: 7 วัน  
  - แจ้งเตือนก่อนหมดอายุ: 14 วัน  
  - ความยาวขั้นต่ำ: 12 ตัวอักษร (ต้องมีตัวใหญ่ เล็ก ตัวเลข และสัญลักษณ์)  
- ติดตั้ง **libpam-pwquality** เพื่อบังคับใช้ความซับซ้อนของรหัสผ่าน  

---

## ⚡ งานที่ 2: การกำหนดสิทธิ์ Sudo
- สร้างกลุ่ม: `sudo-developers`, `sudo-limited`  
- **Developers** (`chonl`, `nunt`) → ใช้ sudo ได้เต็มสิทธิ์  
- **Limited** (`tuser`) → ใช้ได้เฉพาะ:
  - `systemctl status`  
  - `tail` log files  
  - `ps`  
- **Database admin** (`david`) → ใช้ได้เฉพาะคำสั่งที่เกี่ยวกับ MySQL  
- การตั้งค่าเพิ่มเติม:
  - เวลา session sudo หมดอายุภายใน **15 นาที**  
  - เก็บ log คำสั่ง sudo ที่ `/var/log/sudo.log`  

---

## 🔒 งานที่ 3: ความปลอดภัย SSH
- ปรับแต่ง `sshd_config`:
  - เปลี่ยน port → **2222**  
  - ปิด root login  
  - อนุญาตเฉพาะ users ที่ระบุ  
  - เปิดการยืนยันตัวตนด้วย key-based authentication  
  - จำกัดจำนวนครั้งในการพยายาม login และ timeout ของ session  
- เพิ่ม **SSH banner** เพื่อแจ้งเตือนก่อนเข้าใช้งาน  
- Restart และทดสอบการเชื่อมต่อ SSH  

---

## 🌐 งานที่ 4: การตั้งค่า Firewall (UFW)
- ค่าเริ่มต้น: **deny incoming**, **allow outgoing**  
- อนุญาตบริการ:
  - SSH → `2222/tcp`  
  - Web → `80/tcp`, `443/tcp`  
- จำกัดการเข้าถึง MySQL เฉพาะเครือข่ายภายใน  
- เปิดใช้งาน **rate limiting** สำหรับ SSH  
- เปิด logging และตรวจสอบ rules  

---

## 📊 งานที่ 5: การตรวจสอบระบบ
- ติดตั้งเครื่องมือ: **Fail2Ban, Logwatch, Sysstat, Htop, Iotop, ELK stack (option)**  
- ตั้งค่า **Fail2Ban**:
  - ป้องกัน SSH และ Apache จากการ brute-force  
- สร้างสคริปต์ **system_monitor.sh**:
  - เก็บข้อมูล CPU, Memory, Disk, Active users, Failed logins  
- ตั้ง cron job ให้รันทุกชั่วโมง  
- ตั้งค่า **logrotate**:
  - เก็บ log ไว้ 30 วัน และบีบอัดอัตโนมัติ  

---

## 📸 สิ่งที่ต้อง Capture
- `sudo fail2ban-client status`  
- `sudo fail2ban-client status sshd`  
- ไฟล์: `/var/log/system_monitor.log`  
- `sudo systemctl status fail2ban`  

---

## ✅ สรุป
Lab นี้แสดงให้เห็นการทำ **Linux Security Hardening** ครบขั้นตอน ได้แก่:  
- การจัดการผู้ใช้และนโยบายรหัสผ่าน  
- การกำหนดสิทธิ์และการจัดการ sudo  
- การรักษาความปลอดภัยของ SSH  
- การตั้งค่า Firewall  
- การตรวจสอบและจัดการ log ระบบ  
Link Linux Security Server Configuration Download

---
