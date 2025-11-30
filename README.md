
# **Design & Implementation of a Remote Mobile Cybersecurity Reconnaissance & Wireless Network Penetration Testing Platform**

*A Technical Documentation for GitHub Repository*

---

## **Abstract**

This project presents the design and implementation of a remotely controlled mobile cybersecurity reconnaissance and wireless penetration testing platform, built using a Raspberry Pi 5 mounted on an RC car chassis. The system enables field-deployable wireless network assessments through secure remote access, real-time low-latency video streaming, Wi-Fi/BLE/IoT reconnaissance, and onboard penetration testing tool execution. Secure control and streaming are achieved using encrypted VPN tunneling over LTE, enabling operation at long distances without local network dependency. The project demonstrates a convergence of cybersecurity engineering, embedded systems, wireless networking, and robotics.

---

## **1. Introduction**

Wireless penetration testing typically requires physical proximity to the target environment. However, scenarios involving restricted areas or physical risk call for an alternative approach. This project aims to create a **mobile cybersecurity field rover** capable of remote wireless reconnaissance, designed for real-world usage where mobility is essential.

The system converts a standard RC vehicle into a complete cybersecurity deployment tool, supporting:

* Wireless network reconnaissance and attack modules
* BLE and IoT device fingerprinting
* Secure remote command execution
* Real-time live video for navigation

---

## **2. Problem Statement**

To develop a remotely operated mobile platform capable of:

* Performing wireless reconnaissance and penetration testing
* Navigating through environments visually using live video feed
* Operating fully remotely without local infrastructure
* Maintaining secure control and data confidentiality
* Supporting modular expansion for cybersecurity tooling

---

## **3. Project Objectives**

* Build a mobile platform enabling real-time cybersecurity assessments
* Implement secure remote multi-session access over encrypted channels
* Support Wi-Fi, BLE and IoT reconnaissance tools directly on-board
* Provide low-latency live video streaming for remote control
* Create a deployable and resilient system suitable for field testing

---

## **4. System Overview**

```
┌───────────────────────────────────────────────────────────┐
│ Operator Workstation (Laptop / PC)                        │
│ SSH Control + Video Stream via Tailscale Encrypted Tunnel │
└───────────────▲───────────────────────────────────────────┘
                │
                │ Secure Mesh VPN (LTE / Wi-Fi)
                │
┌───────────────┴───────────────────────────────────────────┐
│ Raspberry Pi 5 (On-Board Compute Platform)                │
│ ├─ Pi Camera (Low Latency Streaming Pipeline)             │
│ ├─ Wi-Fi / BLE Recon & Pentest Modules                    │
│ ├─ IoT Fingerprinting via nmap / masscan / searchsploit   │
│ └─ RC Motor Drive & Mobility                              │
└───────────────────────────────────────────────────────────┘
```

---

## **5. Hardware Components**

| Component                       | Specification                               |
| ------------------------------- | ------------------------------------------- |
| Raspberry Pi 5                  | CPU for all compute tasks                   |
| Pi Camera Module 3 Wide         | 720p/1080p low-latency wide-angle streaming |
| Pre-built RC Car                | Base chassis with motor assembly            |
| 12V 5200mAh Li-ion 3S Battery   | Main power source                           |
| 12V → 5V 5A Buck Converter      | Stable power for Pi                         |
| Amazon Basics 4G LTE USB Dongle | Remote cellular uplink                      |
| Internal Wi-Fi & BLE            | Wireless attack & scanning                  |
| USB-C Power Delivery Cable      | Safe high-load power                        |
| Structural wiring & 3D Mounts   | Assembly support                            |

---

## **6. Software Stack**

