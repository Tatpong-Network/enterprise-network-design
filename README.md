# Enterprise Network Design (OSPF + VLAN)

## 📌 วัตถุประสงค์
โปรเจกต์นี้เป็นการออกแบบและจำลองเครือข่ายระดับองค์กรโดยใช้ Huawei eNSP เพื่อทดสอบการทำงานของ OSPF Routing และ VLAN Segmentation

## 🗺️ Network Architecture
![Network Diagram](network-architecture.png)

```mermaid
graph TD 

    subgraph Internet_Zone["🌐 Internet Zone"]
        ISP[ISP / Internet]
    end
    
    subgraph Security_Zone["🔥 Security Zone"]
        FW[Firewall<br/>USG6000V]
    end

    subgraph Core_Zone["⚙️ Core Zone"]
        R1[Router<br/>AR2220<br/>10.0.0.1/30]
        CS[Core Switch<br/>S5700<br/>10.0.0.2/30]
    end

    subgraph Access_Zone["🔀 Access Zone"]
        ASW1[Access Switch 1<br/>VLAN 10 - IT]
        ASW2[Access Switch 2<br/>VLAN 20 - HR]
        ASW3[Access Switch 3<br/>VLAN 30 - Guest]
    end

    subgraph End_Devices["💻 End Devices"]
        PC1[PC - IT<br/>192.168.10.10]
        PC2[PC - HR<br/>192.168.20.10]
        PC3[PC - Guest<br/>192.168.30.10]
    end

    ISP --> FW
    FW --> R1
    R1 -->|OSPF Area 0| CS
    CS --> ASW1
    CS --> ASW2
    CS --> ASW3
    ASW1 --> PC1
    ASW2 --> PC2
    ASW3 --> PC3

    style Internet_Zone fill:#e1f5ff
    style Security_Zone fill:#ffe1e1
    style Core_Zone fill:#fff4e1
    style Access_Zone fill:#e1ffe1
    style End_Devices fill:#f0e1ff

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

  
