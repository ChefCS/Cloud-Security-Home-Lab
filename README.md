# Self-Hosted Cloud Infrastructure with Cybersecurity

## Overview
This project creates a self-hosted cloud environment to explore virtualization, containerization, and cybersecurity. It uses UTM on macOS to run Proxmox as a hypervisor, hosting VMs and Docker containers for web and file services, secured with a pfSense firewall, Wazuh SIEM, and penetration testing.

### Features
- **Virtualization:** Proxmox manages VMs for Nginx (web) and Nextcloud (file storage).
- **Containerization:** Docker runs lightweight services.
- **Network Security:** pfSense firewall with WireGuard VPN and VLANs.
- **Monitoring:** Wazuh SIEM for log analysis and intrusion detection.
- **Penetration Testing:** Kali Linux VM for vulnerability testing.
- **Encryption:** TLS via Let’s Encrypt for secure communication.

### Prerequisites
- **Hardware:** Mac with 8GB+ RAM and 100GB+ storage (Intel or Apple Silicon).
- **Software:** UTM (download from [getutm.app](https://getutm.app)), Proxmox VE ISO, Docker, pfSense ISO, Wazuh, Kali Linux ISO, Ansible (optional).

### Setup Instructions
1. **Install UTM on macOS:**
   - Download UTM from the official site or Mac App Store.
   - Open UTM and ensure it runs (no SIP/kernel extension issues on Apple Silicon).
2. **Set Up Proxmox VM:**
   - In UTM, create a new VM (Architecture: x86_64, System: Generic).
   - Attach the Proxmox VE ISO (downloaded from [proxmox.com](https://www.proxmox.com)).
   - Allocate 4GB RAM, 2 CPUs, 50GB disk; enable VirtIO drivers for network/disk.
   - Boot and install Proxmox via the GUI installer.
3. **Deploy Nested VMs in Proxmox:**
   - Via Proxmox web UI (`http://<mac-ip>:8006`):
     - Ubuntu VM (2GB RAM, 20GB disk) for Nginx/Nextcloud.
     - Kali Linux VM (4GB RAM, 20GB disk) for testing.
     - pfSense VM (1GB RAM, 10GB disk) as gateway.
   - Install OSes from ISOs uploaded to Proxmox.
4. **Configure Services:**
   - On Ubuntu: `sudo apt install nginx` and Nextcloud (snap or manual).
   - On Docker: Run `docker run -d nextcloud` inside Ubuntu VM.
   - On pfSense: Set firewall rules and WireGuard VPN.
5. **Secure the Environment:**
   - Add TLS to Nginx with Certbot (`sudo certbot --nginx`).
   - Harden SSH: Edit `/etc/ssh/sshd_config` (disable root, use keys).
6. **Monitoring & Testing:**
   - Install Wazuh manager on a VM and agents on others.
   - Use Kali to run Nmap/Metasploit, fix vulnerabilities found.
7. **Automation (Optional):**
   - Install Ansible on macOS (`brew install ansible`) and script VM configs.

### Usage
- Access web server: `https://<proxmox-ip>:80`.
- Log into Nextcloud: `https://<proxmox-ip>:443`.
- Monitor Wazuh dashboard for security events.
- Test attacks from Kali and verify detection.

### Reflections
Running Proxmox via UTM on macOS worked smoothly after tweaking network settings—nested virtualization was the trickiest part. Wazuh’s real-time alerts during my own attacks were a highlight. Next: Add redundancy or explore macOS-native tools like Parallels.

### License
MIT License - adapt and share freely!
