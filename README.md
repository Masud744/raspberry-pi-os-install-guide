
# 🍓 Raspberry Pi OS Installation Guide (Raspberry Pi 3 Model B)

A complete, beginner‑friendly guide to correctly install **Raspberry Pi OS** on a **Raspberry Pi 3 Model B**,  
including flashing, first boot, **remote access (SSH, VNC, RDP)** and common troubleshooting tips.

---

## ✅ 1. Requirements

### 🧰 Hardware
- Raspberry Pi **3 Model B**
- microSD card **(16–32 GB, Class 10 recommended)**
- Good quality **SD card reader**
- **5V 2.5A** power adapter for Pi
- (Optional) HDMI monitor, keyboard and mouse

### 💻 Software (Official / Trusted)

- **Raspberry Pi Imager**  
  👉 https://www.raspberrypi.com/software/

- **Raspberry Pi OS (manual download, if needed)**  
  👉 https://www.raspberrypi.com/software/operating-systems/

- **balenaEtcher** (alternative flasher)  
  👉 https://www.balena.io/etcher/

- **PuTTY (SSH client for Windows)**  
  👉 https://www.putty.org/

- **RealVNC Viewer (remote desktop)**  
  👉 https://www.realvnc.com/en/connect/download/viewer/

- **Fing mobile app (to find Pi IP easily)**  
  👉 https://www.fing.com/products/fing-app

---

## 💿 2. Prepare & Format the microSD Card

> ⚠️ This will erase everything on the SD card.

1. Insert the SD card into your PC using a card reader.
2. Open **File Explorer → This PC**.
3. Right‑click the SD card → **Format…**
4. File system: **FAT32**
5. Check **Quick Format** → Click **Start**.
6. When it finishes, close the window.

---

## 🧊 3. Flash Raspberry Pi OS using Raspberry Pi Imager

1. Open **Raspberry Pi Imager**.
2. Click **Choose OS** →  
   `Raspberry Pi OS (other)` → **Raspberry Pi OS (32‑bit)**  
   > This is the recommended OS for Raspberry Pi 3 Model B.
3. Click **Choose Storage** → select your SD card.
4. Click **Write** → confirm if asked.
5. Wait until **Write** and **Verify** both complete (100%).

Now eject and re‑insert the SD card.  
You should see boot files like:

```text
bootcode.bin
start.elf
kernel.img
cmdline.txt
config.txt
overlays/
```

> ❌ If you see folders like `recovery`, `defaults`, `os` only → that is a **recovery/installer image**,  
> not Raspberry Pi OS. Format the card and flash again with **Raspberry Pi OS (32‑bit)**.

---

## 🚀 4. First Boot on Raspberry Pi

1. Insert the flashed microSD card into the **Raspberry Pi 3B**.
2. Connect monitor (HDMI), keyboard and mouse (optional, if not doing headless).
3. Connect the power adapter to boot.

### 🔴🟢 LED Indicator Guide (Pi 3B)

| LED  | State            | Meaning                              |
|------|------------------|--------------------------------------|
| Red  | Solid ON         | Power OK                             |
| Red  | Blinking         | Power issue (use 5V 2.5A adapter)   |
| Green| Blinking         | SD card is being read (boot OK)     |
| Green| OFF (never blinks)| Boot files missing / SD card error |

If everything is correct, you should see:

- A rainbow splash screen
- Then the Raspberry Pi logo
- Then the Raspberry Pi OS setup wizard

---

## 🧠 5. Headless Setup (No Monitor / Keyboard Needed)

You can control your Raspberry Pi entirely from your PC using **SSH** and **VNC**.

### 5.1 Enable SSH

1. With the SD card still in your PC, open the **boot** partition.
2. Create an **empty file** named:

```text
ssh
```

- No file extension
- The file can be completely empty  
This tells Raspberry Pi OS to enable **SSH** on first boot.

---

### 5.2 Configure Wi‑Fi (Optional but useful)

In the same **boot** partition, create a file:

```text
wpa_supplicant.conf
```

Paste this (change Wi‑Fi name & password):

```conf
country=BD
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="Your_WiFi_Name"
    psk="Your_WiFi_Password"
}
```

Save → eject SD card → insert into Pi → power on.  
The Pi will automatically connect to this Wi‑Fi network.

---

