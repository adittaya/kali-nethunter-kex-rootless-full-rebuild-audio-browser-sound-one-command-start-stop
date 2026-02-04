==============================
KALI NETHUNTER KeX (ROOTLESS)
FULL REBUILD + AUDIO + BROWSER SOUND
ONE-COMMAND START / STOP
==============================

TARGET:
- Android
- Termux
- Kali NetHunter (rootless)
- NetHunter KeX
- Audio WORKING (system + browser)
- Chromium sound FIXED
- No systemd
- VNC/KeX compatible

==============================
STEP 0 — INSTALL APPS (MANUAL)
==============================
Install from Play Store / F-Droid:
- Termux
- NetHunter Store
- NetHunter KeX
- VNC Viewer (if KeX not embedded)

==============================
STEP 1 — TERMUX BASE SETUP
==============================

pkg update -y && pkg upgrade -y
pkg install -y wget curl git proot pulseaudio
termux-change-repo

Enable:
- Main
- Community

Restart Termux.

==============================
STEP 2 — INSTALL KALI NETHUNTER (ROOTLESS)
==============================

wget -O install-nethunter-termux https://offs.ec/2MceZWr
chmod +x install-nethunter-termux
./install-nethunter-termux

After install:
nethunter

==============================
STEP 3 — FIX KALI BASE
==============================

sudo apt update
sudo apt --fix-broken install -y
sudo dpkg --configure -a
sudo apt upgrade -y

==============================
STEP 4 — INSTALL KeX + DESKTOP + AUDIO
==============================

sudo apt install -y \
kali-desktop-xfce \
kali-win-kex \
dbus-x11 \
pulseaudio pulseaudio-utils alsa-utils \
pavucontrol \
chromium

==============================
STEP 5 — CHROMIUM AUDIO FIX (MANDATORY)
==============================

mkdir -p ~/.local/bin
nano ~/.local/bin/chromium

PASTE:
#!/bin/bash
export PULSE_SERVER=127.0.0.1
exec /usr/bin/chromium \
  --disable-features=AudioServiceSandbox \
  --no-sandbox "$@"

SAVE, THEN:

chmod +x ~/.local/bin/chromium
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

==============================
STEP 6 — START AUDIO SERVER (TERMUX ONLY)
==============================

pulseaudio --kill || true
pulseaudio --start \
  --exit-idle-time=-1 \
  --load="module-native-protocol-tcp auth-ip-acl=127.0.0.1 auth-anonymous=1"

DO NOT CLOSE TERMUX.

==============================
STEP 7 — KALI AUDIO CONFIG
==============================

Inside Kali:

echo 'export PULSE_SERVER=127.0.0.1' >> ~/.bashrc
source ~/.bashrc

NOTE:
If you see:
"User-configured server at 127.0.0.1, refusing to start"
THIS IS CORRECT.

==============================
STEP 8 — TEST AUDIO
==============================

paplay /usr/share/sounds/alsa/Front_Center.wav

If sound plays → OK.

==============================
STEP 9 — EASY START SCRIPT (TERMUX)
==============================

nano ~/start-nethunter-kex.sh

PASTE:
#!/data/data/com.termux/files/usr/bin/bash

echo "[+] Starting PulseAudio"
pulseaudio --kill 2>/dev/null
pulseaudio --start \
  --exit-idle-time=-1 \
  --load="module-native-protocol-tcp auth-ip-acl=127.0.0.1 auth-anonymous=1"

sleep 2

echo "[+] Starting NetHunter KeX"
nethunter -r "export PULSE_SERVER=127.0.0.1 && kex start"

SAVE, THEN:

chmod +x ~/start-nethunter-kex.sh

==============================
STEP 10 — EASY STOP SCRIPT (TERMUX)
==============================

nano ~/stop-nethunter-kex.sh

PASTE:
#!/data/data/com.termux/files/usr/bin/bash

echo "[-] Stopping KeX"
nethunter -r "kex stop"

echo "[-] Stopping PulseAudio"
pulseaudio --kill

SAVE, THEN:

chmod +x ~/stop-nethunter-kex.sh

==============================
USAGE
==============================

START EVERYTHING:
./start-nethunter-kex.sh

STOP EVERYTHING:
./stop-nethunter-kex.sh

==============================
RESULT
==============================

✓ NetHunter KeX
✓ XFCE Desktop
✓ Audio working
✓ Browser sound working
✓ Chromium fixed
✓ No systemd
✓ Rebuild-safe
✓ Works after Termux deletion

==============================
END
==============================
