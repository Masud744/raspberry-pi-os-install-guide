
# 🆘 Troubleshooting Raspberry Pi OS Installation (Raspberry Pi 3B)

This file collects common issues you may face while installing or booting Raspberry Pi OS on a Raspberry Pi 3 Model B, along with practical fixes.

---

## 1️⃣ Pi Has Red LED ON but Green LED Never Blinks

**Symptoms:**
- Red LED solid ON  
- Green LED completely OFF (no blink at all)  
- No image on monitor

**Likely Causes:**
- SD card does not contain a valid boot partition
- Wrong image (e.g. EEPROM / recovery / NOOBS) was flashed
- SD card is corrupted or fake

**Fix:**
1. Backup any data from SD card (if needed).
2. Format SD card as **FAT32**.
3. Use **Raspberry Pi Imager**.
4. Select: `Raspberry Pi OS (other) → Raspberry Pi OS (32-bit)`.
5. Write + Verify again.
6. Try booting the Pi.

---

## 2️⃣ Red LED is Blinking

**Symptoms:**
- Red LED flickers repeatedly
- Pi reboots randomly or never boots fully

**Likely Cause:**
- Power supply is too weak or unstable.

**Fix:**
- Use a proper **5V 2.5A** power adapter.
- Avoid cheap USB phone chargers and thin cables.

---

## 3️⃣ Pi Boots into “Recovery” or “Installer” Menu

**Symptoms:**
- Screen shows a recovery or NOOBS/PINN installer, not Raspberry Pi OS.

**Likely Cause:**
- A recovery / NOOBS / PINN image was flashed instead of Raspberry Pi OS.

**Fix:**
1. Format SD card.
2. Flash again using **Raspberry Pi OS (32‑bit)** only.
3. Confirm boot files (`bootcode.bin`, `kernel.img`, etc.) after flashing.

---

## 4️⃣ Cannot SSH into Raspberry Pi

**Symptoms:**
- SSH connection refused or times out.

**Possible Reasons & Fixes:**

### a) SSH not enabled
- Ensure there is an **empty file** named `ssh` (no extension) in the **boot** partition *before* first boot.

### b) Wrong IP address
- Use `ping raspberrypi.local` or the **Fing** app or router admin page to confirm the IP.

### c) Firewall / Network restrictions
- Make sure your PC and Pi are on the same network.
- Disable overly strict firewall temporarily to test.

---

## 5️⃣ Wi‑Fi Not Working

**Symptoms:**
- Pi does not appear on network.
- `wpa_supplicant.conf` created but still no connection.

**Checklist:**
- SSID and password are typed exactly (case‑sensitive).
- Country code in `wpa_supplicant.conf` matches your region (`BD` for Bangladesh, `US` for United States, etc.).
- 2.4 GHz network is enabled on your router (Pi 3B prefers 2.4 GHz).

---

## 6️⃣ VNC / Remote Desktop Not Connecting

**VNC Issues:**
- Ensure VNC is enabled:

```bash
sudo raspi-config
# Interface Options → VNC → Enable
```

**RDP Issues (xrdp):**
- Confirm xrdp is installed:

```bash
sudo apt install xrdp -y
```

- Then use `mstsc` from Windows and connect to `raspberrypi.local` or the Pi’s IP.

---

## 7️⃣ Very Slow Performance

**Tips:**
- Use a **Class 10** or better microSD card.
- Avoid running heavy browsers with many tabs on Pi 3B.
- Keep the system updated:

```bash
sudo apt update && sudo apt upgrade -y
```

---

If you discover a new issue, feel free to open a GitHub Issue and contribute improvements or additional troubleshooting tips. 🙂
