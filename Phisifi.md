# PhiSiFi: ESP8266 WiFi Attack Toolkit

## Overview

**PhiSiFi** is a WiFi hacking toolkit for the ESP8266 microcontroller, combining features from [ESP8266-EvilTwin](https://github.com/M1z23R/ESP8266-EvilTwin) and [ESP8266-Captive-Portal](https://github.com/adamff1/ESP8266-Captive-Portal), with core deauthentication logic from [SpacehuhnTech/esp8266_deauther](https://github.com/SpacehuhnTech/esp8266_deauther). It enables both **Deauth** and **Evil Twin** attacks independently or simultaneously with a simple web interface for managing the exploit. This project is intended for educational use and WiFi security research on networks you own and have explicit permission to test.

## Features

- **Deauthentication**: Disconnects clients from a target WiFi access point.
- **Evil Twin Attack**: Clones the target AP and presents a captive portal to capture WiFi passwords, with live password verification against the original access point.
- **Simultaneous Attacks**: Run deauth and Evil Twin attacks together no need to toggle.
- **Web Interface**: Administer attacks, monitor devices, and collect credentials from any connected phone or PC.

## Installation

1. **Install Arduino IDE**
2. Go to `File` → `Preferences`, add this to "Additional Boards Manager URLs":

```
https://raw.githubusercontent.com/SpacehuhnTech/arduino/main/package_spacehuhn_index.json
```

3. Go to `Tools` → `Board` → `Boards Manager` and install the **deauther** package.
4. Download and open `ESP8266_PhiSiFi.ino` in Arduino IDE.
5. Select your **ESP8266 Deauther** board: `Tools` → `Board`.
6. Install [CP210x Windows Driver](https://www.silabs.com/software-and-tools/usb-to-uart-bridge-vcp-drivers?tab=downloads) (required for many ESP8266 modules).
7. Connect your ESP8266 device, select the correct port under `Tools` → `Port`.
8. Click **Upload**.

## Usage

1. **Connect** to the AP:
    - SSID: `WiPhi_34732`
    - Password: `d347h320`
2. **Scan Targets**: Select the target AP (refreshes every 30s).
3. **Deauth Attack**: Start "Deauthing" to disconnect clients from the target AP.
4. **Evil Twin Attack**: Start the Evil Twin; optionally reconnect to the fake AP (same SSID, open).
5. **Admin Panel**: Visit `192.168.4.1/admin` while connected to the Evil Twin AP to stop any attack.
6. **Passwords**: When a correct WiFi password is captured, it is displayed in the admin table. Note: Data is lost on device reset or power down.

## Legal \& Ethical Disclaimer

- **Use for educational and authorized penetration testing only.**
- Consult local laws and regulations before testing WiFi exploits.


## Credits

- [p3tr0s/PhiSiFi](https://github.com/p3tr0s/PhiSiFi)
- [SpacehuhnTech/esp8266_deauther](https://github.com/SpacehuhnTech/esp8266_deauther)
- [M1z23R/ESP8266-EvilTwin](https://github.com/M1z23R/ESP8266-EvilTwin)
- [adamff1/ESP8266-Captive-Portal](https://github.com/adamff1/ESP8266-Captive-Portal)

