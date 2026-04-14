# 🔐 ARES-DEFENDER 2  
### Real-Time Cyber Attack Detection & Prevention System

---

## 📌 Overview

ARES-DEFENDER 2 is a lightweight **Intrusion Detection and Prevention System (IDPS)** designed for web applications.  

It monitors incoming HTTP requests, detects malicious activity in real time, and automatically blocks attackers using a unified detection pipeline.

The system demonstrates a complete cybersecurity lifecycle:

> **Attack → Detection → Decision → Prevention → Visualization**

---

## 🎯 Key Features

- 🔍 Real-time attack detection  
- ⚡ Automatic IP blocking (Prevention)  
- 🧠 Rule-based + behavioral analysis  
- 🔐 Zero-Trust security model  
- 📊 Live SOC-style dashboard  
- 🎛️ Demo Mode (controlled real input environment)  
- 🧾 Detailed logging & attack tracking  

---

## 🚨 Supported Attacks

The system focuses on high-impact, real-world attacks:

| Attack Type        | Description |
|------------------|------------|
| SQL Injection     | Detects payloads like `' OR 1=1 --` |
| XSS (Cross-Site Scripting) | Detects `<script>` injection |
| Brute Force       | Detects repeated failed login attempts |
| API Abuse         | Detects high-frequency request patterns |

---

## 🏗️ System Architecture


Incoming Request (User / Attacker)
↓
process_request()
↓
┌────────────────────┐
│ Detection Engine │
│ - Rule-based │
│ - Behavioral │
└────────────────────┘
↓
Decision Engine
↓
┌────────────────────┐
│ Mitigation Engine │
│ - Block IP │
│ - Deny access │
└────────────────────┘
↓
Dashboard UI


---

## ⚙️ Tech Stack

- **Backend:** Python, Flask  
- **Frontend:** HTML, CSS, JavaScript  
- **Security Logic:** Rule-based + Behavior Analysis  
- **System Integration:** Linux (iptables optional)  

---

## 🚀 Getting Started

### 1️⃣ Clone Repository

```bash
git clone https://github.com/your-username/ARES-DEFENDER.git
cd ARES-DEFENDER
2️⃣ Install Dependencies
pip install -r requirements.txt
3️⃣ Run Application
python app.py
4️⃣ Open Dashboard
http://localhost:5002
🧪 Demo Mode

The system runs in DEMO MODE by default:

✔ Only real HTTP inputs are processed
✔ No random attack simulation
✔ Clean and controlled demonstration
⚔️ Attack Simulation (Kali Linux / Terminal)
🔴 SQL Injection
curl -X POST http://localhost:5002/demo/search \
-d "query=' OR 1=1 --"
⚡ XSS Attack
curl -X POST http://localhost:5002/demo/comment \
-d "comment=<script>alert(1)</script>"
🔓 Brute Force Attack
for i in {1..6}; do
curl -X POST http://localhost:5002/demo/login \
-d "username=admin&password=wrong$i"
done
🛑 Verify Blocking
curl http://localhost:5002/demo/login

Expected response:

403 Forbidden
📊 Dashboard Features
Live attack feed
Attack type & severity
Blocked IP list
REAL MODE indicator
Zero-Trust scoring
🔐 Security Model

ARES-DEFENDER follows a Zero-Trust Architecture:

No request is trusted by default. Every request is verified based on behavior and payload.

📈 Detection Logic
Rule-Based Detection
Pattern matching (' OR 1=1, <script>)
Behavioral Detection
Login attempt thresholds
Request rate monitoring
⚠️ Limitations
Uses predefined rules (no advanced ML)
Limited to web-based attacks
Firewall blocking depends on system permissions
🚀 Future Enhancements
ML-based anomaly detection
Real network traffic monitoring
Cloud deployment
Advanced threat intelligence integration
👨‍💻 Author

Your Name
Cybersecurity Enthusiast | Python Developer

📜 License

This project is for educational and research purposes.