## 🌐 6. Find Your Raspberry Pi’s IP Address

You need the IP to connect from your PC.

You can use **any one** of these methods:

### 📱 Method 1 – Using Fing (Mobile App)
1. Install **Fing** on your phone.  
2. Connect phone to the same Wi‑Fi network as the Pi.  
3. Scan the network – look for a device named **`raspberrypi`**.

### 🌍 Method 2 – Router Admin Page
1. Login to your Wi‑Fi router’s web interface.
2. Check the *Connected Devices* / *DHCP Clients* list.
3. Look for **`raspberrypi`** and note its IP.

### 💻 Method 3 – From PC
On Windows CMD / PowerShell:

```bash
ping raspberrypi.local
```

If mDNS is working, this gives the IP of your Pi.

---

## 🛜 7. SSH Remote Access (Terminal)

From **Windows** (CMD, PowerShell or PuTTY):

```bash
ssh pi@raspberrypi.local
# or, using IP:
ssh pi@<PI_IP_ADDRESS>
```

Default credentials (for fresh Raspberry Pi OS):

- **Username:** `pi`  
- **Password:** `raspberry` (you will be asked to change this on first login)

Once logged in, you have full terminal access.

Useful commands:

```bash
sudo apt update && sudo apt upgrade -y   # update system
hostname -I                              # show IP address
sudo reboot                              # restart Pi
```

---

## 🖥️ 8. VNC Remote Desktop (Full GUI from PC)

### 8.1 Enable VNC on Raspberry Pi

SSH into the Pi, then run:

```bash
sudo raspi-config
```

Navigate:

`Interface Options  →  VNC  →  Enable`

Exit and reboot if asked.

---

### 8.2 Connect from PC using RealVNC Viewer

1. Install **VNC Viewer** on your PC:  
   https://www.realvnc.com/en/connect/download/viewer/
2. Open VNC Viewer and enter:

```text
raspberrypi.local
# or:
<PI_IP_ADDRESS>
```

3. Login with:
   - Username: `pi`
   - Password: (your Pi password)

Now you can see and control the **full Raspberry Pi desktop** from your PC.

---

## 🪟 9. Remote Desktop via RDP (Alternative to VNC)

If you prefer using Windows’ built‑in Remote Desktop:

### 9.1 Install xrdp on Raspberry Pi

SSH into the Pi and run:

```bash
sudo apt update
sudo apt install xrdp -y
```

### 9.2 Connect from Windows

1. Press `Win + R` → type `mstsc` → Enter.  
2. In **Computer** field, enter:

```text
raspberrypi.local
# or:
<PI_IP_ADDRESS>
```

3. Click **Connect**, then login with your Pi username and password.

---

## 🧩 10. Direct LAN Cable Connection (No Router)

You can connect your Raspberry Pi directly to your PC using an Ethernet cable:

1. Connect Pi ↔ PC using a LAN cable.
2. Windows will usually create a private network and assign IPs automatically.
3. Check Pi’s IP from Windows using:

```bash
arp -a
```

Then SSH / VNC into the Pi using that IP.

---

## 🆘 11. Troubleshooting (Summary)

More details are available in [`troubleshooting.md`](troubleshooting.md).

| Problem                                        | Possible Cause                          | Solution                                        |
|-----------------------------------------------|-----------------------------------------|-------------------------------------------------|
| Red LED solid, green LED never blinks         | No valid OS / SD card error            | Reflash SD card with **Raspberry Pi OS (32‑bit)** |
| Red LED blinking                              | Power supply problem                    | Use official‑quality **5V 2.5A** adapter        |
| Pi boots into recovery installer              | You flashed a recovery / NOOBS image    | Format SD card and flash Raspberry Pi OS again  |
| SSH not working                               | `ssh` file missing                      | Add empty `ssh` file in boot partition          |
| Wi‑Fi not connecting                          | Wrong SSID/password or country code     | Fix `wpa_supplicant.conf` and reboot            |
| VNC connection refused                        | VNC not enabled                         | Enable via `sudo raspi-config`                  |

---

## 👨‍💻 12. Author

**Masud**  
B.Sc. in IoT & Robotics Engineering
University of Frontier Technology, Bangladesh
Passionate about **Raspberry Pi, IoT, Robotics, and AI**

> Feel free to open an Issue or Pull Request if you find a bug or want to improve this guide.
