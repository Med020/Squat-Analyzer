# 🏋️ Squat Analyzer with YOLO v8

A real-time squat form analyzer using YOLO v8 Pose Detection and React. Analyze your squat technique with precise angle measurements, symmetry analysis, and instant feedback.

![Version](https://img.shields.io/badge/version-4.0-blue)
![Python](https://img.shields.io/badge/python-3.8+-green)
![React](https://img.shields.io/badge/react-18.0+-blue)
![License](https://img.shields.io/badge/license-MIT-orange)

---

## ✨ Features

- 🎯 **Three Squat Types**: Partial, Medium, and Deep squat analysis
- 📐 **Precise Angle Tracking**: Real-time knee angle measurements (left & right)
- ⚖️ **Symmetry Analysis**: Detect and visualize balance issues
- 📹 **Live Video Processing**: Frame-by-frame analysis with skeleton overlay
- 💬 **Smart Feedback**: Color-coded feedback (Green/Orange/Red)
- 📊 **Performance Metrics**: Speed, depth, and form tracking
- 🚀 **YOLO v8 Powered**: State-of-the-art pose detection

---

## 📁 Project Structure

```
squat-analyzer/
│
├── backend/
│   ├── app.py                 # FastAPI backend with YOLO
│   ├── requirements.txt       # Python dependencies
│   └── yolov8n-pose.pt       # YOLO model (auto-downloaded)
│
├── frontend/
│   ├── public/
│   │   └── index.html
│   ├── src/
│   │   ├── App.jsx           # Main React component
│   │   ├── index.js          # React entry point
│   │   └── index.css         # Tailwind CSS
│   ├── package.json
│   └── tailwind.config.js
│
└── README.md
```

---

## 🚀 Quick Start

### Prerequisites

- Python 3.8 or higher
- Node.js 14 or higher
- npm or yarn
- Webcam or video file

### Installation

#### 1️⃣ Clone the Repository

```bash
git clone https://github.com/yourusername/squat-analyzer.git
cd squat-analyzer
```

#### 2️⃣ Backend Setup

```bash
cd backend

# Create virtual environment (recommended)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Run the server
python app.py
```

The backend will start on `http://localhost:8000`

**Note**: First run will automatically download the YOLO v8 model (~6MB)

#### 3️⃣ Frontend Setup

Open a new terminal:

```bash
cd frontend

# Install dependencies
npm install

# Start development server
npm start
```

The frontend will open at `http://localhost:3000`

---

## 📦 Dependencies

### Backend (Python)

```
fastapi==0.104.1
uvicorn[standard]==0.24.0
python-multipart==0.0.6
ultralytics==8.0.220
opencv-python==4.8.1.78
numpy==1.24.3
```

### Frontend (Node.js)

```
react: ^18.2.0
lucide-react: ^0.263.1
tailwindcss: ^3.3.0
```

---

## 🎮 How to Use

1. **Select Squat Type**
   - Partial (90°–150°): Beginner-friendly, minimal flexion
   - Medium (70°–90°): Standard parallel squat
   - Deep (45°–70°): Full range, advanced

2. **Upload Video**
   - Click the upload area
   - Select your squat video (MP4, AVI, MOV)
   - Recommended: Side-view, full body visible

3. **Start Analysis**
   - Click "Démarrer l'analyse"
   - Watch real-time processing with skeleton overlay
   - View metrics update in real-time

4. **Review Results**
   - Check minimum depth reached
   - Verify left/right knee symmetry
   - Read feedback and recommendations

---

## 📊 Understanding the Metrics

### Angle Measurements

| Angle | Position | Description |
|-------|----------|-------------|
| 180° | Standing | Legs fully extended |
| 90° | Partial | Quarter squat |
| 70° | Medium | Parallel squat |
| 45° | Deep | Full depth squat |

### Squat Type Thresholds

| Type | Target Range | Fitness Level |
|------|-------------|---------------|
| Partial | 90°–150° | Beginner |
| Medium | 70°–90° | Intermediate |
| Deep | 45°–70° | Advanced |

### Symmetry Analysis

- **0°–10°**: Perfect balance ✅
- **10°–15°**: Minor imbalance ⚠️
- **15°+**: Significant imbalance ❌

### Feedback Colors

- 🟢 **Green**: Perfect form for selected type
- 🟠 **Orange**: Minor issues or warnings
- 🔴 **Red**: Form correction needed

---

## 🔧 Configuration

### Backend Configuration

Edit `backend/app.py`:

```python
# Change processing speed (line ~170)
time.sleep(0.05)  # Lower = faster, Higher = slower

# Change confidence threshold (line ~78)
results = model(frame, conf=0.3, verbose=False)  # 0.3 = 30% confidence

# Change CORS origins (line ~22)
allow_origins=["*"]  # For production, specify domains
```

### Frontend Configuration

Edit `frontend/src/App.jsx`:

```javascript
// Change API URL (line 24)
const API_URL = 'http://localhost:8000/api';

// Change polling rate (line 53)
pollInterval.current = setInterval(async () => {
    // ...
}, 50); // 50ms = 20 polls/second
```

---

## 🎯 API Endpoints

### Base URL: `http://localhost:8000`

| Endpoint | Method | Description | Request Body |
|----------|--------|-------------|--------------|
| `/` | GET | API information | - |
| `/health` | GET | Health check | - |
| `/api/upload` | POST | Upload & start analysis | `video`, `squat_type` |
| `/api/status` | GET | Get current metrics | - |
| `/api/stop` | POST | Stop analysis | - |
| `/api/reset` | POST | Reset metrics | - |
| `/docs` | GET | Interactive API docs | - |

### Example API Usage

```bash
# Health check
curl http://localhost:8000/health

# Upload video (using curl)
curl -X POST http://localhost:8000/api/upload \
  -F "video=@squat_video.mp4" \
  -F "squat_type=medium"

# Get status
curl http://localhost:8000/api/status

# Stop analysis
curl -X POST http://localhost:8000/api/stop

# Reset
curl -X POST http://localhost:8000/api/reset
```

---

## 📹 Video Requirements

### Recommended Specifications

- **Format**: MP4, AVI, MOV
- **Resolution**: 720p or higher
- **Duration**: 5-30 seconds per rep
- **FPS**: 30 fps or higher
- **View**: Side profile (perpendicular to body)
- **Framing**: Full body visible from head to feet
- **Background**: Clear, uncluttered
- **Lighting**: Bright, even lighting

### Tips for Best Results

✅ **DO:**
- Film from the side (90° angle to body)
- Ensure full body is in frame
- Use good lighting
- Wear fitted clothing
- Have clear background
- Keep camera stable

❌ **DON'T:**
- Film from front or back only
- Cut off body parts in frame
- Use dim or backlit lighting
- Wear very baggy clothing
- Have cluttered background
- Move the camera during recording

---

## 🐛 Troubleshooting

### Backend Issues

**Problem**: `ModuleNotFoundError: No module named 'ultralytics'`

**Solution**:
```bash
pip install ultralytics
```

**Problem**: YOLO model not downloading

**Solution**:
```bash
python -c "from ultralytics import YOLO; YOLO('yolov8n-pose.pt')"
```

**Problem**: Port 8000 already in use

**Solution**:
```bash
# Kill process on port 8000
# Windows
netstat -ano | findstr :8000
taskkill /PID <PID> /F

# Linux/Mac
lsof -ti:8000 | xargs kill -9

# Or change port in app.py
uvicorn.run(app, host="0.0.0.0", port=8001)
```

### Frontend Issues

**Problem**: CORS errors in browser console

**Solution**: Make sure backend is running and has CORS enabled (already configured)

**Problem**: Video not showing during analysis

**Solution**: 
- Check browser console for errors
- Verify backend is processing frames (check backend console)
- Try with a shorter video first
- Check network tab to see if frames are being sent

**Problem**: `npm install` fails

**Solution**:
```bash
# Clear cache and reinstall
npm cache clean --force
rm -rf node_modules package-lock.json
npm install
```

### General Issues

**Problem**: Analysis too slow

**Solution**:
- Use lower resolution video
- Increase `time.sleep()` in backend
- Process every Nth frame instead of all frames

**Problem**: Inaccurate angle detection

**Solution**:
- Improve video quality and lighting
- Ensure full body is visible
- Try side profile view
- Increase YOLO confidence threshold

---

## 🎨 Customization

### Change Squat Type Thresholds

Edit `backend/app.py`:

```python
THRESHOLDS = {
    "partial": {"min": 90, "max": 150, "name": "Partial"},
    "medium": {"min": 70, "max": 90, "name": "Medium"},
    "deep": {"min": 45, "max": 70, "name": "Deep"}
}
```

### Add New Squat Type

**Backend** (`app.py`):
```python
THRESHOLDS = {
    # ...existing types...
    "custom": {"min": 60, "max": 80, "name": "Custom"}
}
```

**Frontend** (`App.jsx`):
```javascript
const squatTypes = [
    // ...existing types...
    {
        id: 'custom',
        name: 'Custom Type',
        range: '60°–80°',
        description: 'Your custom description',
    }
];
```

### Modify UI Theme

Edit `frontend/src/App.jsx` and change Tailwind classes:

```javascript
// Change background gradient
<div className="min-h-screen bg-gradient-to-br from-purple-900 via-blue-800 to-purple-900">

// Change card colors
<div className="bg-blue-800/50 backdrop-blur-sm rounded-2xl p-6 border border-blue-700">
```

---

## 📈 Performance Optimization

### Backend Optimization

```python
# Process every Nth frame
frame_count = 0
while cap.isOpened():
    ret, frame = cap.read()
    frame_count += 1
    
    # Process every 2nd frame
    if frame_count % 2 != 0:
        continue
    
    # ... rest of processing
```

### Frontend Optimization

```javascript
// Reduce polling frequency
setInterval(async () => {
    // ...
}, 100); // Change from 50ms to 100ms
```

---

## 🔐 Security Considerations

⚠️ **Current setup is for development only!**

For production deployment:

1. **Add Authentication**: Implement user authentication
2. **Limit File Uploads**: Add file size limits
3. **Validate Input**: Sanitize all user inputs
4. **Use HTTPS**: Enable SSL/TLS
5. **Rate Limiting**: Prevent API abuse
6. **Environment Variables**: Store sensitive configs
7. **CORS**: Restrict origins to your domain

---

## 🚀 Deployment

### Backend (Docker)

```dockerfile
FROM python:3.9-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY app.py .
CMD ["python", "app.py"]
```

### Frontend (Vercel/Netlify)

```bash
# Build for production
npm run build

# Deploy (example with Vercel)
vercel deploy
```

### Full Stack (Docker Compose)

```yaml
version: '3.8'
services:
  backend:
    build: ./backend
    ports:
      - "8000:8000"
  
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    depends_on:
      - backend
```

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

---

## 🙏 Acknowledgments

- **YOLO v8** by Ultralytics - Pose detection model
- **FastAPI** - Modern Python web framework
- **React** - Frontend framework
- **Tailwind CSS** - Utility-first CSS framework
- **Lucide Icons** - Beautiful icon library

---

## 📧 Contact

- **Project Maintainer**: Your Name
- **Email**: your.email@example.com
- **GitHub**: [@yourusername](https://github.com/yourusername)

---

## 🗺️ Roadmap

- [ ] Add webcam live analysis
- [ ] Export analysis reports (PDF/CSV)
- [ ] Multiple person detection
- [ ] Exercise database integration
- [ ] Mobile app (React Native)
- [ ] Progress tracking over time
- [ ] AI-powered form recommendations
- [ ] Social sharing features

---

## 📝 Changelog

### Version 4.0 (Current)
- ✨ Migrated from MediaPipe to YOLO v8
- ✨ Added individual knee tracking (left/right)
- ✨ Implemented symmetry analysis
- ✨ Enhanced feedback system with color coding
- ✨ Improved UI with more detailed metrics
- 🐛 Fixed frame streaming issues
- 🐛 Improved processing speed

### Version 3.0
- ✨ Added FastAPI backend
- ✨ Real-time video processing
- ✨ React frontend implementation

### Version 2.0
- ✨ MediaPipe integration
- ✨ Basic angle calculation

### Version 1.0
- 🎉 Initial release
- ✨ Basic squat detection

---

## ⭐ Star History

If you find this project helpful, please consider giving it a star! ⭐

---

**Made with ❤️ for fitness enthusiasts and developers**
