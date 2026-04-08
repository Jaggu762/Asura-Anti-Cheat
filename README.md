# Asura — Advanced Anti-Cheat Exam Proctoring System

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-2.x-000?logo=flask&logoColor=white)
![Socket.IO](https://img.shields.io/badge/Socket.IO-4.5.4-010101?logo=socketdotio&logoColor=white)
![License](https://img.shields.io/badge/License-Academic-blue)

Asura is a comprehensive Flask-based online examination platform with advanced anti-cheat enforcement, AI-powered violation detection, real-time monitoring, and instant exam termination capabilities. Built for educational institutions requiring robust exam integrity tools.

---

## Table of Contents

1. [Features](#features)
2. [Architecture Overview](#architecture-overview)
3. [Repository Layout](#repository-layout)
4. [Quick Start](#quick-start)
5. [User Roles & Workflows](#user-roles--workflows)
6. [Anti-Cheat System](#anti-cheat-system)
7. [AI-Powered Detection](#ai-powered-detection)
8. [Real-Time Monitoring](#real-time-monitoring)
9. [Native Monitoring Toolkit](#native-monitoring-toolkit-windows)
10. [Browser Extension](#browser-extension)
11. [CI/CD Automation](#cicd-automation-github-actions)
12. [Configuration](#configuration)
13. [Security & Privacy](#security--privacy)
14. [Tech Stack](#tech-stack)
15. [Contributing](#contributing)

---

## Features

### 🎓 **Exam Management**
- **Multi-role system** — Admin, Lecturer, Staff, and Student roles with distinct permissions
- **Batch-based organization** — Group students by class/section for targeted exam distribution
- **Timed exams** — Per-student duration with automatic deadline enforcement
- **Question bank** — Multiple-choice questions with instant grading
- **Results publishing** — Control when students can view their scores
- **Auto-grading** — Automatic scoring with manual override capability

### 🔒 **Advanced Anti-Cheat Protection**
- **Fullscreen enforcement** — Exams require fullscreen mode; exits trigger violations
- **Comprehensive shortcut blocking** — Prevents Alt+Tab, Alt+F4, Win key, PrintScreen, Ctrl+S/P, Win+Shift+S, DevTools (F12)
- **Screenshot deterrence** — White overlay on suspected capture attempts with event logging
- **Tab switch detection** — Monitors window blur/focus events
- **Window visibility tracking** — Detects minimizing or hiding exam window
- **Process monitoring** — Optional Windows agent reports foreground applications
- **Real-time violation logging** — All suspicious activities timestamped and recorded

### 🤖 **AI-Powered Violation Detection**
- **Intelligent event analysis** — ML-based analysis of student behavior patterns
- **Severity classification** — CRITICAL, HIGH, MEDIUM, LOW risk categorization
- **Violation type identification**:
  - 🚫 Fullscreen exits (CRITICAL)
  - 📸 Screenshot/cheating shortcuts (HIGH)
  - 🔄 Tab switching patterns (MEDIUM)
  - 👁️ Window blur/hidden events (MEDIUM)
- **Activity summaries** — Detailed breakdown of each violation with counts
- **Dashboard visualization** — Color-coded alerts for quick assessment

### ⚡ **Real-Time Features**
- **Live monitoring dashboard** — Socket.IO powered real-time event streaming
- **Instant exam termination** — Teachers/staff can terminate exams immediately for cheating
- **Student notifications** — Real-time alerts when exam is terminated
- **Auto-save system** — Answers saved every 30 seconds + on termination
- **Live event feeds** — Watch student activities as they happen
- **Multi-attempt monitoring** — Staff can oversee all active exams simultaneously

### 🎨 **Modern Professional UI**
- **Professional exam portal design** — Clean, distraction-free interface
- **Security-focused color scheme** — Trust-inspiring blue gradients
- **Responsive layout** — Works on all screen sizes
- **Glassmorphism effects** — Modern card designs with blur effects
- **Smooth animations** — Polished interactions throughout
- **Accessible design** — High contrast, readable fonts (Roboto)

### 📊 **Reporting & Analytics**
- **Detailed event logs** — CSV export of all student activities
- **Violation reports** — AI-generated suspicious activity summaries
- **Score tracking** — Complete grade history with publish controls
- **Termination records** — Students see why they received 0% for cheating
- **Audit trails** — Who terminated exams and when

---

## Architecture Overview

```text
┌─────────────┐
│   Students  │──┐
└─────────────┘  │
                 ├──> Flask Web App ──> SQLite Database
┌─────────────┐  │         │
│ Lecturers   │──┤         ├──> Socket.IO Server (Real-time)
└─────────────┘  │         │           │
                 │         │           ├──> Live Event Stream
┌─────────────┐  │         │           └──> Termination Signals
│    Staff    │──┤         │
└─────────────┘  │         ├──> AI Analysis Engine
                 │         │           └──> Violation Detection
┌─────────────┐  │         │
│   Admins    │──┘         └──> Anti-Cheat System
└─────────────┘                    ├──> Browser Enforcement
                                   ├──> Windows Agent (Optional)
                                   └──> Chrome Extension (Optional)
```

---

## Repository Layout

| Path | Purpose |
|------|---------|
| `app.py` | Main Flask application (1579 lines) — models, routes, Socket.IO handlers, AI detection |
| `templates/` | Jinja2 templates with modern UI |
| `├─ base.html` | Professional exam portal theme with gradients |
| `├─ take_exam.html` | Student exam interface with anti-cheat enforcement + real-time termination |
| `├─ exam_ai_alerts.html` | AI-powered violation dashboard with termination controls |
| `├─ student_results.html` | Score display with termination indicators |
| `├─ live_attempt_events.html` | Real-time event monitoring for staff |
| `scripts/agent_win_monitor.py` | Windows background agent for process monitoring |
| `student_monitor_app.py` | Tkinter GUI launcher with process termination |
| `exam_proctor_extension/` | Chrome extension for tab management and token display |
| `static/` | Static assets (CSS/JS) |
| `server/` | Node.js components (if applicable) |

---

## Quick Start

### Prerequisites
- Python 3.10+
- pip and virtualenv
- Windows OS (for native agent)

### Installation (Windows PowerShell)

```powershell
# Clone repository
cd "S:\Semester 6\ST6047CEM Cyber Security Project\CODE"

# Create virtual environment
python -m venv .venv
.\.venv\Scripts\Activate.ps1

# Install dependencies
pip install -r requirements.txt

# Run application
python app.py
```

### First Login
Default admin credentials (⚠️ **CHANGE IMMEDIATELY**):
```
Username: admin
Password: adminpass
```

### Server Details
- **URL**: http://localhost:25570
- **Database**: SQLite (`app.db` in project root)
- **WebSocket**: Socket.IO v4.5.4

---

## User Roles & Workflows

### 👨‍💼 **Admin**
- Full system access
- User management (create/edit/delete lecturers, staff, students)
- Batch creation and assignment
- Password management
- System oversight

### 👨‍🏫 **Lecturer**
- Create and configure exams
- Add/edit/delete questions
- Start exam sessions
- View student attempts and scores
- Access AI violation alerts
- Terminate exams for cheating
- Publish/unpublish results
- Grade manual submissions

### 👨‍💼 **Staff**
- Monitor all active exam attempts in real-time
- View live event streams for all students
- Access AI violation dashboards
- Terminate exams for suspicious activity
- Export event logs
- No exam creation or grading permissions

### 👨‍🎓 **Student**
- View available exams for their batch
- Take exams within scheduled windows
- Auto-save progress every 30 seconds
- View published results
- See termination reasons if applicable

---

## Anti-Cheat System

### Browser-Level Enforcement (`take_exam.html`)

```javascript
// Fullscreen requirement
- Exam hidden until fullscreen activated
- Automatic termination on fullscreen exit
- ESC key blocked during exam

// Shortcut blocking
- Alt+Tab, Alt+F4: Window switching prevention
- Win key: Start menu blocked
- PrintScreen, Win+Shift+S: Screenshot prevention
- Ctrl+S, Ctrl+P: Save/print blocked
- F12, Ctrl+Shift+I: DevTools disabled
- Ctrl+U: View source blocked

// Event tracking
- Window blur/focus events
- Visibility change detection
- Tab switching patterns
- Screenshot attempt indicators
```

### Native Windows Agent

**Features:**
- Foreground process monitoring
- Application window tracking
- Unauthorized software detection
- Process termination (Task Manager, cmd, PowerShell)
- Periodic reporting to Flask server

**Usage:**
```powershell
python scripts/agent_win_monitor.py --attempt <ATTEMPT_ID> --token <TOKEN> --server http://localhost:25570
```

### Chrome Extension

**Capabilities:**
- Close non-exam tabs
- Display attempt tokens
- Quick copy functionality
- Tab guard enforcement

**Installation:**
1. Open Chrome → Extensions → Developer mode
2. Load unpacked → Select `exam_proctor_extension/`

---

## AI-Powered Detection

### Analysis Algorithm (`analyze_attempt_logs`)

```python
Detection Thresholds (Ultra-Sensitive):
├─ Fullscreen Exit: 1+ violations = CRITICAL
├─ Screenshot/Shortcut: 1+ attempts = HIGH  
├─ Tab Switching: 3+ switches = MEDIUM
├─ Window Blur: 5+ events = MEDIUM
└─ Window Hidden: 3+ events = MEDIUM
```

### Violation Categories

| Event | Severity | Description | Action |
|-------|----------|-------------|--------|
| `exam_portal_fullscreen_exit` | 🔴 CRITICAL | Student exited fullscreen | Auto-terminate |
| `exam_portal_shortcut_blocked` | 🟠 HIGH | Screenshot/cheating shortcuts | Flag for review |
| Tab switching pattern | 🟡 MEDIUM | Multiple blur→focus cycles | Alert teacher |
| `exam_portal_blur` | 🟡 MEDIUM | Lost window focus | Log event |
| `exam_portal_hidden` | 🟡 MEDIUM | Window minimized/hidden | Log event |

### AI Dashboard (`/exam/<id>/ai-alerts`)
- Real-time violation statistics
- Student-by-student breakdown
- Severity-based color coding
- Activity timelines
- One-click exam termination

---

## Real-Time Monitoring

### Socket.IO Implementation

**Server Events:**
```python
# Student joins exam room
socket.emit('join_attempt', {attempt_id: X})

# Activity broadcast
socket.emit('attempt_event', {
    attempt_id: X,
    record: {event, timestamp, data}
})

# Exam termination
socket.emit('exam_terminated', {
    attempt_id: X,
    message: "Terminated due to violations"
})
```

**Client Listeners:**
```javascript
// Student receives termination
socket.on('exam_terminated', function(data) {
    // Auto-save answers
    // Force finish attempt
    // Redirect with alert
});
```

### Live Monitoring Features
- `/teacher/attempt/<id>/live` — Single student monitoring
- `/staff/live_all` — All active attempts dashboard
- Event filtering and search
- CSV export functionality

---

## Native Monitoring Toolkit (Windows)

### GUI Launcher (`student_monitor_app.py`)

**Features:**
- Token validation interface
- Background process monitoring
- Automatic termination of:
  - Task Manager
  - Command Prompt
  - PowerShell
  - Other blacklisted processes
- Status indicators
- Clean shutdown

**Usage:**
```powershell
python student_monitor_app.py
```
1. Enter Attempt ID (from exam page)
2. Enter Agent Token (from exam page)
3. Click "Start Monitoring"
4. Minimize and take exam

### Command-Line Agent

**Requirements:**
```bash
pip install pywin32 psutil requests
```

**Reports:**
- Foreground window titles
- Active process names
- Application switches
- Timestamp for all events

---

## CI/CD Automation (GitHub Actions)

This repository now includes a workflow at `.github/workflows/ci-cd.yml` that:

- Runs on every push and pull request
- Installs Python dependencies
- Performs syntax checks on all tracked Python files
- Imports and initializes the Flask app/database to catch startup errors
- Performs a runtime smoke test by starting the app and checking `/login`

If all checks pass and the push is to `dev`, it automatically deploys using a self-hosted GitHub Actions runner on your VPS.

### Self-Hosted Runner Setup (VPS)

On your VPS, register a GitHub Actions runner for this repository:

```bash
# Create runner folder
mkdir -p /opt/actions-runner && cd /opt/actions-runner

# Download runner package from GitHub UI instructions for your repo
# Extract and configure with the provided URL/token
./config.sh --url https://github.com/hyperdargo/Asura-Anti-Cheat --token <RUNNER_TOKEN>

# Install and start as service
sudo ./svc.sh install
sudo ./svc.sh start
```

Use labels including `self-hosted` and `linux`.

### Required GitHub Repository Variables (for auto-deploy)

Add these in **GitHub Repository Settings → Secrets and variables → Actions → Variables**:

- `DEPLOY_APP_DIR` (absolute deployment directory on VPS, e.g. `/opt/asura`)

Optional:

- `DEPLOY_SERVICE_NAME` (systemd service name, default: `asura`)

### Deployment Behavior

On successful validation:

1. Syncs workflow checkout into `DEPLOY_APP_DIR`
2. Recreates/updates virtual environment
3. Installs `requirements.txt`
4. Restarts your systemd service (`asura` by default)

Make sure your production server already has:

- A registered self-hosted GitHub Actions runner for this repo
- Python 3 and `python3-venv` installed
- `rsync` installed
- A systemd unit for your app (or adjust the workflow command)
- `sudo` permissions for runner user to restart the service

## Configuration

### Environment Variables

```bash
# Flask Configuration
SECRET_KEY=your-secret-key-here  # CHANGE IN PRODUCTION!
DATABASE_URL=sqlite:///app.db    # Or MySQL/PostgreSQL URL

# Server Settings
HOST=0.0.0.0
PORT=25570

# MySQL/PostgreSQL (Optional)
MYSQL_HOST=localhost
MYSQL_USER=exam_user
MYSQL_PASSWORD=secure_password
MYSQL_DB=exam_portal
```

### Production Deployment

```bash
# Use production-grade WSGI server
pip install gunicorn eventlet

# Run with Gunicorn + Socket.IO
gunicorn --worker-class eventlet -w 1 --bind 0.0.0.0:25570 app:app

# Or with Gevent
gunicorn --worker-class gevent -w 1 --bind 0.0.0.0:25570 app:app
```

### Database Migration (SQLite → MySQL/PostgreSQL)

```python
# Update DATABASE_URL in environment
DATABASE_URL=mysql+pymysql://user:pass@host/db
# or
DATABASE_URL=postgresql://user:pass@host/db

# Run migrations
flask db init
flask db migrate -m "Initial migration"
flask db upgrade
```

---

## Security & Privacy

### ⚠️ **Important Considerations**

1. **User Consent** — Obtain explicit consent before:
   - Recording window/process names
   - Enabling native monitoring agents
   - Tracking browser activity

2. **Data Retention** — Define and enforce:
   - Event log retention policies
   - Personal data deletion schedules
   - GDPR/privacy law compliance

3. **HTTPS Required** — Always use HTTPS in production:
   - Protects authentication tokens
   - Secures Socket.IO connections
   - Encrypts sensitive data

4. **CSRF Protection** — Implement for all JSON endpoints:
   ```python
   from flask_wtf.csrf import CSRFProtect
   csrf = CSRFProtect(app)
   ```

5. **Rate Limiting** — Add to prevent abuse:
   ```python
   from flask_limiter import Limiter
   limiter = Limiter(app, key_func=get_remote_address)
   ```

6. **Limitations**
   - Browser-based enforcement can be bypassed with sufficient technical knowledge
   - OS-level screenshots cannot be completely prevented
   - Use in combination with live proctoring for high-stakes exams

---

## Tech Stack

### Backend
- **Flask 2.x** — Web framework
- **SQLAlchemy** — ORM for database operations
- **Flask-Login** — User authentication & session management
- **Flask-SocketIO 5.x** — Real-time WebSocket communication
- **SQLite** — Default database (MySQL/PostgreSQL supported)

### Frontend
- **Jinja2** — Server-side templating
- **Bootstrap 5.3.0** — UI framework with custom theme
- **Socket.IO Client 4.5.4** — Real-time client library
- **Vanilla JavaScript** — Browser enforcement logic

### Native Tools
- **Python 3.10+** — Core language
- **pywin32** — Windows API access
- **psutil** — Process monitoring
- **Tkinter** — GUI toolkit

### Monitoring
- **Socket.IO** — Real-time event streaming
- **JSON** — Event storage format
- **CSV Export** — Reporting capability

---

## Exam Termination Flow

```text
┌─────────────────────────────────────────────────────────────┐
│ 1. Teacher/Staff detects violations in AI Alerts Dashboard │
└────────────────────┬────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────┐
│ 2. Clicks "🚫 Terminate Exam" button                        │
└────────────────────┬────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────┐
│ 3. Server sets score to 0.00% and finished_at timestamp    │
└────────────────────┬────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────┐
│ 4. Server logs termination event with staff details        │
└────────────────────┬────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────┐
│ 5. Socket.IO broadcasts to student's browser               │
└────────────────────┬────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────┐
│ 6. Student's browser auto-saves answers                    │
└────────────────────┬────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────┐
│ 7. Force-finishes attempt and redirects student            │
└────────────────────┬────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────┐
│ 8. Alert shown: "Exam terminated due to suspicious activity"│
└────────────────────┬────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────┐
│ 9. Student sees 0.00% with ⚠️ TERMINATED badge in results │
└─────────────────────────────────────────────────────────────┘
```

---

## Contributing

Contributions are welcome! Please follow these guidelines:

### Pull Request Process
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/YourFeature`)
3. Make focused changes with clear commits
4. Add tests for new functionality
5. Update documentation (including this README)
6. Submit PR with detailed description

### Code Standards
- Follow PEP 8 for Python code
- Use meaningful variable/function names
- Add docstrings to functions
- Comment complex logic
- Maintain consistent indentation

### Database Changes
- Use Alembic for migrations
- Document schema changes
- Test migration up/down
- Preserve existing data

### Privacy First
- Respect user privacy in new features
- Document data collection clearly
- Obtain proper consent
- Comply with regulations (GDPR, FERPA, etc.)

---

## License

**Academic Use License**

This project is intended for educational and academic use. Please respect institutional policies regarding exam proctoring and student privacy.

---

## Credits

**Built by:** DTEmpire  
**Contact:** [ankitgupta.com.np](https://ankitgupta.com.np/)  
**Links:** [linktr.ee/dargotamber](https://linktr.ee/dargotamber)

---

## Troubleshooting

### Common Issues

**Issue:** Server won't start
```bash
# Check if port is in use
netstat -ano | findstr :25570
# Kill process if needed
taskkill /PID <process_id> /F
```

**Issue:** Socket.IO not connecting
```python
# Ensure eventlet/gevent is installed
pip install eventlet
# or
pip install gevent
```

**Issue:** Native agent fails
```bash
# Install Windows dependencies
pip install pywin32 psutil requests
```

**Issue:** Database locked error
```bash
# Stop all Python processes
# Delete app.db (WARNING: loses all data)
# Restart server to recreate database
```

---

## Roadmap

### Planned Features
- [ ] Webcam proctoring integration
- [ ] Machine learning-based behavior analysis
- [ ] Mobile app for proctors
- [ ] Multi-language support
- [ ] Face recognition for student verification
- [ ] Advanced analytics dashboard
- [ ] API for third-party integrations
- [ ] Docker containerization
- [ ] Kubernetes deployment configs

---

## Support

For issues, questions, or contributions:
- **GitHub Issues**: Report bugs and request features
- **Documentation**: Check this README and code comments
- **Contact**: See credits section above

---

**⚠️ Important Reminder:** Always change default credentials and SECRET_KEY before deploying to production!
