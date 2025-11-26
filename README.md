

# CleverCoffee Display Panel (ESPHome)

This project provides a standalone **hardware control & display panel** for the  
[CleverCoffee PID project](https://github.com/rancilio-pid/clevercoffee).

It enables you to adjust key brewing parameters **directly at the machine** using:

- An **ESP32**
- A **SH1106 128×64 OLED display**
- An **EC11 rotary encoder** (with push button)
- Two additional hardware buttons
- An **MQTT interface** to read sensor data and update setpoints

The panel displays real-time temperature, timing and weight data and allows changing all important brewing parameters without using the machine's web interface.

---

## ✨ Features

### Display
- Live brew temperature
- Brew setpoint temperature
- Steam temperature setpoint
- Brew time (target)
- Preinfusion time & pause
- Brew weight target  
- Backflush status

### Controls
- Rotary encoder for navigation + editing
- Encoder button for selection & confirm
- Button A → next menu page
- Button B → previous page / cancel editing
- Ability to:
  - Adjust brew temperature  
  - Adjust steam temperature  
  - Adjust brew time  
  - Adjust preinfusion & pause  
  - Adjust target brew weight  
  - Trigger scale tare  
  - Toggle backflush mode

### Smart behaviour
- Auto-sleep after inactivity (5 minutes)
- Wake on first interaction
- “Machine offline” screen if the Rancilio Silvia is not online
- “No MQTT connection” fallback screen
- No updates are applied while editing values
- MQTT updates are ignored while the screen is asleep

---

## 🧩 Requirements

**Hardware**
- ESP32 (ESP32-DevKit, NodeMCU-ESP32, etc.)
- SH1106 OLED display (128×64, I²C)
- EC11 rotary encoder with push button
- Two momentary push buttons
- Basic wiring for buttons, encoder and I²C

**Software**
- ESPHome (installation guide below)
- MQTT broker (e.g. Mosquitto, Home Assistant MQTT Addon)
- CleverCoffee firmware running on your machine

---

## 🚀 Installation & Setup

### 1. Install ESPHome
Follow the official ESPHome installation instructions:

👉 **https://esphome.io/**

You can install it via:

- **Home Assistant Addon**
- **Docker**
- **Pip / Python³**
- **ESPHome Dashboard app**

### 2. Clone this repository

```bash
git clone https://github.com/<your-username>/clevercoffee-display.git
cd clevercoffee-display
```

### 3. Add your `secrets.yaml`

Create a `secrets.yaml` file (not stored in Git) with:

```yaml
wifi_ssid_iot: "YOUR_WIFI"
wifi_password_iot: "YOUR_WIFI_PASSWORD"

mqtt_broker: "192.168.1.xxx"
mqtt_username: "YOUR_MQTT_USER"
mqtt_password: "YOUR_MQTT_PASSWORD"

mqtt_base: "custom/Silvia.silvia"
ota_password: "YOUR_OTA_PW"
```

### 4. Flash the ESP32

```bash
esphome run coffeeDisplay.yaml
```

Follow on-screen instructions to connect and upload firmware.

---

## 📡 MQTT Topics Used

| Function / Value          | Topic                                 |
|---------------------------|----------------------------------------|
| Brew temp setpoint        | `custom/Silvia.silvia/brewSetpoint`     |
| Steam temp setpoint       | `custom/Silvia.silvia/steamSetpoint`    |
| Current temperature       | `custom/Silvia.silvia/temperature`       |
| Preinfusion               | `custom/Silvia.silvia/preinfusion`      |
| Preinfusion pause         | `custom/Silvia.silvia/preinfusionPause` |
| Brew time                 | `custom/Silvia.silvia/targetBrewTime`   |
| Brew weight setpoint      | `custom/Silvia.silvia/targetBrewWeight` |
| Backflush state           | `custom/Silvia.silvia/backflushOn`      |
| Scale tare                | `custom/Silvia.silvia/scaleTareOn/set`  |
| Machine online/offline    | `custom/Silvia.silvia/status`           |

---

## 📄 File Structure

```
clevercoffee-display/
 ├── coffeeDisplay.yaml     # ESPHome configuration
 ├── secrets.yaml           # not committed, contains WiFi/MQTT credentials
 ├── README.md              # this file
 └── .gitignore             # excludes secrets & build files
```

---

## 🛠️ Development

To reflash after changes:

```bash
esphome run coffeeDisplay.yaml
```

To monitor the device:

```bash
esphome logs coffeeDisplay.yaml
```

---

## 📜 License

MIT License © 2024–2025
