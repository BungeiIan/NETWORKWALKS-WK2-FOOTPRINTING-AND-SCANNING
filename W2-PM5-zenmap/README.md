# NETWORKWALKS-WK2-PM5-NETWORK-SCANNING-ZENMAP

## Network Scanning with Zenmap: README

This project records a host-discovery exercise with **Zenmap**, the graphical front end of Nmap. The goal was to find my own IP address and subnet, list the live hosts on that subnet, note their IP and MAC addresses, and view the network topology. The scan was run on 20 September 2026.

## What You Need

- Windows PC
- Nmap/Zenmap from <https://nmap.org/download.html> (the installer includes Npcap)
- A network you own or have permission to scan
- Screenshots of `ipconfig`, the scan results and the topology

## Steps Carried Out

### 1. Install Zenmap
Zenmap (Nmap 7.991) was installed from the official Nmap download page. The first installer download was corrupted and failed its integrity check, so it was downloaded again.

### 2. Find the local IP address and subnet
`ipconfig` and `ipconfig /all`

| Adapter | IPv4 address | Subnet mask | Notes |
|---|---|---|---|
| Wi-Fi | 10.111.9.12 | 255.255.0.0 | Gateway 10.111.0.1. A shared /16 network of 65,536 addresses, so it was **not** scanned |
| Mobile Hotspot (Microsoft Wi-Fi Direct Virtual Adapter #2) | 192.168.5.1 | 255.255.255.0 | MAC `36-F3-9A-5A-2D-2D`. This is the network that was scanned |

The Wi-Fi network is not mine and I have no permission to test it, so I turned on the Windows Mobile Hotspot and scanned that network instead. The hotspot subnet is **192.168.5.0/24**.

### 3. Find the live hosts
Target: `192.168.5.0/24`, profile: **Quick scan**
`nmap -T4 -F 192.168.5.0/24`

The scan covered 256 addresses and finished in 13.71 seconds. Unlike a plain ping scan (`nmap -sn`), the Quick scan profile also checks the 100 most common ports.

### 4. How many hosts are live?
**2 hosts** are live, including my own PC.

### 5. IP addresses of the live hosts
- `192.168.5.1` (my PC)
- `192.168.5.182` (a device connected to the hotspot)

### 6. MAC addresses of the live hosts

| Host | MAC address | Notes |
|---|---|---|
| 192.168.5.1 | 36-F3-9A-5A-2D-2D | My own PC; Nmap does not show its own MAC, so it was read from `ipconfig /all` |
| 192.168.5.182 | 6A:A8:51:47:D8:2D | Vendor shown as Unknown |

The MAC of `192.168.5.182` is locally administered (randomised), which is why Nmap cannot name a manufacturer. Phones often use randomised addresses for privacy.

### 7. View the topology
In Zenmap, open the **Topology** tab, turn on **Legend**, then click **Save Graphic** to export it as a PDF. The map shows `localhost` connected to `192.168.5.1` (yellow: three to six open ports) and to `192.168.5.182` (green: fewer than three open ports).

## Quick Reference

| Item | Result |
|---|---|
| Network scanned | 192.168.5.0/24 (own Windows Mobile Hotspot) |
| Scan profile | Quick scan (`nmap -T4 -F`) |
| Addresses scanned | 256 |
| Live hosts | 2 |
| Scan time | 13.71 seconds |
| Open ports on my PC (192.168.5.1) | 80 (http), 135 (msrpc), 139 (netbios-ssn), 445 (microsoft-ds), 3306 (mysql), 5357 (wsdapi) |
| Ports on 192.168.5.182 | All 100 scanned ports closed |

## Notes

- Only scan networks you own or have written permission to test. A shared Wi-Fi network is not yours to scan.
- The quick scan showed that my own PC exposes file sharing, RPC and a MySQL database on the hotspot-facing address. Anyone who joins the hotspot could reach them. Bind MySQL to localhost, turn off file sharing on hotspot and public networks, and restrict inbound connections with the Windows firewall.
- These results are observations, not proven vulnerabilities.

## Evidence

```
W2-PM5-zenmap/
├── README.md
├── screenshots/    # ipconfig, Zenmap scan results, topology
└── topology.pdf    # exported from Zenmap
```

**Author:** Ian Bungei Imbayi

Based on the "Network Scanning with Zenmap" task from Networkwalks Academy: [www.networkwalks.com](https://www.networkwalks.com)
