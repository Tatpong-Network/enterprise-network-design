# Enterprise Network Design (OSPF + VLAN)

## 📌 วัตถุประสงค์
โปรเจกต์นี้เป็นการออกแบบและจำลองเครือข่ายระดับองค์กรโดยใช้ Huawei eNSP เพื่อทดสอบการทำงานของ OSPF Routing และ VLAN Segmentation

## 🗺️ Network Architecture
![Network Diagram](network-architecture.png)

## 🛠️ อุปกรณ์ที่ใช้
- Router: AR2220
- Firewall: USG6000V
- Core Switch: S5700
- Access Switch: S3700
- PC: 3 เครื่อง

## 📋 IP Addressing & VLAN

| อุปกรณ์ | Interface | IP Address | VLAN |
|---|---|---|---|
| Router | GE0/0/0 | 10.0.0.1/30 | - |
| Core Switch | GE0/0/1 | 10.0.0.2/30 | - |
| Core Switch | Vlanif10 | 192.168.10.1/24 | 10 (IT) |
| Core Switch | Vlanif20 | 192.168.20.1/24 | 20 (HR) |
| Core Switch | Vlanif30 | 192.168.30.1/24 | 30 (Guest) |

## ⚙️ การตั้งค่าหลัก (Key Configurations)

### 1. VLAN Configuration (Core Switch)

vlan batch 10 20 30
interface GigabitEthernet0/0/1
port link-type trunk
port trunk allow-pass vlan 10 20 30


### 2. OSPF Configuration (Router & Core Switch)

ospf 1 router-id 1.1.1.1
area 0.0.0.0
network 10.0.0.0 0.0.0.3
network 192.168.10.0 0.0.0.255
network 192.168.20.0 0.0.0.255
network 192.168.30.0 0.0.0.255


## ✅ การทดสอบ (Verification)
- [x] PC ใน VLAN 10 สามารถ Ping ไปยัง VLAN 20 ได้
- [x] Router เรียนรู้เส้นทาง OSPF ได้ (display ip routing-table)
- [x] Firewall บล็อคทราฟฟิกจาก Guest VLAN ไปยัง IT VLAN

## 🧠 สิ่งที่ได้เรียนรู้
1. การทำงานของ OSPF ใน Area 0
2. การแยก VLAN เพื่อความปลอดภัย
3. การ Troubleshoot ปัญหา Network เบื้องต้น

## 🔗 References
- HCIA-Datacom Official Guide
- Huawei eNSP Simulator

  
