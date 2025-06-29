# 🛡️ SOC Lab Setup with Open-Source Tools

## ⚙️ Overview
This repository provides a professional-grade step-by-step guide to building a **realistic blue vs red SOC home lab** using **free and open-source tools**. It’s designed for SOC Analysts, Blue Teamers, and Cybersecurity Enthusiasts who want to simulate real-world scenarios including:

- SIEM (Wazuh)
- EDR/DFIR (Velociraptor)
- IDS/IPS (Suricata)
- SOAR (Shuffle)
- Full Packet Capture (Arkime)
- Endpoint telemetry (Sysmon + Sigma)
- Attacker tooling with Kali Linux

> Built for training, testing, and security research in an isolated environment.

---

## 🧱 Lab Infrastructure

| System       | Role                  | OS                      | Purpose                                           |
|--------------|-----------------------|--------------------------|---------------------------------------------------|
| **Attacker** | Red Team Workstation  | Kali Linux (latest)      | Exploitation, phishing, C2, enumeration           |
| **Defender** | SOC + Endpoint Sensor | Ubuntu/Xubuntu or Win10  | SIEM, EDR, IDS, SOAR, packet analysis             |

---

## 🗂️ Suggested Project Structure

```bash
soc-lab-home/
├── tools/               # Scripts and helper tools
├── configs/             # Velociraptor configs, Sysmon rules, etc.
├── dashboards/          # Kibana/Wazuh/Shuffle dashboard exports
├── scripts/             # Setup and automation shell scripts
├── docs/                # Documentation and diagrams
└── README.md            # Project overview
```

---

## 🛠️ Setup Instructions

### 💻 Attacker Machine – Kali Linux

```bash
sudo apt update && sudo apt full-upgrade -y
sudo apt install -y nmap metasploit-framework bloodhound neo4j \
                   seclists evilginx2 responder powershell-empire \
                   curl git python3-pip
```

#### 🛠️ Optional Tools

- Covenant:
```bash
git clone --recurse-submodules https://github.com/cobbr/Covenant
cd Covenant/Covenant
dotnet build
```
- Gophish:
```bash
wget https://github.com/gophish/gophish/releases/download/v0.12.1/gophish-v0.12.1-linux-64bit.zip
unzip gophish-*.zip
cd gophish
./gophish
```

---

### 🛡️ Defender Machine – Ubuntu/Xubuntu

#### ✅ Docker + Update
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y docker.io docker-compose git curl
sudo systemctl enable --now docker
```

#### ✅ Wazuh (SIEM + EDR)
```bash
curl -sO https://packages.wazuh.com/4.8/wazuh-install.sh
sudo bash wazuh-install.sh --wazuh-indexer --wazuh-server --dashboard
```
Access: `https://<defender-ip>:5601`

#### ✅ Suricata (IDS)
```bash
sudo apt install -y suricata
sudo suricata -i <interface> -D
```

#### ✅ Velociraptor (EDR/DFIR)
```bash
wget https://github.com/Velocidex/velociraptor/releases/latest/download/velociraptor-linux-amd64
chmod +x velociraptor-linux-amd64
./velociraptor-linux-amd64 config generate > server.config.yaml
./velociraptor-linux-amd64 --config server.config.yaml frontend
```

#### ✅ Shuffle (SOAR)
```bash
git clone https://github.com/frikky/Shuffle.git
cd Shuffle
./start.sh
```
Access: `http://localhost:3001`

#### ✅ Arkime (Packet Capture)
```bash
wget https://s3.amazonaws.com/files.molo.ch/builds/arkime-3.5.1.x86_64.deb
sudo apt install ./arkime-*.deb
sudo /opt/arkime/bin/Configure
```

