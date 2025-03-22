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

### Grafana Installation
Download ได้ที่ https://grafana.com/grafana/download?platform=linux&edition=oss
1. เลือก Edition เป็น OSS
2. เลือกระบบปฏิบัติการที่เหมาะสมกับเครื่อง
3. ติดตั้งตามขั้นตอน

### Loki Installation
Download ได้ที่ https://github.com/grafana/loki/releases/<br>
หรืออ่านวิธีติดตั้งอย่างละเอียดที่ https://grafana.com/docs/loki/latest/setup/install/local/
1. หาหัวข้อ Assets
2. ดาวน์โหลด Loki ที่เหมาะสมกับระบบปฏิบัติการ เช่น loki-windows-amd64.exe.zip
3. แตกไฟล์ไว้ใน Folder
4. ใน Command Line ให้เลือกไปที่ Folder ที่แตกไฟล์ไว้
5. สร้าง configuration files ด้วยคำสั่ง(Powershell) Invoke-WebRequest -Uri "https://raw.githubusercontent.com/grafana/loki/main/cmd/loki/loki-local-config.yaml" -OutFile "loki-local-config.yaml"
6. Start Loki (แนะนำPowershell) ด้วย .\loki-windows-amd64.exe --config.file=loki-local-config.yaml<br>
   หรือ ./loki-linux-amd64 -config.file=loki-local-config.yaml
   * ตัวไฟล์ .exe ต้องอยู่ใน Folder เดียวกันกับ Config File และการรันคำสั่งต้อง cd ไปที่ Folder นั้นก่อน
  
### Promtail Installation
Download ได้ที่ https://github.com/grafana/loki/releases/<br>
หรืออ่านวิธีติดตั้งอย่างละเอียดที่ https://grafana.com/docs/loki/latest/setup/install/local/
1. หาหัวข้อ Assets
2. ดาวน์โหลด Promtail ที่เหมาะสมกับระบบปฏิบัติการ เช่น promtail-windows-amd64.exe.zip
3. แตกไฟล์ไว้ใน Folder (เพื่อความสะดวกใช้ Folder เดียวกันกับ Loki ก็ได้)
4. ใน Command Line ให้เลือกไปที่ Folder ที่แตกไฟล์ไว้
5. สร้าง configuration files ด้วยคำสั่ง(Powershell) Invoke-WebRequest -Uri "https://raw.githubusercontent.com/grafana/loki/main/clients/cmd/promtail/promtail-local-config.yaml" -OutFile "promtail-local-config.yaml"
6. Start Promtail (แนะนำPowershell) ด้วย .\promtail-windows-amd64.exe --config.file=promtail-local-config.yaml<br>
   หรือ ./promtail-linux-amd64 -config.file=promtail-local-config.yaml
    * ตัวไฟล์ .exe ต้องอยู่ใน Folder เดียวกันกับ Config File และการรันคำสั่งต้อง cd ไปที่ Folder นั้นก่อน

## ⚙️ Configuration File
### Loki
```yaml
auth_enabled: false  # ปิดระบบ authentication

server:
  http_listen_port: 3100  # พอร์ตที่ Loki ใช้ให้บริการผ่าน HTTP
  grpc_listen_port: 9096  # พอร์ตที่ Loki ใช้ให้บริการผ่าน gRPC
  log_level: debug  # ระดับการแสดง log (debug, info, warn, error)
  grpc_server_max_concurrent_streams: 1000  # จำนวน gRPC streams สูงสุดที่รับได้

common:
  instance_addr: 127.0.0.1  # ที่อยู่ของอินสแตนซ์ Loki
  path_prefix: /tmp/loki  # โฟลเดอร์เก็บข้อมูล
  storage:
    filesystem:
      chunks_directory: /tmp/loki/chunks  # ที่เก็บ log chunks
      rules_directory: /tmp/loki/rules  # ที่เก็บไฟล์ rule
  replication_factor: 1  # จำนวนสำเนาของข้อมูลที่ทำซ้ำ
  ring:
    kvstore:
      store: inmemory  # ใช้ memory store แทน disk

query_range:
  results_cache:
    cache:
      embedded_cache:
        enabled: true  # เปิดใช้งาน cache
        max_size_mb: 100  # ขนาด cache สูงสุด 100MB

limits_config:
  metric_aggregation_enabled: true  # เปิดใช้งานการรวม metric

schema_config:
  configs:
    - from: 2020-10-24
      store: tsdb  # ใช้ TSDB ในการจัดเก็บ log
      object_store: filesystem  # เก็บข้อมูลลงไฟล์ระบบ
      schema: v13  # ใช้ schema version 13
      index:
        prefix: index_  # ชื่อ index prefix
        period: 24h  # index ใหม่จะถูกสร้างทุก 24 ชั่วโมง

ruler:
  alertmanager_url: http://localhost:9093  # URL ของ Alertmanager ที่ใช้แจ้งเตือน

frontend:
  encoding: protobuf  # ใช้ Protobuf ในการ encode ข้อมูล

querier:
  engine:
    enable_multi_variant_queries: true  # เปิดใช้งาน multi-variant queries





