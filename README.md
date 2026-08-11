
# 📡 BLE Bearing Finder

A web-based "radar" for locating Bluetooth Low Energy (BLE) devices. Unlike standard BLE scanners that only 
show signal strength, this project estimates both the **distance** and the **directional bearing** of devices 
by leveraging the "body shadowing" effect.

## 🚀 Features

- **Bearing Estimation**: Uses the signal attenuation caused by your own body as you rotate to determine the 
direction of the device.
- **Distance Estimation**: Calculates approximate distance based on the Log-Distance Path Loss model.
- **Visual Radar**: A dynamic canvas dial that shows devices as "blips" with confidence arcs.
- **Audio Sonar**: 
  - **Frequency**: Beeps faster as you get closer to the target.
  - **Stereo Panning**: Pans the sound left/right based on the device's bearing (requires headphones).
- **Compass Integration**: Aligns the radar with magnetic north using the device's magnetometer.
- **Demo Mode**: Simulate a search environment without needing actual BLE hardware.
- **Real-time Calibration**: Adjust Path Loss exponents and TX power to match your environment.

## 🛠️ How it Works

### 1. Direction (Bearing)
The app uses **Body Shadowing**. Since the human body is mostly water, it absorbs 2.4GHz radio signals. By 
rotating slowly on the spot, the app tracks RSSI (signal strength) across 36 buckets (10° each). The point of 
maximum signal strength typically indicates the direction of the device.

### 2. Distance
Distance is estimated using the formula:
$RSSI = TxPower - 10 \cdot n \cdot \log_{10}(distance)$
Where:
- **TxPower**: The signal strength at 1 meter.
- **n**: The path-loss exponent (e.g., 2.0 for open space, 3.0+ for cluttered rooms).

## ⚠️ Requirements & Compatibility

Because this project uses the **Web Bluetooth API** and **Device Orientation API**, there are strict browser 
requirements:

### 🌐 Browser Support
| Platform | Recommended Browser | Note |
| :--- | :--- | :--- |
| **Android** | Chrome / Edge | Full Support |
| **macOS / Windows** | Chrome / Edge | Requires Experimental Flags (see below) |
| **iOS (iPhone/iPad)** | [Bluefy Browser](https://www.bluefy.app/) | Standard Safari does not support Web 
Bluetooth |

### 🔐 Critical Setup
1. **HTTPS is Required**: Web Bluetooth and Compass APIs will not work over HTTP. You must serve this file via 
HTTPS or `localhost`.
2. **Chrome Experimental Flags**: For continuous scanning on desktop, enable:
   `chrome://flags/#enable-experimental-web-platform-features`
   *(Restart the browser after enabling)*.
3. **Permissions**: You must grant permission for both **Bluetooth** and **Motion/Orientation** when prompted.

## 📖 Usage Guide

1. **Start the App**: Open the page in a supported browser via HTTPS.
2. **Enable Compass**: Click "Enable Compass" and hold your phone flat.
3. **Scan**: Click "Start Scan".
4. **Locate**: 
   - Select a device from the list.
   - **Turn slowly in a full circle** on the spot.
   - Watch the "confidence arc" on the dial narrow down as the app gathers more data.
5. **Follow the Sound**: Enable sound and use headphones. The beep will speed up and pan toward the target.
6. **Calibrate**: If the distance seems wrong, stand exactly 1 meter from your device and adjust the "Power @ 
1m" slider until the reading shows `1.0m`.

## 📁 Project Structure
- **HTML**: Defines the UI layout and the radar canvas.
- **CSS**: A custom "dark-mode" industrial theme using CSS variables.
- **JavaScript**:
    - `estimateBearing()`: The core logic for analyzing signal buckets and calculating the vector.
    - `ingest()`: Handles incoming BLE advertisement packets.
    - `draw()`: The Canvas API loop that renders the radar and range rings.
    - `playBeep()`: Web Audio API implementation for the sonar effect.

## ⚖️ License
MIT License. Feel free to use and modify for your own projects!
