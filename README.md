# Enterprise Network Design (OSPF + VLAN)

## 📌 Objective:
This project designs and simulates an enterprise-level network using Huawei eNSP to test the functionality of OSPF Routing and VLAN Segmentation.

## 🗺️ Network Architecture

```mermaid
graph TD
    subgraph Internet_Zone
        ISP[ISP / Internet]
    end

    subgraph Security_Zone
        FW[Firewall USG6000V]
    end

    subgraph Core_Zone
        R1[Router AR2220<br>10.0.0.1/30]
        CS[Core Switch S5700<br>10.0.0.2/30]
    end

    subgraph Access_Zone
        ASW1[Access Switch 1<br>VLAN 10 - IT]
        ASW2[Access Switch 2<br>VLAN 20 - HR]
        ASW3[Access Switch 3<br>VLAN 30 - Guest]
    end

    subgraph End_Devices
        PC1[PC - IT<br>192.168.10.10]
        PC2[PC - HR<br>192.168.20.10]
        PC3[PC - Guest<br>192.168.30.10]
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
```

## 🛠️ Equipment used
- Router: AR2220
- Firewall: USG6000V
- Core Switch: S5700
- Access Switch: S3700
- PC: 3 units

## 📋 IP Addressing & VLAN

| equipment | Interface | IP Address | VLAN |
|---|---|---|---|
| Router | GE0/0/0 | 10.0.0.1/30 | - |
| Core Switch | GE0/0/1 | 10.0.0.2/30 | - |
| Core Switch | Vlanif10 | 192.168.10.1/24 | 10 (IT) |
| Core Switch | Vlanif20 | 192.168.20.1/24 | 20 (HR) |
| Core Switch | Vlanif30 | 192.168.30.1/24 | 30 (Guest) |

## ⚙️ Key Configurations

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


## ✅ Verification
- [x] PCs in VLAN 10 can ping VLAN 20.
- [x] The router can learn OSPF routing (display ip routing-table).
- [x] The firewall blocks traffic from the Guest VLAN to the IT VLAN.

## 🧠 What I learned
1. OSPF operation in Area 0
2. VLAN separation for security
3. Basic network troubleshooting

## 🔗 References
- HCIA-Datacom Official Guide
- Huawei eNSP Simulator

  
