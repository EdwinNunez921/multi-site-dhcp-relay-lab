## 🖥️ DHCP Configuration

In this portion of the lab, a DHCP server was implemented to dynamically assign IP addresses to all end hosts across all VLANs.

---

### 📋 DHCP Pools

Five pools were created on the DHCP server — one for each VLAN. Each pool was configured starting from the **second available IP address**, since the first usable IP in each subnet was reserved for the router subinterfaces.

| Pool | VLAN | Purpose |
|---|---|---|
| Pool 1 | VLAN 10 | Management |
| Pool 2 | VLAN 20 | IT |
| Pool 3 | VLAN 30 | Staff |
| Pool 4 | VLAN 40 | Staff |
| Pool 5 | VLAN 50 | Guest |

---

### 🏠 Server Placement & Switch Configuration
- The DHCP server was placed in **VLAN 10 (Management)**
- The FastEthernet port on **SW1** connecting to the server was configured as an **access port assigned to VLAN 10**
- This allowed the server to communicate on the network and serve IP addresses to hosts

---

### 💻 End Host Configuration
- Static IP configurations were removed from all end hosts and set to **DHCP**
- After running `ipconfig /renew`, **PC1** successfully received an IP address and default gateway from the DHCP server
- This confirmed the server was reachable within VLAN 10

---

### 🔀 DHCP Relay Agent
Since DHCP broadcasts do not cross router interfaces, the `ip helper-address 192.168.10.254` command was configured on the following subinterfaces:

| Router | Subinterface | VLAN |
|---|---|---|
| R1 | VLAN 20 subinterface | IT |
| R1 | VLAN 30 subinterface | Staff |
| R2 | VLAN 40 subinterface | Staff |
| R2 | VLAN 50 subinterface | Guest |

---

### ✅ Verification
- All end hosts ran `ipconfig /release` followed by `ipconfig /renew` and successfully received IP addresses from their respective DHCP pools
- **PC1** was used to ping **PC2, PC3, PC4, and PC5** across all VLANs
- PC1 also successfully pinged the **DHCP server** in VLAN 10
- Full end-to-end connectivity confirmed across all VLANs ✔️
