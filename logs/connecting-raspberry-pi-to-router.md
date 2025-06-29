# Connecting Raspberry Pi to Router

**Description:**  
Steps for connecting a Raspberry Pi to a home router and assigning a static IP via DHCP reservation. Covers verifying connectivity and resolving common issues.

---

## Setup

- **Device:** Raspberry Pi 5 (4GB RAM)
- **Connection:** Ethernet cable
- **Client Device:** Mac
- **Network:** Wired LAN (Ethernet)
- **Router Access:** Home router via web interface

---

## 1. Physical Connection

- Connect Raspberry Pi to router via Ethernet cable **or** connect wirelessly via Wi-Fi.
- Ensure router is powered on and functioning.

---

## 2. Verify Network Interface on Pi

- Check IP address assigned to Pi:

```bash
ip addr show
```

- Look for Ethernet (eth0) or Wi-Fi (wlan0) interface and note the assigned IP.

---

## 3. Access Router Admin Panel

- Open a browser on a device connected to the same network.
- Navigate to the router's IP address.
- Log in with admin credentials.

---

## 4. DHCP Reservation

- Locate DHCP settings or LAN setup section.
- Reserve an IP address for your Pi by binding its MAC address to a specific IP
- Save and apply settings.

---

## 5. Verify Connectivity

- From the Pi, ping the router:

```bash
ping -c 4 <router-ip>
```

- Ping an external IP to check internet connectivity (if applicable):

```bash
ping -c 4 8.8.8.8
```

- Check DNS resolution:

```bash
ping -c 4 google.com
```

---

## Key Learnings

- Learned how to assign a reserved static IP to the Raspberry Pi using the router’s DHCP settings.
- Understood the difference between DHCP (dynamic IP) and static IP assignment.
- Gained familiarity with router settings, including MAC address bindings and DHCP leases.
- Discovered how DNS resolution works by comparing ping results (IP vs domain).
- Realized that DHCP reservation simplifies consistent SSH access across reboots.
