
Ella: Africa's First Single-Chip Autonomous Health Robot
Ella is a low-cost, AI-powered healthcare assistant designed to bridge the gap in patient monitoring in Sub-Saharan Africa. Built on a single ESP32-S3 SoC, Ella provides a "Zero-App" interface for medical telemetry, autonomous ward navigation, and empathetic AI companionship.

🌍 The Mission: Solving the "Japa" Crisis
The mass emigration of medical professionals ("Japa") has left African clinics understaffed. Ella is engineered to:

Reduce Staff Burden: Automate routine vitals checks (Heart Rate, SpO2, Temperature).

Eliminate "App Fatigue": Remove the need for smartphones or app installations for elderly patients.

Push Microcontroller Limits: Prove that a sub-$40 single-chip solution can replace $500+ microprocessor-based systems.

🧠 Software Architecture
Ella’s firmware is built on FreeRTOS, utilizing a multi-threaded approach to handle real-time motor control alongside high-bandwidth AI audio streaming.

1. Hybrid AI Voice Pipeline
To ensure reliability during internet outages, Ella uses a dual-layered voice system:

Offline Reflex (SU-03T): A dedicated UART-based module for zero-latency safety commands (Stop, Forward, Manual Override).

Online Intelligence (Groq + Gemini): * STT: Uses whisper-large-v3-turbo via Groq LPU for <500ms transcription.

LLM: Google Gemini 2.0 Flash provides a "sassy best friend" persona to reduce patient anxiety.

TTS: Progressive chunked streaming via Google TTS for immediate audio feedback.

2. Medical Sensor State Machine (MAX30102)
Bio-sensing on a mobile robot is prone to noise. Ella implements a strict 4-state verification machine to ensure medical-grade accuracy:

IDLE: Polling for finger proximity.

STABILIZING: A 5-second countdown to ensure the patient is motionless.

SAMPLING: Capturing 100 raw samples into a circular buffer.

CALCULATING: Executing the maxim_heart_rate_and_oxygen_saturation algorithm with motion artifact rejection.

🛠️ Technical Deep Dive
PSRAM Memory Management
Standard ESP32-S3 SRAM (512KB) is insufficient for recording 5+ seconds of high-fidelity audio. We utilize the 8MB PSRAM via the ps_malloc allocator:

Recording Buffer: 16-bit, 16kHz MONO PCM data is stored directly in PSRAM to prevent stack overflows during Wi-Fi transmission.

Power & Electrical Engineering
Sudden motor stops caused Back-EMF spikes (>20V), which initially destroyed four development boards.

The Solution: Isolated power rails using a dedicated XL6009 Buck Converter for motors and 100µF bulk capacitors to absorb voltage transients, protecting the sensitive ESP32-S3 logic.

📂 Repository Structure
Plaintext

Ella-AI/
├── ella.ino               # Main firmware & FreeRTOS task management
├── VitalsAlgorithm.cpp     # MAX30102 signal processing
├── MotorControl.h          # Cinematic ramping & PWM logic
├── data/                   # Web dashboard files (HTML/JS/CSS)
├── docs/                   # Schematics & 3D Model (STL)
└── README.md
🔧 Setup & Installation
Hardware: ESP32-S3 (8MB PSRAM), MAX30102, SU-03T, INMP441, MAX98357A, DRV8833.

Environment: * Install ESP32-audioI2S

Install ArduinoJson

Flash: Upload the data/ folder via LittleFS Data Upload tool before flashing the firmware.

🔮 Roadmap
[ ] Local Dialect Support: Adding Yoruba, Hausa, and Igbo to the offline reflex system.

[ ] Autonomous Ward Patrol: Intelligent cinematic exploration logic.

Proudly Developed in Nigeria.
