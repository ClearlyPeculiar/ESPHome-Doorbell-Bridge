# ESPHome Doorbell Bridge

This project provides an ESPHome-based configuration for an ESP-01S relay module to modernize a traditional doorbell. It enables smart detection and "Silent Mode" while maintaining physical chime functionality.

## 🎯 Features
- **Doorbell Press Detection**: Monitors a physical button on **GPIO2**.
- **Chime Control**: Operates a physical chime via the relay on **GPIO0**.
- **Home Assistant Integration**: Manually trigger the chime or toggle "Silent Mode" directly from HA.
- **Configurable Timing**: Adjust "Ring Duration" and "Post-Chime Delay" via HA sliders.
- **Silent Mode**: Decouples the button from the chime (allowing notifications without sound).
- **Remote Package Support**: Can be easily imported into your local config while keeping sensitive data private.

## 🚀 Quick Start (Remote Package)

You can use this project as a package without needing to clone it locally. Simply add the following to your ESPHome configuration file (e.g., `my-doorbell.yaml`):

```yaml
esphome:
  name: doorbell-bridge
  friendly_name: Doorbell Bridge

# Define your pinout here
substitutions:
  button_pin: GPIO2
  relay_pin: GPIO0
  status_led_pin: GPIO1

esp8266:
  board: esp01_1m

# Import the core logic directly from GitHub
packages:
  doorbell:
    url: https://github.com/ClearlyPeculiar/ESPHome-Doorbell-Bridge
    file: doorbell-bridge.yaml
    refresh: 1d

# Your local configuration
wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

api:
  encryption:
    key: !secret api_key

ota:
  - platform: esphome
    password: !secret ota_password

captive_portal:
```

## 🛠️ Hardware Requirements
- **ESP-01S Relay Module**: Standard 5V module.
- **Power**: 5V DC power supply.
- **Optocoupler (Optional)**: For isolating the doorbell button signal.

## 📋 Home Assistant Entities
### 🎮 Controls
- **Chime Relay**: Manual trigger for the physical chime (automatically pulses).
- **Silent Mode**: Toggle to enable/disable the physical chime while keeping notifications active.
- **Restart Device**: Button to remotely reboot the ESP-01S.

### ⚙️ Configuration
- **Chime Ring Duration**: Duration the relay remains active (Default: 500ms).
- **Post-Chime Delay**: Minimum wait time between rings to prevent hardware strain (Default: 1000ms).

## 📋 Pinout Reference (Configurable via Substitutions)
- **button_pin**: `GPIO2` (Doorbell Button)
- **relay_pin**: `GPIO0` (Chime Relay)
- **status_led_pin**: `GPIO1` (Blue LED)

## 🚀 Flashing Instructions
1. **Prepare ESPHome**:
   ```bash
   pip install esphome
   ```
2. **Update Credentials**: Create your local configuration file and set your WiFi/API secrets.
3. **Compile and Upload**:
   ```bash
   esphome run your-local-config.yaml
   ```

## 🛡️ Safety Warning
Ensure all high-voltage connections (AC doorbell transformer) are properly isolated.
