# ARES-DEFENDER 2 â€” Project Architecture & Detailed Documentation

**Version:** 2.0.0  
**Date:** 2026  
**Type:** Automated Reconnaissance, Exploit Detection & Response System  
**Framework:** Flask (Python Web Framework)

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [System Architecture](#system-architecture)
3. [Directory Structure](#directory-structure)
4. [Core Components](#core-components)
5. [Module Deep Dive](#module-deep-dive)
6. [API Routes & Endpoints](#api-routes--endpoints)
7. [Data Flow & Workflows](#data-flow--workflows)
8. [Configuration](#configuration)
9. [Deployment & Targets](#deployment--targets)
10. [Security Features](#security-features)
11. [Frontend & UI](#frontend--ui)
12. [How to Make Changes](#how-to-make-changes)

---

## Project Overview

**ARES-DEFENDER 2** is an **Intrusion Detection & Prevention System (IDPS)** built with Flask that simulates a Security Operations Center (SOC) dashboard. It provides:

- **Real-time threat detection** using multiple detection layers
- **Zero-Trust architecture** with continuous IP verification
- **Automated response mechanisms** including IP blocking and lockdown mode
- **Live dashboard** for SOC operators to monitor security events
- **Detailed reporting** of security incidents
- **Target monitoring** for managing multiple protected applications

### Key Features:
âœ… Multi-layer threat detection (behavioral, rule-based, anomaly)  
âœ… Zero-Trust scoring engine  
âœ… Automatic IP blocking & firewall integration  
âœ… Attack event simulation for SOC demo/training  
âœ… GeoIP enrichment for attacker tracking  
âœ… Interactive web dashboard with real-time updates  
âœ… JSON-based structured logging  
âœ… System lockdown mode for emergency response  
âœ… Target node management & health monitoring  

---

## System Architecture

### High-Level Architecture Diagram

```
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚                    Flask Web Application                        â”‚
â”‚                      (app.py)                                   â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                             â”‚
        â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
        â”‚                    â”‚                    â”‚
        â–¼                    â–¼                    â–¼
   â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”          â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”         â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
   â”‚ Routes  â”‚          â”‚ Modules â”‚         â”‚ Frontend â”‚
   â”‚ (Flask) â”‚          â”‚ Logic   â”‚         â”‚ (HTML/JS)â”‚
   â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜          â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜         â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
        â”‚                    â”‚
        â”‚              â”Œâ”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
        â”‚              â”‚                   â”‚          â”‚         â”‚
        â–¼              â–¼                   â–¼          â–¼         â–¼
   â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
   â”‚ Dashboard    â”‚ Analyzer    â”‚ Detector     â”‚ Mitigationâ”‚ Logger  â”‚
   â”‚ (Stats API)  â”‚ (Event Gen) â”‚ (Threats)    â”‚ (Blocking)â”‚ (Logs)  â”‚
   â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
          â”‚              â”‚              â”‚              â”‚         â”‚
          â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                                â”‚
                    â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
                    â”‚                       â”‚
                    â–¼                       â–¼
              â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”      â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
              â”‚ Zero-Trust   â”‚      â”‚ File-based DB  â”‚
              â”‚ Engine       â”‚      â”‚ (Logs, Reports)â”‚
              â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜      â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                    â”‚
                    â–¼
              â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
              â”‚ Target Nodes â”‚
              â”‚ (Port 5002+) â”‚
              â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
```

---

## Directory Structure

```
ARES_DEFENDER_2/
â”œâ”€â”€ app.py                          # Main Flask application entry point
â”œâ”€â”€ config.py                       # Centralized configuration
â”œâ”€â”€ requirements.txt                # Python dependencies
â”œâ”€â”€ start_targets.py               # Script to launch target nodes
â”‚
â”œâ”€â”€ modules/                       # Core business logic
â”‚   â”œâ”€â”€ __init__.py
â”‚   â”œâ”€â”€ analyzer.py               # Traffic analyzer & attack simulator
â”‚   â”œâ”€â”€ detector.py               # Multi-layer threat detection
â”‚   â”œâ”€â”€ geoip.py                  # IP geolocation service
â”‚   â”œâ”€â”€ log_monitor.py            # Background log monitoring thread
â”‚   â”œâ”€â”€ logger.py                 # Structured security logging
â”‚   â”œâ”€â”€ mitigation.py             # IP blocking & firewall control
â”‚   â”œâ”€â”€ monitor.py                # Central orchestrator (Security Monitor)
â”‚   â”œâ”€â”€ report_gen.py             # JSON report generation
â”‚   â”œâ”€â”€ target_monitor.py         # Target node tracking
â”‚   â””â”€â”€ zero_trust.py             # Zero-Trust scoring engine
â”‚
â”œâ”€â”€ routes/                        # Flask blueprints (API endpoints)
â”‚   â”œâ”€â”€ __init__.py
â”‚   â”œâ”€â”€ actions.py                # Control actions (blocking, mode, etc)
â”‚   â”œâ”€â”€ alerts.py                 # Attack event feed
â”‚   â”œâ”€â”€ dashboard.py              # Stats & main dashboard APIs
â”‚   â””â”€â”€ demo.py                   # Demo/testing endpoints
â”‚
â”œâ”€â”€ targets/                       # Target application nodes (under attack)
â”‚   â””â”€â”€ main.py                   # Target node script
â”‚
â”œâ”€â”€ templates/                     # HTML templates
â”‚   â”œâ”€â”€ base.html                 # Base template
â”‚   â”œâ”€â”€ index.html                # Main dashboard
â”‚   â”œâ”€â”€ login.html                # Login page
â”‚   â””â”€â”€ demo/                      # Demo page templates
â”‚       â”œâ”€â”€ home.html
â”‚       â”œâ”€â”€ login.html
â”‚       â”œâ”€â”€ search.html
â”‚       â””â”€â”€ comment.html
â”‚
â”œâ”€â”€ static/                        # Static assets
â”‚   â”œâ”€â”€ css/
â”‚   â”‚   â”œâ”€â”€ style.css             # Main stylesheet
â”‚   â”‚   â””â”€â”€ demo.css              # Demo page styles
â”‚   â””â”€â”€ js/
â”‚       â””â”€â”€ dashboard.js          # Frontend logic
â”‚
â”œâ”€â”€ logs/                          # Runtime logs (generated)
â”‚   â””â”€â”€ security_structured.json   # Structured security events
â”‚
â””â”€â”€ reports/                       # Generated security reports
    â””â”€â”€ ares_report_*.json        # Timestamped JSON reports
```

---

## Core Components

### 1. **Flask Application (app.py)**
The main entry point that initializes Flask and registers all blueprints.

**Key Responsibilities:**
- Create Flask app instance with proper configuration
- Register route blueprints (dashboard, alerts, actions, demo)
- Implement before-request middleware for IP blocklist enforcement
- Implement after-request middleware for self-monitoring/logging
- Handle global error handlers
- Provide internal API for target nodes (`/api/check_block`)

**Key Functions:**
```python
create_app()              # Application factory
enforce_ip_blocklist()    # Middleware: block requests from blocked IPs
log_traffic()             # Middleware: self-monitoring log
```

---

### 2. **Configuration (config.py)**
Centralized configuration management for the entire system.

**Key Settings:**
```python
# Application
APP_NAME = "ARES-DEFENDER"
APP_VERSION = "2.0.0"
SECRET_KEY = "ares-defender-secret-key-change-in-production"
DEBUG = True
PORT = 5005
HOST = "127.0.0.1"

# Security Thresholds
ZERO_TRUST_DEFAULT_SCORE = 50      # Starting trust for new IPs
ZERO_TRUST_THRESHOLD = 30          # Below this = Blocked
ZERO_TRUST_TRUSTED = 75            # Above this = Trusted
IP_BLOCK_DURATION = 300            # 5 minutes

# Attack Simulation Data
ATTACKER_IPS = [...]               # Pool of simulated attacker IPs
IP_GEO_MAP = {...}                 # GeoIP data for attackers
ATTACK_SIGNATURES = {...}          # Attack patterns to detect
TARGET_SERVICES = [...]            # Target applications to protect

# Behavior Thresholds
BEHAVIOR_FAILED_LOGIN_LIMIT = 5    # Brute force detection
BEHAVIOR_REQ_PER_SEC_LIMIT = 20    # Rate limiting threshold
```

**Usage:** Import from config throughout the codebase for consistent configuration.

---

## Module Deep Dive

### **1. analyzer.py â€” Traffic Analyzer & Attack Simulator**

**Purpose:** Simulates realistic network traffic and generates attack events for dashboard demo.

**Class: `TrafficAnalyzer`**

**Key Attributes:**
- `attack_history`: Rolling history of attack events
- `system_mode`: "NORMAL" or "LOCKDOWN"
- `attack_counts`: Counters for each attack type
- `request_frequency`: Per-IP request counts
- `whitelist`: List of allowed IPs

**Key Methods:**
```python
generate_random_attack()           # Create random attack event
analyze_behavior(ip, is_login_fail)# Detect behavioral anomalies
get_system_stats()                 # Return dashboard statistics
```

**Attack Types Simulated:**
- SQL Injection (OWASP A03)
- XSS Attack (OWASP A07)
- Remote Code Execution (RCE)
- Brute Force attacks
- DDoS floods
- Port scans
- Man-in-the-Middle (MITM)
- Local File Inclusion (LFI)
- Path Traversal
- Rate limit violations

---

### **2. detector.py â€” Multi-Layer Threat Detection**

**Purpose:** Comprehensive threat detection using behavioral, rule-based, and anomaly detection.

**Class: `DetectionEngine`**

**Detection Layers:**
1. **Behavioral Layer**: Detects rate limiting and brute force attacks
2. **Rule-Based Layer**: Matches against attack signatures (SQL, XSS, RCE, etc)
3. **Anomaly Layer**: Entropy analysis for obfuscated payloads
4. **Context-Aware**: IP reputation and Zero-Trust scores

**Key Methods:**
```python
analyze_event(source_ip, payload, target_name,    # Complete event analysis
              target_url, request_method, is_auth_attempt, auth_success)

_check_behavioral(source_ip)        # Behavioral anomaly detection
_check_entropy(data)                # Detect obfuscated/encoded payloads
_check_signatures(payload)          # Pattern matching against known attacks
_analyze_reputation(source_ip)      # Check IP reputation & Zero-Trust
```

**Output:** Returns detailed threat assessment with:
- `malicious`: Boolean verdict
- `type`: Attack category
- `severity`: HIGH/MEDIUM/LOW
- `explanation`: Human-readable threat description
- `mitigation`: Action taken (Auto Blocked, Reported, etc)
- `owasp`: Relevant OWASP category

---

### **3. mitigation.py â€” Mitigation Engine**

**Purpose:** Execute defensive actions including IP blocking and firewall integration.

**Class: `MitigationEngine`**

**Key Capabilities:**
- **IP Blocking**: Instantly block IPs via `iptables` (Linux) or internal database
- **Session Termination**: Terminate active sessions for blocked IPs
- **Block Expiry**: Automatic block expiration after configured duration
- **Audit Logging**: Track all mitigation actions

**Key Methods:**
```python
block_ip(ip, duration=300, reason)     # Block IP for duration
unblock_ip(ip)                         # Remove IP from blocklist
is_ip_blocked(ip)                      # Check if IP is currently blocked
terminate_sessions(ip)                 # Kill all sessions from IP
get_active_blocks()                    # List currently blocked IPs
```

**Linux Integration:**
```bash
iptables -A INPUT -s <IP> -j DROP     # Add to firewall
iptables -D INPUT -s <IP> -j DROP     # Remove from firewall
```

---

### **4. zero_trust.py â€” Zero-Trust Scoring Engine**

**Purpose:** Continuous per-IP trust verification. No implicit trust is ever granted.

**Class: `ZeroTrustEngine`**

**Trust Scoring Logic:**
- **Starting Score**: 50/100 (neutral)
- **Blocked Range**: 0-30 (blocked)
- **Restricted Range**: 30-75 (limited access)
- **Trusted Range**: 75-100 (full access)

**Key Methods:**
```python
get_score(identity)                    # Get current trust score
reduce_trust(identity, penalty=15)     # Penalize for suspicious behavior
boost_trust(identity, amount=2)        # Passive trust recovery
reset_trust(identity)                  # Admin override (set to 80)
verify_request(identity)               # Evaluate request access
get_all_scores()                       # Return all tracked IPs
```

**Trust Dynamics:**
- All requests receive passive trust boost (+1 per call)
- Malicious events cause trust penalties (varies by severity)
- Trust scores capped between 0-100
- Blocked IPs cannot make requests regardless of score

---

### **5. logger.py â€” Security Logger**

**Purpose:** Structured logging of all security events in multiple formats.

**Class: `SecurityLogger`**

**Logging Outputs:**
1. **Standard Log File** (`security.log`): Human-readable format
2. **Structured JSON** (`security_structured.json`): Machine-readable format
3. **Console Output**: Real-time alerts

**Log Entry Structure:**
```json
{
  "ip_address": "185.12.34.12",
  "attack_type": "SQL Injection",
  "target_name": "ARES-Defender",
  "target_url": "/api/search",
  "timestamp": "2026-04-09T12:34:56.789123",
  "payload": "SELECT * FROM users;--",
  "request_method": "POST",
  "severity": "HIGH",
  "action_taken": "Auto Blocked",
  "country": "Russia",
  "city": "Moscow"
}
```

**Key Methods:**
```python
log_incident(ip_address, attack_type, action_taken, severity, ...)
log_system(message)                    # Log system events
get_recent_logs(limit=50)              # Retrieve recent logs
get_log_count()                        # Total logged events
```

---

### **6. monitor.py â€” Security Monitor (Central Orchestrator)**

**Purpose:** Single point of truth. Orchestrates all subsystems and provides unified status API.

**Class: `SecurityMonitor`**

**Responsibilities:**
- Aggregate status from all subsystems
- Provide unified dashboard API
- Control system modes (NORMAL/LOCKDOWN)
- Manage manual payload testing
- Generate security reports
- Provide attack event feed

**Key Methods:**
```python
get_full_status()                      # Comprehensive system snapshot
set_system_mode(mode)                  # Switch NORMAL â†” LOCKDOWN
get_attack_feed(auto_generate=True)    # Attack event history
test_payload(payload, source_ip)       # Manual threat testing
generate_report()                      # Create JSON report
```

**Full Status Response:**
```json
{
  "uptime": "0:02:34",
  "system_mode": "NORMAL",
  "status": "OPERATIONAL",
  "total_threats": 1245,
  "blocked_ips": 23,
  "mitigation_rate": 95.2,
  "active_alerts": 12,
  "auto_blocked": 156,
  "total_requests": 50234,
  "analytics": {...},
  "mitigation_summary": {...},
  "zero_trust_nodes": [{...}],
  "log_count": 5432,
  "target_status": [{...}]
}
```

---

### **7. geoip.py â€” GeoIP Provider**

**Purpose:** Enrich threat intelligence with geographical IP data.

**Class: `GeoIPProvider`**

**Features:**
- IP geolocation lookup (country, city, ISP)
- Emoji flag generation for countries
- Caching to reduce API calls
- Fallback to config data

**Data Sources:**
- Primary: `ip-api.com` free API
- Fallback: `IP_GEO_MAP` from config.py
- Hardcoded: Localhost/internal addresses

**Key Methods:**
```python
lookup(ip)                             # Get IP metadata
_get_flag(country_code)                # Convert code to emoji ðŸ‡·ðŸ‡º
```

**Response Format:**
```json
{
  "country": "Russia",
  "city": "Moscow",
  "country_code": "RU",
  "isp": "ISP Name",
  "flag": "ðŸ‡·ðŸ‡º"
}
```

---

### **8. report_gen.py â€” Report Generator**

**Purpose:** Generate JSON-formatted security incident reports.

**Class: `ReportGenerator`**

**Key Methods:**
```python
generate_json_report(stats, alerts, blocked_ips) -> filename
```

**Report Contents:**
- Generated timestamp
- System version
- Summary statistics
- List of blocked IPs
- Recent attack events (last 20)

**Output:** Timestamped JSON file in `reports/` directory
```
ares_report_20260305_082218.json
```

---

### **9. target_monitor.py â€” Target Node Manager**

**Purpose:** Track and manage multiple target applications being protected.

**Key Functionality:**
- Add/remove targets from monitoring
- Track target health & availability
- Report target status on dashboard

**Data Structure:**
```json
{
  "id": "target-123",
  "name": "Web App Server",
  "url": "http://localhost:5002",
  "status": "ONLINE",
  "last_check": "2026-04-09T12:34:56",
  "response_time_ms": 45
}
```

---

### **10. log_monitor.py â€” Background Log Monitor**

**Purpose:** Background thread that continuously monitors log files for security events.

**Features:**
- Runs in separate thread
- Actively watches security logs
- Triggers alerts on suspicious patterns
- Updates real-time dashboard

---

## API Routes & Endpoints

### **Dashboard Routes** (`routes/dashboard.py`)

| Route | Method | Purpose |
|-------|--------|---------|
| `/` | GET | Serve main SOC dashboard (index.html) |
| `/login` | GET | Serve admin login page |
| `/api/stats` | GET | Real-time system metrics |
| `/api/logs` | GET | Recent security log entries (limit: 50) |
| `/api/generate_report` | GET | Trigger JSON report generation |
| `/reports/<filename>` | GET | Download generated report |
| `/api/add_target` | POST | Add new monitoring target |
| `/api/remove_target` | POST | Remove monitoring target |

**Example: `/api/stats` Response:**
```json
{
  "uptime": "0:15:32",
  "system_mode": "NORMAL",
  "status": "OPERATIONAL",
  "total_threats": 1245,
  "blocked_ips": 23,
  "mitigation_rate": 95.2,
  ...
}
```

---

### **Alert Routes** (`routes/alerts.py`)

| Route | Method | Purpose |
|-------|--------|---------|
| `/api/alerts` | GET | Live attack event feed (auto-generates new events) |
| `/api/analytics` | GET | Per-attack-type frequency counters |

**Example: `/api/alerts` Response:**
```json
[
  {
    "timestamp": "2026-04-09T12:34:56",
    "source_ip": "185.12.34.12",
    "country": "Russia",
    "type": "SQL Injection",
    "severity": "HIGH",
    "target": "ARES-Defender",
    "action": "Auto Blocked",
    "payload": "SELECT * FROM users;--",
    ...
  },
  ...
]
```

---

### **Action Routes** (`routes/actions.py`)

| Route | Method | Purpose |
|-------|--------|---------|
| `/api/toggle_mode` | POST | Switch NORMAL â†” LOCKDOWN mode |
| `/api/test_payload` | POST | Analyze custom payload for threats |
| `/api/block_ip` | POST | Manually block an IP |
| `/api/unblock_ip` | POST | Unblock a previously blocked IP |
| `/api/reset_trust` | POST | Reset Zero-Trust score for IP (admin override) |

**Example: `/api/test_payload` Request:**
```json
{
  "payload": "SELECT * FROM users WHERE id=1;--",
  "source_ip": "192.168.1.100"
}
```

**Example: `/api/test_payload` Response:**
```json
{
  "malicious": true,
  "type": "SQL Injection",
  "severity": "HIGH",
  "explanation": "OWASP A03 â€” SQL injection pattern matched...",
  "mitigation": "Auto Blocked",
  "confidence": 0.95
}
```

---

### **Demo Routes** (`routes/demo.py`)

Routes for testing and demonstration purposes (home, search, comment endpoints for simulated target apps).

---

## Data Flow & Workflows

### **Attack Detection & Response Workflow**

```
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚  Incoming Request   â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
           â”‚
           â–¼
    â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
    â”‚  Extract metadata   â”‚
    â”‚  (IP, payload, etc) â”‚
    â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
               â”‚
               â–¼
        â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
        â”‚ 1. Check if IP is already       â”‚
        â”‚    blocked?                     â”‚
        â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                  â”‚
        â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”
        â”‚ YES              â”‚ NO
        â–¼                  â–¼
    REJECT           â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
    (403)            â”‚ 2. Behavioral Layer:  â”‚
                     â”‚    Rate limit check   â”‚
                     â”‚    Brute force check  â”‚
                     â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                                â”‚
                      â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”
                      â”‚ THREAT           â”‚ OK
                      â”‚ DETECTED         â”‚
                      â–¼                  â–¼
                 BLOCK IP         â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
                                  â”‚ 3. Rule-based:   â”‚
                                  â”‚    Signature     â”‚
                                  â”‚    matching      â”‚
                                  â””â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                                           â”‚
                                 â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”
                                 â”‚ THREAT           â”‚ OK
                                 â”‚ DETECTED         â”‚
                                 â–¼                  â–¼
                            BLOCK IP        â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
                                            â”‚ 4. Zero-Trust:   â”‚
                                            â”‚    Check score   â”‚
                                            â””â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                                                     â”‚
                                           â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”
                                           â”‚ Blocked          â”‚ OK
                                           â”‚
                                           â–¼
                                    â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
                                    â”‚ 5. Log event       â”‚
                                    â”‚ 6. Update stats    â”‚
                                    â”‚ 7. Allow request   â”‚
                                    â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
```

### **IP Blocking Workflow**

```
block_ip(ip, duration, reason)
    â””â”€> Check if whitelisted
            â””â”€> If yes: SKIP
            â””â”€> If no: Continue
                â””â”€> Calculate expiry timestamp
                â””â”€> Add to internal blocklist
                â””â”€> If Linux: Execute iptables command
                â””â”€> Log block event
                â””â”€> Return success
```

### **Zero-Trust Verification Workflow**

```
verify_request(ip)
    â””â”€> Boost trust (+1, capped at 100)
    â””â”€> Get current score
    â””â”€> Evaluate status:
            Score â‰¥ 75      â†’ "Trusted"
            30-75           â†’ "Restricted"
            < 30            â†’ "Blocked"
    â””â”€> Return verification result
```

### **Report Generation Workflow**

```
generate_report()
    â””â”€> Collect system stats
    â””â”€> Gather all attack events
    â””â”€> List blocked IPs
    â””â”€> Compile report object
    â””â”€> Serialize to JSON
    â””â”€> Save with timestamp: ares_report_YYYYMMDD_HHMMSS.json
    â””â”€> Return filename
```

---

## Configuration

### **Security Thresholds**
```python
ZERO_TRUST_DEFAULT_SCORE = 50      # New IPs start neutral
ZERO_TRUST_THRESHOLD = 30          # Below = Blocked
ZERO_TRUST_TRUSTED = 75            # Above = Trusted
IP_BLOCK_DURATION = 300            # 5 minutes in seconds
WHITELIST_IPS = ["127.0.0.1"]      # Always allowed
```

### **Behavioral Limits**
```python
BEHAVIOR_FAILED_LOGIN_LIMIT = 5        # Brute force trigger
BEHAVIOR_REQ_PER_SEC_LIMIT = 20        # Rate limiting threshold
```

### **Simulated Attack Data**
See `config.py` for:
- `ATTACKER_IPS`: Pool of simulated attacker IPs
- `IP_GEO_MAP`: GeoIP data for each attacker
- `ATTACK_SIGNATURES`: Pattern definitions for detection
- `TARGET_SERVICES`: Applications being protected
- `SEVERITY_COLORS`: Color codes for UI

---

## Deployment & Targets

### **Running ARES-DEFENDER**

1. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

2. **Start the main application:**
   ```bash
   python app.py
   ```
   - Runs on `http://127.0.0.1:5005`
   - Log monitoring starts automatically

3. **Start target applications (optional):**
   ```bash
   python start_targets.py
   ```
   - Launches Target-App-3 (port 5002)
   - Launches Target-App-4 (port 5003)
   - These simulate applications under attack

### **Target Node Architecture** (`targets/main.py`)

Target nodes are simulated applications that:
- Receive requests from the main ARES system
- Report their status back to the dashboard
- Can be monitored for health/availability
- Participate in the attack simulation

---

## Security Features

### **1. Multi-Layer Detection**
- Behavioral anomaly detection (rate limiting, brute force)
- Rule-based signature matching (OWASP Top 10)
- Entropy analysis for obfuscated payloads
- Zero-Trust continuous verification

### **2. Automated Response**
- Instant IP blocking via firewall integration
- Session termination for blocked users
- System-wide lockdown mode
- Configurable block durations with auto-expiry

### **3. Zero-Trust Architecture**
- Every request verified continuously
- No implicit trust granted
- Dynamic scoring based on behavior
- Passive trust recovery over time

### **4. Firewall Integration** (Linux-specific)
- Uses `iptables` for kernel-level IP filtering
- Faster than application-level blocking
- Automatic cleanup on block expiration

### **5. Whitelist Protection**
- Whitelisted IPs bypass all security checks
- Perfect for admin/trusted sources
- Configurable in `config.py`

### **6. Attack Simulation**
- Realistic attack pattern generation
- OWASP Top 10 coverage
- GeoIP-enriched threat intelligence
- Perfect for SOC training

---

## Frontend & UI

### **Main Dashboard** (`templates/index.html`)

Real-time SOC dashboard displaying:
- System status & uptime
- Active threat count
- Blocked IPs
- Attack event feed (live updates)
- Attack type analytics (charts)
- Mitigation summary
- Zero-Trust scores for tracked IPs
- Target node health

### **Admin Controls**
- System mode toggle (NORMAL â†” LOCKDOWN)
- Manual IP blocking/unblocking
- Payload testing interface
- Trust score reset
- Report generation

### **Interactive Elements**
- Live updating attack feed
- Real-time statistics
- Drilldown capabilities
- Responsive design

### **Frontend Stack**
- HTML5 templates
- CSS for styling
- JavaScript for interactivity & AJAX updates
- Data fetched from REST API

---

## How to Make Changes

### **Adding a New Attack Type**

1. **Add signature to `config.py`:**
   ```python
   ATTACK_SIGNATURES = {
       ...
       "New Attack": {
           "patterns": [...],
           "severity": "HIGH",
           "owasp": "A99"
       }
   }
   ```

2. **Update detector.py** to handle new attack:
   ```python
   # In _check_signatures()
   if "new_pattern" in payload.lower():
       return {...}
   ```

3. **Update analyzer.py** attack explanations:
   ```python
   EXPLANATIONS = {
       ...
       "New Attack": "Description of new attack pattern..."
   }
   ```

4. **Test via `/api/test_payload` endpoint**

---

### **Modifying Security Thresholds**

Edit `config.py`:
```python
ZERO_TRUST_DEFAULT_SCORE = 50       # â† Change default
ZERO_TRUST_THRESHOLD = 30           # â† Block threshold
ZERO_TRUST_TRUSTED = 75             # â† Trust threshold
IP_BLOCK_DURATION = 300             # â† Duration in seconds
BEHAVIOR_REQ_PER_SEC_LIMIT = 20     # â† Rate limit
```

Changes apply immediately on next request (no restart needed for most values).

---

### **Adding a New Route**

1. **Create route file** in `routes/`:
   ```python
   from flask import Blueprint, jsonify
   
   new_bp = Blueprint("new", __name__)
   
   @new_bp.route("/api/new_endpoint")
   def new_endpoint():
       return jsonify({"status": "success"})
   ```

2. **Register in `app.py`:**
   ```python
   from routes.new import new_bp
   app.register_blueprint(new_bp)
   ```

3. **Restart application**

---

### **Extending the Logger**

Add custom log types in `logger.py`:
```python
def log_custom_event(self, event_type: str, details: dict):
    """Log custom security event."""
    entry = {
        "event_type": event_type,
        "timestamp": datetime.now().isoformat(),
        "details": details
    }
    # Write to log files...
```

---

### **Modifying the Dashboard**

1. **Update API endpoint** in `routes/dashboard.py` to return new data
2. **Update HTML** in `templates/index.html`
3. **Update JavaScript** in `static/js/dashboard.js` to fetch and render
4. **Update CSS** in `static/css/style.css` for styling

---

### **Adding a New Target**

1. **Add to `TARGETS` list** in `start_targets.py`:
   ```python
   TARGETS = [
       ("Target-App-3", 5002),
       ("Target-App-4", 5003),
       ("New-Target-5", 5004),  # â† Add here
   ]
   ```

2. **Run** `python start_targets.py`

---

### **Integrating Real Firewall**

Replace `iptables` calls in `mitigation.py`:
```python
# Current (Linux iptables):
subprocess.run(["sudo", "iptables", "-A", "INPUT", "-s", ip, "-j", "DROP"])

# For Windows Firewall:
subprocess.run(["netsh", "advfirewall", "firewall", "add", "rule", ...])

# For Windows Defender:
# Use WinRM or PowerShell integration

# For cloud (AWS):
# Use boto3 to modify security group rules

# For cloud (Azure):
# Use Azure SDK to modify network security groups
```

---

### **Scaling Considerations**

1. **Redis for distributed blocking**: Replace in-memory `blocked_ips` dict
   ```python
   self.redis = redis.Redis()
   self.redis.setex(ip, IP_BLOCK_DURATION, "blocked")
   ```

2. **Database for audit logs**: Replace file-based logging
   ```python
   self.db.insert("security_logs", log_entry)
   ```

3. **Message queue for high volume**: Use Kafka/RabbitMQ for event streaming

4. **Load balancer**: Distribute requests across multiple app instances

5. **Caching layer**: Redis for frequently accessed data

---

## Key Takeaways

ðŸ” **ARES-DEFENDER 2** is a comprehensive **IDPS** demonstrating:
- Modern threat detection techniques
- Zero-Trust security model
- Automated incident response
- Real-time SOC dashboard
- Production-ready logging & reporting

ðŸŽ¯ **Perfect for:**
- Security training & SOC simulations
- Threat detection learning
- Incident response practice
- Enterprise security architecture studying
- Penetration testing platforms

ðŸš€ **Extensible** to real firewall integration, database backends, and cloud platforms.

---

## Support & Debugging

### **Common Issues**

**1. iptables permission denied (Linux)**
- Solution: Run with `sudo` or add user to sudo group

**2. Port already in use**
- Change PORT in `config.py`
- Or kill existing process: `lsof -i :5005`

**3. Missing log files**
- Directory auto-creates, check write permissions

**4. GeoIP API rate limited**
- Fallback uses config data automatically
- Consider caching or premium API

---

**Generated:** April 9, 2026  
**Last Updated:** 2026  
**Status:** Production-Ready v2.0.0
