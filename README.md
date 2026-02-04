# KALI NETHUNTER KeX (ROOTLESS) - Complete Setup Guide

## TARGET:
- Android
- Termux
- Kali NetHunter (rootless)
- NetHunter KeX
- Audio WORKING (system + browser)
- Chromium sound FIXED
- No systemd
- VNC/KeX compatible

---

## STEP 0 – INSTALL APPS (MANUAL)
Install from Play Store / F-Droid:
- Termux
- NetHunter Store
- NetHunter KeX
- VNC Viewer (if KeX not embedded)

---

## STEP 1 – TERMUX BASE SETUP

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
- Main
- Community

Restart Termux.

---

## STEP 2 – INSTALL KALI NETHUNTER (ROOTLESS)

```bash
wget -O install-nethunter-termux https://offse.ec/2MceZWr
```

```bash
chmod +x install-nethunter-termux
```

```bash
./install-nethunter-termux
```

After install:
```bash
nethunter
```

---

## STEP 3 – FIX KALI BASE

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

## STEP 4 – INSTALL KeX + DESKTOP + AUDIO

```bash
sudo apt install -y kali-desktop-xfce kali-win-key dbus-x11 pulseaudio pulseaudio-utils alsa-utils pavucontrol chromium
```

---

## STEP 5 – CHROMIUM AUDIO FIX (MANDATORY)

```bash
mkdir -p ~/.local/bin
```

```bash
nano ~/.local/bin/chromium
```

PASTE THIS CONTENT:
```bash
#!/bin/bash
export PULSE_SERVER=127.0.0.1
exec /usr/bin/chromium   --disable-features=AudioServiceSandbox   --no-sandbox "$@"
```

SAVE, THEN:

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

## STEP 6 – START AUDIO SERVER (TERMUX ONLY)

```bash
pulseaudio --kill || true
```

```bash
pulseaudio --start   --exit-idle-time=-1   --load="module-native-protocol-tcp auth-ip-acl=127.0.0.1 auth-anonymous=1"
```

DO NOT CLOSE TERMUX.

---

## STEP 7 – KALI AUDIO CONFIG

Inside Kali:

```bash
echo 'export PULSE_SERVER=127.0.0.1' >> ~/.bashrc
```

```bash
source ~/.bashrc
```

NOTE:
If you see:
"User-configured server at 127.0.0.1, refusing to start"
THIS IS CORRECT.

---

## STEP 8 – TEST AUDIO

```bash
paplay /usr/share/sounds/alsa/Front_Center.wav
```

If sound plays — OK.

---

## STEP 9 – EASY START SCRIPT (TERMUX)

```bash
nano ~/start-nethunter-kex.sh
```

PASTE THIS CONTENT:
```bash
#!/data/data/com.termux/files/usr/bin/bash

echo "[+] Starting PulseAudio"
pulseaudio --kill 2>/dev/null
pulseaudio --start   --exit-idle-time=-1   --load="module-native-protocol-tcp auth-ip-acl=127.0.0.1 auth-anonymous=1"

sleep 2

echo "[+] Starting NetHunter KeX"
nethunter -r "export PULSE_SERVER=127.0.0.1 && kex start"
```

SAVE, THEN:

```bash
chmod +x ~/start-nethunter-kex.sh
```

---

## STEP 10 – EASY STOP SCRIPT (TERMUX)

```bash
nano ~/stop-nethunter-kex.sh
```

PASTE THIS CONTENT:
```bash
#!/data/data/com.termux/files/usr/bin/bash

echo "[-] Stopping KeX"
nethunter -r "kex stop"

echo "[-] Stopping PulseAudio"
pulseaudio --kill
```

SAVE, THEN:

```bash
chmod +x ~/stop-nethunter-kex.sh
```

---

## USAGE

START EVERYTHING:
```bash
./start-nethunter-kex.sh
```

STOP EVERYTHING:
```bash
./stop-nethunter-kex.sh
```

---

## RESULT

✓ NetHunter KeX  
✓ XFCE Desktop  
✓ Audio working  
✓ Browser sound working  
✓ Chromium fixed  
✓ No systemd  
✓ Rebuild-safe  
✓ Works after Termux deletion  

---

## END

This setup provides a complete Kali Linux environment on Android with working audio and browser sound, all in a rootless configuration that survives rebuilds and works after Termux deletion.
