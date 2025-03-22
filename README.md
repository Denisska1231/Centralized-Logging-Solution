<p align="center">
  <img src="https://github.com/user-attachments/assets/4376f6fe-0324-4a0b-89ed-bb5407a80c6f" alt="image" width="25%">
</p>

# Centralized-Logging-Solution

โปรเจกต์นี้เป็นระบบ **Centralized Logging** โดยใช้ **Grafana, Loki และ Promtail** สำหรับเก็บและวิเคราะห์ Log จากระบบต่างๆ

## 🛠️ เครื่องมือที่ใช้
- **Grafana** - แสดงผลข้อมูล Log ในรูปแบบ Dashboard
- **Loki** - จัดเก็บ Log แบบดัชนีต่ำ (Index-Free)
- **Promtail** - ตัวรวบรวม Log และส่งไปยัง Loki

## 🚀 วิธีติดตั้งและตั้งค่า
ต้องติดตั้ง **Grafana** **Loki** **Promtail** ให้ครบทั้ง 3 ตัว จึงจะใช้งานได้
แนะนำให้ติดตั้งทั้ง 3 ตัว ภายในเครื่องเดียวกัน เพื่อความง่ายและสะดวกในการใช้งาน

Grafana Installation
Download ได้ที่ https://grafana.com/grafana/download?platform=linux&edition=oss
1. เลือก Edition เป็น OSS
2. เลือกระบบปฏิบัติการที่เหมาะสมกับเครื่อง
3. ติดตั้งตามขั้นตอน

Loki Installation
Download ได้ที่ https://github.com/grafana/loki/releases/ หรืออ่านวิธีติดตั้งอย่างละเอียดที่ https://grafana.com/docs/loki/latest/setup/install/local/
1. หาหัวข้อ Assets
2. ดาวน์โหลด Loki ที่เหมาะสมกับระบบปฏิบัติการ เช่น loki-windows-amd64.exe.zip
3. แตกไฟล์ไว้ใน Folder
4. ใน Command Line ให้เลือกไปที่ Folder ที่แตกไฟล์ไว้
5. จากนั้นใช้คำสั่ง wget https://raw.githubusercontent.com/grafana/loki/main/cmd/loki/loki-local-config.yaml เพื่อสร้าง configuration files (หากรันโค้ดไม่ได้ ให้ปรับเปลี่ยนคำสั่ง wget เพื่อให้เข้ากับโปรแกรมที่รันโค้ดแต่ยังใช้ URL เดิม)
6. Start Loki ด้วย Command .\loki-windows-amd64.exe --config.file=loki-local-config.yaml หรือ ./loki-linux-amd64 -config.file=loki-local-config.yaml
   * ตัวไฟล์ .exe ต้องอยู่ใน Folder เดียวกันกับ Config File และการรันคำสั่งต้อง cd ไปที่ Folder นั้นก่อน
  
Promtail Installation
Download ได้ที่ https://github.com/grafana/loki/releases/ หรืออ่านวิธีติดตั้งอย่างละเอียดที่ https://grafana.com/docs/loki/latest/setup/install/local/
1. หาหัวข้อ Assets
2. ดาวน์โหลด Promtail ที่เหมาะสมกับระบบปฏิบัติการ เช่น promtail-windows-amd64.exe.zip
3. แตกไฟล์ไว้ใน Folder (เพื่อความสะดวกใช้ Folder เดียวกันกับ Loki ก็ได้)
4. ใน Command Line ให้เลือกไปที่ Folder ที่แตกไฟล์ไว้
5. จากนั้นใช้คำสั่ง wget https://raw.githubusercontent.com/grafana/loki/main/clients/cmd/promtail/promtail-local-config.yaml เพื่อสร้าง configuration files (หากรันโค้ดไม่ได้ ให้ปรับเปลี่ยนคำสั่ง wget เพื่อให้เข้ากับโปรแกรมที่รันโค้ดแต่ยังใช้ URL เดิม)
6. Start Promtail ด้วย Command .\promtail-windows-amd64.exe --config.file=promtail-local-config.yaml หรือ .\promtail-linux-amd64.exe --config.file=promtail-local-config.yaml
    * ตัวไฟล์ .exe ต้องอยู่ใน Folder เดียวกันกับ Config File และการรันคำสั่งต้อง cd ไปที่ Folder นั้นก่อน





