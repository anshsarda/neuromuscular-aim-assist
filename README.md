# neuromuscular-aim-assist
# Neuromuscular Aim Assist (Recreation)

Recreation of BasicallyHomeless’s “Neuromuscular Aim Assist” project — using a Raspberry Pi, relay control, and a consumer TENS unit to simulate neuromuscular feedback linked to computer-vision detection.

---

## ⚙️ Overview

This project demonstrates a closed control loop between **computer vision**, a **microcontroller**, and **muscle stimulation** hardware.  
A YOLO-based model detects in-game enemies and sends control signals to a Raspberry Pi.  
The Pi toggles relays connected to a TENS unit, which triggers mild muscle contractions to influence aim.

**Flow:**  
`Game frame → YOLO detection → Pi GPIO → Relay → TENS button → Forearm muscle`

---

## 🧰 Hardware

| Component | Example Model | Qty | Purpose |
|------------|---------------|-----|----------|
| Raspberry Pi 4 / 5 | — | 1 | Receives commands and toggles relays |
| 4-Channel 5 V Relay Board | Songle SRD-05VDC-SL-C | 1 | Isolates Pi logic from TENS buttons |
| TENS/EMS Unit | TENS 7000 / PowerDot / Compex Mini | 1 | Generates safe muscle stimulation |
| Gel Electrodes | 2″ pads | 4 – 8 | Deliver pulses to forearm muscles |
| Jumper Wires + Breadboard | — | 1 set | Connect Pi → Relay |
| Emergency Stop Switch | — | 1 | Kill-switch for stim power |
| Thin Hookup Wire | — | — | Tap onto TENS button contacts |

*Use only battery-powered, medically certified TENS units. Do **not** connect Pi GPIOs to electrode outputs.*

---

## 💻 Software

- **Python 3.10+**
- **Libraries:** `torch`, `opencv-python`, `ultralytics`, `RPi.GPIO`, `pyserial`
- **Game Capture:** OBS Studio / GPU API feed
- **OS:** Raspberry Pi OS Bookworm or later

### Install dependencies
```bash
pip install torch opencv-python ultralytics RPi.GPIO pyserial
📂 Directory Structure
markdown
Copy code
neuromuscular-aim-assist/
├── README.md
├── requirements.txt
├── vision_model/
│   └── detect_enemies.py
├── raspberry_pi/
│   └── relay_control.py
├── hardware/
│   ├── wiring_diagram.png
│   └── materials_list.md
└── safety/
    └── safe_use_guidelines.md
🔄 System Operation
Computer Vision (PC):

YOLO model detects enemy bounding boxes in real-time.

Calculates direction vector from crosshair to target.

Sends serial/TCP command to Raspberry Pi.

Raspberry Pi:

Listens for incoming command.

Activates GPIO pin linked to a relay channel.

Relay briefly shorts the corresponding TENS button circuit.

TENS Unit:

When button circuit closes, fires stimulation pulse.

Electrodes on flexor/extensor groups produce minor arm movement.

🧠 Electrode Placement (Example)
Muscle	Function	Location
Extensor Carpi Radialis	Move wrist left/right	Outer forearm
Flexor Carpi Radialis	Opposite direction	Inner forearm
Extensor Digitorum	Trigger finger extension	Mid forearm

⚡ Safety Notes
Use only commercial, battery-powered TENS devices.

Keep Pi and relay circuits isolated from stimulation outputs.

Always include a hardware kill switch or emergency power cutoff.

Do not stimulate across chest, head, or spine.

Begin with lowest intensity and short activations (< 0.5 s).

🧪 Testing
Run detect_enemies.py to verify YOLO detection.

Run relay_control.py on the Pi and watch relays toggle.

Connect TENS leads to dummy load first (not skin) to confirm timing.

Attach electrodes and test minimal stimulation.

Measure system latency (frame → pulse) and log data in /logs/.

🧩 Future Work
Replace relays with opto-isolated digital switches for faster triggering.

Integrate adaptive filtering to reduce false positives.

Explore closed-loop EMG sensing to modulate stimulation strength.

⚖️ Disclaimer
This project is for educational and experimental purposes only.
It should not be used on anyone without informed consent, and all electrical work must follow appropriate safety standards.

© 2025 <YOUR NAME> – Licensed under the MIT License