| Layer             | Tools / Frameworks                            |
| ----------------- | --------------------------------------------- |
| OS                | Raspberry Pi OS Bookworm                      |
| Remote Networking | Tailscale Mesh VPN                            |
| Control Interface | SSH + tmux                                    |
| Video Streaming   | rpicam-vid → H264 → MPEG-TS → mpv             |
| Wireless Tools    | aircrack-ng, airodump-ng, aireplay-ng, wifite |
| BLE Tools         | BlueZ, Bettercap                              |
| IoT Recon         | nmap, masscan, searchsploit                   |
| Utility           | Python & Bash scripting                       |

---

## **7. Networking Architecture**

```
Rover Boot → Tailscale Auto Join → Encrypted Tunnel →
Operator SSH → Start Video Stream + Control Session
```

### Dual SSH Channels

| Session   | Purpose                                   |
| --------- | ----------------------------------------- |
| Session 1 | Live video control pipeline               |
| Session 2 | Cybersecurity tool execution & navigation |

---

## **8. Video Streaming Pipeline**

```
PiCam → rpicam-vid → H.264 Compression → SSH Pipe →
MPV low-latency player
```

### Command:

```bash
ssh -T pi@<TS-IP> \
"rpicam-vid -t 0 --nopreview --width 1280 --height 720 --framerate 30 \
--bitrate 2000000 --codec h264 --libav-format mpegts -o -" \
| mpv --no-cache --profile=low-latency --untimed --really-quiet -
```

Performance:

* 30fps live control
* ~18-50ms LAN latency
* < 120ms LTE latency

---

## **9. Wireless Penetration Testing Modules**

### Wi-Fi Recon & Attacks

```
airmon-ng start wlan0
airodump-ng wlan0mon
aireplay-ng --deauth <target>
```

Capabilities:

* Device & AP scanning with signal prioritization
* Handshake capture
* Deauthentication-based attack vectors

### BLE Recon

* BlueZ stack
* Bettercap BLE modules
* MAC fingerprinting

### IoT Recon

* nmap / masscan port sweeping
* Firmware fingerprinting
* Automated exploit lookup via searchsploit

---

## **10. Operational Workflow**

```
1. Power on rover
2. LTE network establishes Tailscale link
3. Operator connects via SSH
4. Start live video feed
5. Run wireless recon & attacks
6. Navigate rover visually
7. Collect logs & shut down safely
```

---

## **11. Security Considerations**

| Method                          | Purpose                     |
| ------------------------------- | --------------------------- |
| Tailscale End-to-End Encryption | Prevent unauthorized access |
| SSH Keys Only                   | Passwordless secure auth    |
| Access Control Logs             | Usage audit                 |
| Physical kill-switch logic      | Emergency shutdown          |
| Isolated attack environment     | Ethical testing compliance  |

---

## **12. Testing & Evaluation**

| Scenario                   | Result                      |
| -------------------------- | --------------------------- |
| Wi-Fi Recon & Handshakes   | Successful                  |
| BLE Scan & Enumeration     | Stable                      |
| Video Streaming via LTE    | < 120ms latency             |
| Field Movement Performance | Stable control              |
| Exam Demo Failure          | Tailscale IP banned live    |
| Response                   | Pipeline rebuilt & restored |

---

## **13. Challenges & Solutions**

| Challenge              | Solution                           |
| ---------------------- | ---------------------------------- |
| Power instability      | Dedicated buck regulator           |
| Camera drivers failing | OS rebuild & config patching       |
| LTE drops              | reconnect logic / fallback connect |
| Live demo failure      | Manual rapid pipeline recovery     |

---

## **14. Team Members**

Tirthak Likhar
Ayush Shrikhande
Ambar Kumar

---

## **15. Acknowledgements**

* Dr. Sudhanshu Sir
* Dr. Soubhagya Mallik
* Dr. Nikhil Mangrudkar

---

## **16. Ethical & Legal Notice**

This platform is intended strictly for educational and authorized cybersecurity research.
Unauthorized wireless exploitation is illegal and punishable by law.

---

## **17. License**

MIT License (for academic reference only)

---