#### ✅ Sysmon + Sigma (on Windows)
- [Sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)
- [Sigma Rules](https://github.com/SigmaHQ/sigma)
- Use Winlogbeat to forward logs to Wazuh

---

## 🔁 Interconnections

| Flow                                | Description                              |
|-------------------------------------|------------------------------------------|
| Suricata ➡️ Wazuh                    | IDS alerts sent via Filebeat/syslog      |
| Velociraptor ➡️ Wazuh               | Endpoint telemetry + IOC collection      |
| Sysmon ➡️ Wazuh (via Winlogbeat)     | Windows telemetry into SIEM              |
| Wazuh ➡️ Shuffle                     | Alerts trigger SOAR playbooks            |
| Arkime ⬅️ Full packet access         | Query and index captured traffic         |

---

## 🔐 Dashboard Access

| Tool         | URL                             | Notes                  |
|--------------|----------------------------------|-------------------------|
| Wazuh        | `https://<defender-ip>:5601`     | SIEM/EDR dashboard     |
| Velociraptor | `https://<defender-ip>:8889`     | Forensics & hunting    |
| Shuffle      | `http://<defender-ip>:3001`      | SOAR playbooks         |
| Arkime       | `http://<defender-ip>:8005`      | Full packet capture UI |

---

## 📚 Resources

- 🔗 [Wazuh Documentation](https://documentation.wazuh.com/)
- 🔗 [Velociraptor Guides](https://docs.velociraptor.app/)
- 🔗 [Shuffle Automations](https://github.com/frikky/Shuffle)
- 🔗 [Suricata Rules](https://docs.suricata.io/)
- 🔗 [Arkime Setup](https://arkime.com/faq)
- 🔗 [Sigma Rule Repo](https://github.com/SigmaHQ/sigma)

---

## 🧠 Tips

- Use snapshots before simulating malware.
- Document incidents using [TheHive](https://github.com/TheHive-Project/TheHive)
- Use GitHub Projects or Issues to track lab goals.

---

## 📥 Contribution

Want to contribute? Fork this repo, add playbooks, detection rules, dashboards, or red-team scripts!

```bash
git clone https://github.com/<yourname>/soc-lab-home.git
```

---

## 🐉 Created by # 🛡️ SOC Lab Setup with Open-Source Tools

## ⚙️ Overview
This repository provides a professional-grade step-by-step guide to building a **realistic blue vs red SOC home lab** using **free and open-source tools**. It’s designed for SOC Analysts, Blue Teamers, and Cybersecurity Enthusiasts who want to simulate real-world scenarios including:

- SIEM (Wazuh)
- EDR/DFIR (Velociraptor)
- IDS/IPS (Suricata)
- SOAR (Shuffle)
- Full Packet Capture (Arkime)
- Endpoint telemetry (Sysmon + Sigma)
- Attacker tooling with Kali Linux

> Built for training, testing, and security research in an isolated environment.

---

## 🧱 Lab Infrastructure

| System       | Role                  | OS                      | Purpose                                           |
|--------------|-----------------------|--------------------------|---------------------------------------------------|
| **Attacker** | Red Team Workstation  | Kali Linux (latest)      | Exploitation, phishing, C2, enumeration           |
| **Defender** | SOC + Endpoint Sensor | Ubuntu/Xubuntu or Win10  | SIEM, EDR, IDS, SOAR, packet analysis             |

---

## 🗂️ Suggested Project Structure

```bash
soc-lab-home/
├── tools/               # Scripts and helper tools
├── configs/             # Velociraptor configs, Sysmon rules, etc.
├── dashboards/          # Kibana/Wazuh/Shuffle dashboard exports
├── scripts/             # Setup and automation shell scripts
├── docs/                # Documentation and diagrams
└── README.md            # Project overview
```

---

## 🛠️ Setup Instructions

### 💻 Attacker Machine – Kali Linux

```bash
sudo apt update && sudo apt full-upgrade -y
sudo apt install -y nmap metasploit-framework bloodhound neo4j \
                   seclists evilginx2 responder powershell-empire \
                   curl git python3-pip
```

#### 🛠️ Optional Tools

- Covenant:
```bash
git clone --recurse-submodules https://github.com/cobbr/Covenant
cd Covenant/Covenant
dotnet build
```
- Gophish:
```bash
wget https://github.com/gophish/gophish/releases/download/v0.12.1/gophish-v0.12.1-linux-64bit.zip
unzip gophish-*.zip
cd gophish
./gophish
```

---

### 🛡️ Defender Machine – Ubuntu/Xubuntu

#### ✅ Docker + Update
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y docker.io docker-compose git curl
sudo systemctl enable --now docker
```

#### ✅ Wazuh (SIEM + EDR)
```bash
curl -sO https://packages.wazuh.com/4.8/wazuh-install.sh
sudo bash wazuh-install.sh --wazuh-indexer --wazuh-server --dashboard
```
Access: `https://<defender-ip>:5601`

#### ✅ Suricata (IDS)
```bash
sudo apt install -y suricata
sudo suricata -i <interface> -D
```

#### ✅ Velociraptor (EDR/DFIR)
```bash
wget https://github.com/Velocidex/velociraptor/releases/latest/download/velociraptor-linux-amd64
chmod +x velociraptor-linux-amd64
./velociraptor-linux-amd64 config generate > server.config.yaml
./velociraptor-linux-amd64 --config server.config.yaml frontend
```

#### ✅ Shuffle (SOAR)
```bash
git clone https://github.com/frikky/Shuffle.git
cd Shuffle
./start.sh
```
Access: `http://localhost:3001`

#### ✅ Arkime (Packet Capture)
```bash
wget https://s3.amazonaws.com/files.molo.ch/builds/arkime-3.5.1.x86_64.deb
sudo apt install ./arkime-*.deb
sudo /opt/arkime/bin/Configure
```

#### ✅ Sysmon + Sigma (on Windows)
- [Sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)
- [Sigma Rules](https://github.com/SigmaHQ/sigma)
- Use Winlogbeat to forward logs to Wazuh

---

## 🔁 Interconnections

| Flow                                | Description                              |
|-------------------------------------|------------------------------------------|
| Suricata ➡️ Wazuh                    | IDS alerts sent via Filebeat/syslog      |
| Velociraptor ➡️ Wazuh               | Endpoint telemetry + IOC collection      |
| Sysmon ➡️ Wazuh (via Winlogbeat)     | Windows telemetry into SIEM              |
| Wazuh ➡️ Shuffle                     | Alerts trigger SOAR playbooks            |
| Arkime ⬅️ Full packet access         | Query and index captured traffic         |

---

## 🔐 Dashboard Access

| Tool         | URL                             | Notes                  |
|--------------|----------------------------------|-------------------------|
| Wazuh        | `https://<defender-ip>:5601`     | SIEM/EDR dashboard     |
| Velociraptor | `https://<defender-ip>:8889`     | Forensics & hunting    |
| Shuffle      | `http://<defender-ip>:3001`      | SOAR playbooks         |
| Arkime       | `http://<defender-ip>:8005`      | Full packet capture UI |

---

## 📚 Resources

- 🔗 [Wazuh Documentation](https://documentation.wazuh.com/)
- 🔗 [Velociraptor Guides](https://docs.velociraptor.app/)
- 🔗 [Shuffle Automations](https://github.com/frikky/Shuffle)
- 🔗 [Suricata Rules](https://docs.suricata.io/)
- 🔗 [Arkime Setup](https://arkime.com/faq)
- 🔗 [Sigma Rule Repo](https://github.com/SigmaHQ/sigma)

---

## 🧠 Tips

- Use snapshots before simulating malware.
- Document incidents using [TheHive](https://github.com/TheHive-Project/TheHive)
- Use GitHub Projects or Issues to track lab goals.

---

## 📥 Contribution

Want to contribute? Fork this repo, add playbooks, detection rules, dashboards, or red-team scripts!

```bash
git clone https://github.com/M1yu3Xpl0rer22/SOC-Analyst-Home-Lab.git
```

---

## 🐉 Created by Kali GPT And M1yu3Xpl0rer22
