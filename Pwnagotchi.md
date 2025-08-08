
# Pwnagotchi 101

_Pwnagotchi_ is an A2C-based “AI” powered by [bettercap](https://www.bettercap.org/) and runs on a Raspberry Pi. It learns from its surrounding WiFi environment to maximize the capture of crackable WPA key material (by passive sniffing or active deauthentication/association attacks). This material is collected as PCAP files containing WPA handshakes (full, half, PMKID) for use with tools like hashcat.

## Pwnagotchi Interface and Raspberry pi Zero 2 W Ports

<img width="1195" height="819" alt="User-Interface" src="https://github.com/user-attachments/assets/7d2b3423-a459-40d5-a912-769da5cef1b1" />


<img width="861" height="496" alt="Pasberry_ports" src="https://github.com/user-attachments/assets/d45ce354-8d63-4790-88f5-8063bb1234e3" />

## Hardware Used

- **Raspberry Pi:** Zero 2 W
- **Display:** Waveshare v4 2.13-inch E-Ink
- **Storage:** Micro SD Card (16GB)
- **Case:** PicoProject 3D Printed Case
- **Micro-USB Cable:** (for data transfer)
- **Power:** Portable Power Bank / PiSugar


## How WPA/WPA2 Handshakes Work

See [WPA/WPA2 4-Way Handshake](https://www.wifi-professionals.com/2019/01/4-way-handshake) for details.

<img width="885" height="959" alt="4_Way_Handshake" src="https://github.com/user-attachments/assets/099134d1-9d73-44b4-a974-26c1c8d56798" />

## Getting Started

**Downloads \& Software:**

- **Pwnagotchi Image:**
Use [jayofelony/pwnagotchi releases](https://github.com/jayofelony/pwnagotchi/releases)
    - 32-bit for Raspberry Pi Zero W
    - 64-bit for Raspberry Pi Zero 2 W
- [RNDIS Driver (Windows)](https://support.xtool.com/article/1350) - Driver needed only for windows
- [Balena Etcher](https://etcher.balena.io/#download-etcher) OR [Raspberry Pi Imager](https://www.raspberrypi.com/software/) - Image Flasher
- [PuTTY (SSH client)](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) - For SSH Connection to Pwnagotchi
- [FileZilla (File transfer)](https://filezilla-project.org/) - For Easy File transfer

## Network Settings (Windows)

1. Control Panel → Network and Internet → Network and Sharing Options → Change Adapter Settings
2. Locate your network device (often _Ethernet 3_ or _RNDIS Adapter_)
3. Right-click on Properties → Select _Internet Protocol Version 4 (TCP/IPv4)_
4. Set:
    - **IP Address:** `10.0.0.1`
    - **Subnet Mask:** `255.255.255.0`
    - **Preferred DNS:** `8.8.8.8`
5. Save, and make sure Internet Connection Sharing is enabled for your main network device.

## Configuration \& Setup

### Connect via SSH

```shell
ssh pi@10.0.0.2   # default password: raspberry
```

### Share Internet with Pwnagotchi

1. Connect your Pi to your computer using a data-capable USB cable.
2. Download the Powershell connection sharing script:
[win_connection_share.ps1](https://github.com/evilsocket/pwnagotchi/blob/master/scripts/win_connection_share.ps1)
3. Run the following PowerShell commands:

```powershell
powershell -ExecutionPolicy ByPass -File .\win_connection_share.ps1 -SetPwnagotchiSubnet
powershell -ExecutionPolicy ByPass -File .\win_connection_share.ps1 -EnableInternetConnectionSharing
```

### Setting Up Pwnagotchi Configuration

#### Using Wizard

While connected over SSH, launch the wizard:

```shell
sudo pwnagotchi --wizard
```

This creates `/etc/pwnagotchi/config.toml`.

View current config:

```shell
sudo cat /etc/pwnagotchi/config.toml
```


#### Manual Editing

Edit `config.toml` yourself with:

```shell
sudo nano /etc/pwnagotchi/config.toml
```

- If the file doesn’t exist, this will create it.
- Make changes as needed, then press `CTRL + X` to exit and save.

#### Example config.toml

```toml
main.name = "pwnagotchi"
main.lang = "en"
main.whitelist = [
  "EXAMPLE_NETWORK",
  "ANOTHER_EXAMPLE_NETWORK",
  "fo:od:ba:be:fo:od",
  "fo:od:ba"
]

ui.display.enabled = true
ui.display.type = "waveshare_4"   # Change if using a different screen
ui.display.color = "black"
ui.fps = 1
```

See the [default config](https://github.com/jayofelony/pwnagotchi/blob/noai/pwnagotchi/defaults.toml) for all available settings.

## Plugins Docs and Customization
[3rd Party Plugins](https://docs.google.com/spreadsheets/d/1os8TRM3Pc9Tpkqzwu548QsDFHNXGuRBiRDYEsF3-w_A/edit?gid=0#gid=0)
[Custom Face mods](https://github.com/roodriiigooo/PWNAGOTCHI-CUSTOM-FACES-MOD)

## Modifications
Enhance your Pwnagotchi with these hardware modifications for increased power, better portability, or a sleek look.

## External Antenna

**Boost your signal by adding an external antenna!**
You have two options:

- **USB-Antenna** (recommended for most users)
- **IPEX-Connected Antenna** (advanced DIY)

### USB-Antenna

1. **Preparation:**
    - Insert your already flashed SD card into your computer and navigate to the `/BOOT/` directory.
    - Locate the file `config.txt`.
2. **Configuration:**
    - Open `config.txt` and add this line at the end:

```
dtoverlay=disable-wifi
```

    - _Note: Some antennas require monitor mode drivers to work properly. Ensure your adapter supports Linux and monitor mode before purchasing!_
3. **Installation:**
    - Plug your USB Wi-Fi antenna into your Pwnagotchi (use the data port if you have a Raspberry Pi 0W or 02W).
    - Reboot your device.
4. **Done — Enjoy your upgraded Pwnagotchi!**

### IPEX-Antenna (Advanced!)

> ⚠️ **Caution:** Modifying your Pi in this way can permanently damage it or void FCC certification. Proceed only if you're confident in your skills.

**Benefits:**
An IPEX antenna can improve Wi-Fi signal by ~6–8dB, but you’ll lose the onboard antenna.

**Requirements:**

- **Parts:**
    - IPEX (U.FL) connector
    - Coax cable
    - SMA antenna
- **Tools:**
    - Soldering iron (or hot air rework station, optional)
    - Solder, flux
    - Magnification device (jeweler’s loupe or stereo microscope is ideal)
    - Steady hands!

**Steps:**

1. **Disassemble** your Raspberry Pi and locate the antenna trace.
2. **Desolder \& Move** the 0Ω resistor to reroute the signal to the new connector (refer to a detailed guide with images such as [this one](https://briandorey.com/post/raspberry-pi-zero-w-external-antenna-mod)).
3. **Solder** the IPEX connector in place.
4. **Attach** the coax cable and external antenna.
5. **Test** your Pwnagotchi!

_**Warning:** If you lack soldering skills or tools, you risk irreparably damaging your Pi. Magnification is crucial for moving the tiny resistor safely._
_Contributed by @Mastblast09._

## Slimagotchi (Advanced Mod)

Make your Pwnagotchi ultra-slim for portability and minimalist style!

_Contributed by @Mastblast09._

**Tools Needed:**

- Soldering iron
- Flux
- Copper braided wick
- Side cutter

**Parts Needed:**

- Raspberry Pi Zero W
- Waveshare e-ink screen
- Kapton tape

**Instructions:**

1. **Prepare the Board:**
    - Apply flux to the copper wick and the connector pins.
    - Set your soldering iron to ~300˚C (enough to flow solder into the wick).
    - Use the wick and soldering iron to remove the header pins from your Raspberry Pi.
2. **Clean Up:**
    - If necessary, use a side cutter to carefully remove stubborn side pads or pins.
    - Ensure the pads are flat and clean.
3. **Isolate Components:**
    - Apply Kapton tape to the back of the board to prevent short circuits.
4. **Make Custom Pins:**
    - Cut pin headers into individual pins (see guide images for reference).
    - Insert the pins into the screen connector holes.
5. **Mount \& Solder:**
    - Place the Raspberry Pi on top of the pins.
    - Solder the pins to create a low-profile connection between the screen and board.
6. **Done!**
    - Enjoy your slim, space-saving Pwnagotchi.

## References \& Further Reading

- [Author's Blog: Weaponizing and Gamifying AI for WiFi Hacking](https://www.evilsocket.net/2019/10/19/Weaponizing-and-Gamifying-AI-for-WiFi-Hacking-Presenting-Pwnagotchi-1-0-0/)
- [Official Website](https://pwnagotchi.ai/)
- [Community + Plugins](https://pwnagotchi.org/index.html)
- [Detailed Video Guide for Plugins](https://www.youtube.com/watch?v=hVsF-rUosek&list=PLXEaT4M-U7Uj019317YLeUxdcE3RmgUay&index=15)

