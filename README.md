# Kali NetHunter KeX Rootless Setup - Complete Android Pentesting Environment

## 🚀 Ultimate Kali Linux on Android Solution with Audio Support

Transform your Android device into a powerful penetration testing platform with this comprehensive Kali NetHunter setup. This repository provides a complete, rootless installation guide that enables full Kali Linux functionality on Android devices using Termux, with working audio and browser sound capabilities.

### 🔥 Key Features
- ✅ **Rootless Installation** - No need to root your Android device
- ✅ **Full Audio Support** - System and browser audio working perfectly
- ✅ **XFCE Desktop Environment** - Complete desktop experience on Android
- ✅ **One-Command Start/Stop** - Simple scripts to launch and terminate the environment
- ✅ **Chromium Browser Sound** - Fixed audio issues in web browsers
- ✅ **Rebuild-Safe Configuration** - Survives Termux deletion and reinstallation
- ✅ **PulseAudio TCP Bridging** - Advanced audio streaming technology
- ✅ **No systemd Required** - Optimized for Android/Termux environment

### 🎯 Perfect For
- Mobile penetration testing
- On-the-go security assessments
- Learning cybersecurity concepts
- Ethical hacking practice
- Network security testing
- Mobile forensics
- Bug bounty hunting
- Red team exercises

### 📱 Compatible With
- Android smartphones and tablets
- Termux app
- NetHunter Store
- NetHunter KeX
- VNC Viewer

---

## 📋 Prerequisites

Install these essential apps from Play Store or F-Droid:
- **Termux** - Linux environment for Android
- **NetHunter Store** - Kali NetHunter installer
- **NetHunter KeX** - Kali desktop environment
- **VNC Viewer** (optional if KeX has embedded viewer)

---

## 🛠️ Installation Steps

### Step 1 – Termux Base Setup
```bash
pkg update -y && pkg upgrade -y
```

```bash
pkg install -y wget curl git proot pulseaudio
```

```bash
termux-change-repo
```

Enable:
- Main repository
- Community repository

Restart Termux after configuration.

---

### Step 2 – Install Kali NetHunter (Rootless)
```bash
wget -O install-nethunter-termux https://offse.ec/2MceZWr
```

```bash
chmod +x install-nethunter-termux
```

```bash
./install-nethunter-termux
```

After installation completes:
```bash
nethunter
```

---

### Step 3 – Fix Kali Base System
```bash
sudo apt update
```

```bash
sudo apt --fix-broken install -y
```

```bash
sudo dpkg --configure -a
```

```bash
sudo apt upgrade -y
```

---

### Step 4 – Install Desktop Environment & Audio Components
```bash
sudo apt install -y \
kali-desktop-xfce \
kali-win-key \
dbus-x11 \
pulseaudio pulseaudio-utils alsa-utils \
pavucontrol \
chromium
```

---

### Step 5 – Chromium Audio Fix (Essential)
```bash
mkdir -p ~/.local/bin
```

```bash
nano ~/.local/bin/chromium
```

**Paste this content:**
```bash
#!/bin/bash
export PULSE_SERVER=127.0.0.1
exec /usr/bin/chromium \
  --disable-features=AudioServiceSandbox \
  --no-sandbox "$@"
```

**Save, then:**

```bash
chmod +x ~/.local/bin/chromium
```

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
```

```bash
source ~/.bashrc
```

---

### Step 6 – Start Audio Server (Termux Only)
```bash
pulseaudio --kill || true
```

```bash
pulseaudio --start \
  --exit-idle-time=-1 \
  --load="module-native-protocol-tcp auth-ip-acl=127.0.0.1 auth-anonymous=1"
```

**⚠️ Important:** Do not close the Termux app after running this command.

---

### Step 7 – Configure Kali Audio Settings
Inside Kali environment:

```bash
echo 'export PULSE_SERVER=127.0.0.1' >> ~/.bashrc
```

```bash
source ~/.bashrc
```

**Note:** If you see "User-configured server at 127.0.0.1, refusing to start" - this is correct behavior.

---

### Step 8 – Test Audio Functionality
```bash
paplay /usr/share/sounds/alsa/Front_Center.wav
```

If you hear sound playback - the setup is successful!

---

### Step 9 – Create Easy Start Script (Termux)
```bash
nano ~/start-nethunter-kex.sh
```

**Paste this content:**
```bash
#!/data/data/com.termux/files/usr/bin/bash

echo "[+] Starting PulseAudio"
pulseaudio --kill 2>/dev/null
pulseaudio --start \
  --exit-idle-time=-1 \
  --load="module-native-protocol-tcp auth-ip-acl=127.0.0.1 auth-anonymous=1"

sleep 2

echo "[+] Starting NetHunter KeX"
nethunter -r "export PULSE_SERVER=127.0.0.1 && kex start"
```

**Save, then:**

```bash
chmod +x ~/start-nethunter-kex.sh
```

---

### Step 10 – Create Easy Stop Script (Termux)
```bash
nano ~/stop-nethunter-kex.sh
```

**Paste this content:**
```bash
#!/data/data/com.termux/files/usr/bin/bash

echo "[-] Stopping KeX"
nethunter -r "kex stop"

echo "[-] Stopping PulseAudio"
pulseaudio --kill
```

**Save, then:**

```bash
chmod +x ~/stop-nethunter-kex.sh
```

---

## 🎮 Usage Instructions

### Start Everything:
```bash
./start-nethunter-kex.sh
```

### Stop Everything:
```bash
./stop-nethunter-kex.sh
```

---

## ✅ Expected Results

After successful installation, you'll have:
- ✓ **NetHunter KeX** - Kali Linux desktop environment
- ✓ **XFCE Desktop** - Full desktop interface
- ✓ **Audio Working** - System sounds functional
- ✓ **Browser Sound** - Audio in Chromium browser
- ✓ **Chromium Fixed** - No audio sandbox issues
- ✓ **No systemd Required** - Android-optimized
- ✓ **Rebuild-Safe** - Configuration persists
- ✓ **Works After Termux Deletion** - Robust setup

---

## 🔍 SEO Keywords

This repository targets the following search terms for better discoverability:
- Kali Linux on Android
- NetHunter rootless setup
- Android penetration testing
- Termux Kali installation
- Android security tools
- Mobile pentesting
- Kali NetHunter audio fix
- Android ethical hacking
- Mobile security testing
- Kali Linux mobile setup
- Android cybersecurity
- Mobile hacking tools
- Kali on smartphone
- Android security lab
- Mobile forensic tools

---

## 🤝 Contributing

Found this guide helpful? Star the repository and share with others interested in mobile security and penetration testing!

---

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

---

## ⚠️ Disclaimer

This tool is intended for educational purposes and authorized security testing only. Always ensure you have proper authorization before conducting security assessments on systems you do not own.

---

## 🚀 About

This setup provides a complete Kali Linux environment on Android with working audio and browser sound, all in a rootless configuration that survives rebuilds and works after Termux deletion. Perfect for cybersecurity professionals, students, and enthusiasts who need a portable penetration testing environment.
