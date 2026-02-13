# CrowdShield

A real-time safety monitoring system utilizing AI to detect and verify critical incidents (Fire, Violence, Stampede) from camera feeds. CrowdShield combines edge AI detection with intelligent verification and instant notifications to enable rapid emergency response.

## 🎯 Features

- **Real-time Detection**: YOLOv8-based edge detection for fire, violence (fights), weapon usage, and crowd density
- **AI Verification**: Google Gemini-powered intelligent verification to reduce false positives
- **Live Streaming**: Low-latency WebSocket-based video streaming to the dashboard
- **Instant Alerts**: Automated WhatsApp notifications to security teams
- **Web Dashboard**: Next.js-based monitoring interface for incident review and approval
- **Microservices Architecture**: Scalable, distributed system design
- **Database Persistence**: SQLite for incident logging and Firebase for real-time updates

## 🏗️ Architecture Overview

CrowdShield is built on a microservices architecture with the following components:

```
📹 Camera/OBS
    ↓
🧠 Vision Model (YOLOv8) - Edge Detection
    ├→ 🎬 Livestream Service (WebSocket) → Browser
    └→ 🤖 Agent Service (Gemini Verification)
         ↓
    📊 Session Service (Backend API & Database)
         ├→ 💬 Messenger Service (WhatsApp)
         └→ 🔥 Firebase (Real-time Updates)
              ↓
    🖥️ Frontend Dashboard (Next.js)
```

### Core Components

| Component | Port | Technology | Purpose |
|-----------|------|-----------|---------|
| **Vision Model** | N/A | Python, OpenCV, YOLOv8 | Real-time video processing and incident detection |
| **Agent Service** | 8001 | FastAPI, Google Gemini | Intelligent verification of detected incidents |
| **Session Service** | 8002 | FastAPI, SQLite | Central backend for session management |
| **Livestream Service** | 8000 | FastAPI, WebSocket | Low-latency video streaming to frontend |
| **Messenger Service** | 8003 | FastAPI, Playwright | WhatsApp Web automation for notifications |
| **Frontend** | 3000 | Next.js, TailwindCSS | Web dashboard for monitoring and control |

## 📋 Project Structure

```
CrowdShield/
├── backend/                      # Backend microservices
│   ├── session/                 # Session management service
│   ├── livestream/              # Video streaming service
│   └── messenger/               # WhatsApp notification service
├── model/                        # AI models and services
│   ├── vision-model/            # YOLOv8-based detection
│   │   ├── crowd_detection/     # Crowd density detection
│   │   ├── fire_detection/      # Fire detection model
│   │   ├── fight_detection/     # Violence/fight detection
│   │   └── weapon_detection/    # Weapon detection model
│   └── agent/                   # Gemini verification agent
├── frontend/                     # Next.js dashboard
│   └── app/                      # React components and pages
├── ARCHITECTURE.md              # Detailed technical architecture
└── README.md                    # This file
```

## 🚀 Quick Start

### Prerequisites

- Python 3.8+
- Node.js 16+ and npm
- ffmpeg
- Google Gemini API key
- WhatsApp Web (for messenger service)
- Webcam or OBS streaming setup

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/akhileshsiva1212-gif/CrowdShield.git
   cd CrowdShield
   ```

2. **Set up the Vision Model**
   ```bash
   cd model/vision-model
   pip install -r requirements.txt
   ```

3. **Set up the Agent Service**
   ```bash
   cd model/agent
   pip install -r requirements.txt
   # Create .env file with GEMINI_API_KEY
   ```

4. **Set up Backend Services**
   ```bash
   # Session Service
   cd backend/session
   pip install -r requirements.txt
   
   # Livestream Service
   cd ../livestream
   pip install -r requirements.txt
   
   # Messenger Service
   cd ../messenger
   pip install -r requirements.txt
   ```

5. **Set up Frontend**
   ```bash
   cd frontend
   npm install
   ```

### Running the System

Start each service in a separate terminal:

**Terminal 1: Vision Model**
```bash
cd model/vision-model
python main.py
```

**Terminal 2: Agent Service**
```bash
cd model/agent
python main.py
```

**Terminal 3: Session Service**
```bash
cd backend/session
python main.py
```

**Terminal 4: Livestream Service**
```bash
cd backend/livestream
python main.py
```

**Terminal 5: Messenger Service**
```bash
cd backend/messenger
python main.py
```

**Terminal 6: Frontend Dashboard**
```bash
cd frontend
npm run dev
```

Then open your browser to: **http://localhost:3000**

### Windows Users
Use the provided PowerShell script to bootstrap services:
```powershell
.\start_missing_services.ps1
```

## 🔄 Workflow

1. **Detection**: Vision Model captures video and detects incidents (Fire, Violence, Weapons, Crowd Density)
2. **Recording**: On detection, a 10-second clip is recorded and visual alerts are sent to livestream
3. **Verification**: Recorded clip is sent to Agent Service which uses Google Gemini to verify the incident
4. **Session Creation**: Verified incidents create a "Pending" session in the database
5. **Dashboard Alert**: Frontend displays red alert with incident details and recorded clip
6. **Review & Action**: Security officer reviews and clicks "Approve" or "Reject"
7. **Notification**: Upon approval, WhatsApp alert is sent to security team and Firebase updates physical alarms

## ⚙️ Configuration

### Environment Variables

Create `.env` files in:
- `model/agent/` - Add `GEMINI_API_KEY`
- `backend/session/` - Database configuration
- `backend/messenger/` - WhatsApp settings
- `backend/livestream/` - Streaming configuration

### Camera Configuration

Edit `model/vision-model/main.py` to:
- Change camera source (default: 1 for OBS, fallback to 0)
- Adjust detection thresholds
- Modify recording duration (default: 10 seconds)

## 📊 API Endpoints

### Session Service (`http://localhost:8002`)
- `GET /sessions` - List all sessions
- `POST /upload` - Create new session
- `GET /session/{id}` - Get session details
- `POST /session/{id}/approve` - Approve incident
- `POST /session/{id}/reject` - Reject incident

### Livestream Service (`http://localhost:8000`)
- `WebSocket /ws/push/{camera_id}` - Receive frames from Vision Model
- `GET /video_feed/{camera_id}` - MJPEG video stream

### Agent Service (`http://localhost:8001`)
- `POST /verify` - Verify incident with Gemini

## 🗄️ Database

- **SQLite** (`crowd_shield.db`) - Local incident storage and session management
- **Firebase Realtime DB** - Real-time updates and physical alarm triggers

## 🤝 Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit your changes (`git commit -am 'Add improvement'`)
4. Push to the branch (`git push origin feature/improvement`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 👥 Authors

- **Akhilesh Siva** - Creator and Lead Developer

## 🙏 Acknowledgments

- YOLOv8 by Ultralytics
- Google Gemini API
- FastAPI framework
- Next.js and React communities
- Playwright for automation

## 📞 Support

For issues, questions, or suggestions, please open an issue on GitHub or contact the development team.

---

**Stay Safe with CrowdShield** 🛡️