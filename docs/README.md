# Building the Untethered, Always-On AI Companion

## Overview
A Python-based AI assistant that works fully **offline** on mobile (via UserLAnd Ubuntu) 
It supports **vision (face detection)** and **audio (voice interaction groundwork)** and can summarize **recent activities** it has *seen* (vision) and *heard* (audio).  
It exposes a quality **web UI (frontend)** powered by **Flask (backend)** where you can ask questions like:
- “What did you see/hear recently?”
- “Is every sensor working properly?”
- Ask any custom question via the **Ask AI Assistant** bar
- Use the **Reload Model** action if model loading encounters an issue

> **Key promise:** Privacy-first, untethered and offline-capable.

The part that makes this **unique** is its capability to run **offline** without any external cloud connection

---

## Tech Stack
**Language:** Python 3.10.11  

**Core Libraries (with download links) :** 
- [os](https://docs.python.org/3/library/os.html) *(built-in)*
- [json](https://docs.python.org/3/library/json.html) *(built-in)*
- [cv2 (OpenCV)](https://pypi.org/project/opencv-python/)
- [sounddevice](https://pypi.org/project/sounddevice/)
- [numpy](https://pypi.org/project/numpy/)
- [sqlite3](https://docs.python.org/3/library/sqlite3.html) *(built-in)*
- [threading](https://docs.python.org/3/library/threading.html) *(built-in)*
- [time](https://docs.python.org/3/library/time.html) *(built-in)*
- [datetime](https://docs.python.org/3/library/datetime.html) *(built-in)*
- [typing](https://docs.python.org/3/library/typing.html) *(built-in)*
- [flask](https://pypi.org/project/Flask/)
- [socket](https://docs.python.org/3/library/socket.html) *(built-in)*
- [sys](https://docs.python.org/3/library/sys.html) *(built-in)*

**OS/Env:** UserLAnd (Ubuntu) on Android

> **No database** is used. All state for “recent activity” is kept in memory/logs.

---

## High-Level Technical Architecture 
```
[User] ⇄ [Web UI (Frontend, Flask templates/JS)]
                 ↓
            [Flask Backend]
        ┌─────────┴─────────┐
        │                   │
 [Audio Module]       [Vision Module]
 (sounddevice)          (OpenCV)
   capture               camera
   events →           detections →
         \            /
           [In-Memory Activity Log]
                    ↓
              [Summary/Queries]
```
- **Concurrency:** Python `threading` runs audio and vision loops in parallel.
- **Comms:** Modules communicate via in-memory structures / queues and Flask routes.
- **Offline:** Models and logic run locally; no external calls required.

---

## Implementation Details
The AI Companion is implemented as a modular **Python** application combining computer vision, audio processing, memory, context reasoning, and a Flask-based UI. 
The **VisionModule** uses OpenCV to capture frames from the device’s camera and detect faces while the **AudioModule** leverages the SoundDevice library to capture microphone input and detect speech. Both modules run in parallel threads and feed their outputs into a **MemoryModule** (SQLite) that logs events with timestamps. 
A **ContextModule** summarizes the current state (e.g., person seen, speech detected) and provides real-time context. 
The **ReasoningModule** loads a rule-based engine which can be hot-reloaded via the UI if any problem occurs. 
A **Flask web server** hosts the dashboard, providing a live camera feed, system status, logs, and an interactive Ask AI Assistant bar where users can query recent activities, sensor status, or request summaries.

---

## Installation Instructions for Android
- Download UserLAnd.
- Open Ubuntu and then its terminal.
- Download all the mentioned libraries using the links in the Tech Stack section.
- Download **IP WEBCAM** app from Playstore for accessing Microphone and Camera.
- Download Git and then clone the code from github to ubuntu terminal and run the code.

---

## User Guide
1. Once all the installation steps have been performed user has to run the code.
2. Once the code starts running, user has to go to Chrome and fill this URL - 
http://127.0.0.1:5000, this will open the UI and the model will start to load automatically.
3. User can interact with the model via the **Ask AI Assistant** bar:
   - “Who was seen recently?”
   - “What did you hear recently?”
   - “Is every sensor working properly?”
4. If the model fails to load or misbehaves, click **Reload Model** in the UI.

---

## Salient Features
- 🎙️ **Voice Recognition (foundation)** – microphone capture & processing; STT planned
- 👁️ **Vision: Face Detection** – detects people via camera using OpenCV
- 🧠 **Recent Activity Summaries** – recalls what was seen/heard earlier
- 🔁 **Reload Model** – one-click recovery if model fails to load
- 🧪 **Sensor Check** – “Are all sensors OK?” query via UI
- 🌐 **Frontend + Backend** – Flask backend with a browser-based UI
- 📳 **Runs Fully Offline** – designed for mobile devices via UserLAnd Ubuntu

--- 

## Plans for Future Improvements
- 🎤 **Speech-to-Text (STT)** for voice input
- 🔊 **Text-to-Speech (TTS)** for spoken responses
- 🧩 **Richer AI Models** (beyond rule-based) for much better accuracy
- 📱 **Native Mobile App** for better UX and scalability
- 🧠 **Object Detection** in the camera module (detect general objects)
- 👤 **Personalization & UX** improvements
