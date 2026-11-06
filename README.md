
![ai_ponybot_small_size](https://github.com/user-attachments/assets/43f8077b-dd00-4539-9528-519e3ed12086)


# AI Ponybot Extension for micro:bit  

This MakeCode extension provides a set of programmable blocks for controlling the **AI Ponybot**, a micro:bit-based smart robot developed by **boundaryX**.  

---

## 🚀 Features 
- Control up to 4 DC motors and 8 servo motors via PCA9685  
- Easy directional control using Mecanum wheel or basic movement commands
- Read line tracking sensor values (left & right)  
- Measure distance using ultrasonic sensor  
- Read RGB and brightness values from TCS34725 color sensor  
- Display text, numbers, and shapes on 128x64 OLED screen  
- Detect color and compare RGB values using advanced logic  

---

## 🧩 Blocks Overview 

### 🔧 Motor Control 
 - runMotor, stopMotor, stopAllMotors : DC motor control
 - runMecanum, runNormal : directional movement control
 - servo : set servo angle

### 📏 Sensor Input
 - readLineSensor, checkTwoLineState, checkSingleLineSensor : line sensor input
 - ping — distance measurement (cm, inch, µs)
 - getRed, getGreen, getBlue, getLight : read RGB and brightness
 - isColorAdvanced, isColorInRange : color detection logic

### 🖥 OLED Display
 - display, clear, invert : display control
 - showString, showNumber, printString, printNumber : text & number output
 - pixel, horizontalLine, verticalLine, rectangle : draw pixels & shapes

---

## 🧪 Basic Usage Example
```ts
aiPonybot.servo(aiPonybot.Servo.Servo1, 90)
aiPonybot.runMotor(aiPonybot.Motor.Motor1, aiPonybot.Direction.Clockwise, 150)
aiPonybot.stopMotor(aiPonybot.Motor.Motor1)

let dist = aiPonybot.ping(aiPonybot.PingUnit.Centimeters)
basic.showNumber(dist)

aiPonybot.display(true)
aiPonybot.showString("Hello!", 0, 0)
```

---

## 🎨 Color Sensor Modes (TCS34725)

Use the existing V1/V2 module calibration before reading colors:

```typescript
aiPonybot.initColorSensor(aiPonybot.SensorVersion.V2) // purple PCB
aiPonybot.setColorSensorMode(aiPonybot.ColorSensorMode.Fast) // optional
if (aiPonybot.isColorSensorConnected()) {
    const red = aiPonybot.getRed()
}
```

- **Sensitive (default):** 153.6 ms, 60× gain — legacy V1/V2 profile calibration baseline, most sensitive.
- **Balanced:** 48 ms, 16× gain — less latency and gain.
- **Fast:** 24 ms, 16× gain — suitable for experiments with moving color cards (higher noise risk).
- RGB getters still return **0–255** normalized against the V1/V2 baseline; optional modes use an exposure-ratio approximation, so **recalibration with actual paper, lighting, and sensor distance is recommended**.
- `getLight()` reports the **raw Clear channel**, which changes with integration time and gain; it is not lux.
- `isColorAdvanced(color, threshold)` now uses **threshold as required RGB-channel separation (10–100% of the strongest channel)**. Higher values mean stricter classification and may reject weak colors. Existing 40% default can give different results from previous releases; re-test classroom materials.
- The sensor checks chip ID (0x44/0x4D), waits for oscillator startup, checks AVALID, reads RGBC in one I²C transfer, and reuses a 20-ms snapshot for successive getters.
- A missing chip gives zero RGB/clear; `isColorSensorConnected()` distinguishes disconnected hardware. The driver returns the latest valid sample on temporary read failure.
- With no individual black/white Clear-value calibration, black and white are **not** recognized as dedicated colors by `isColorAdvanced`; use `getLight()` and measure thresholds under your exact setup.
- **Hardware test needed:** verify I²C communication, colors, sensor height, and motor-operation interference on real Ponybot before release.

---

## 📦 Installation

In MakeCode editor, click **Extensions** → search for:  
https://github.com/boundary-x/ai_ponybot_basic

---

## 🖼 Example Blocks
![block_example](https://github.com/user-attachments/assets/f5e1ab2a-4f05-4769-8c8b-b8e0e3a078e8)

---

## License
  - MIT License
  (c) 2025 boundaryX
  - This extension is based on xinabox/pxt-OD01 and is licensed under the MIT License
  - This extension is based on gbantique/brickcell-color-tcs34725 and is licensed under the MIT License
  - This extension is based on microsoft/pxt-sonar and is licensed under the MIT License
  - Many thanks to the developers for their work on this extension.

--- 

## 💡 Developed By
- boundaryX – Future-focused EdTech company providing hardware + AI education kits
- Website: https://boundaryx.io
- product webpage : https://boundaryx.io/shop/?idx=26
- E-mail : hi@boundaryx.io

