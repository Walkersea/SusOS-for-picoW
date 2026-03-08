# SusOS

A terminal-based OS for the Raspberry Pi Pico W, controlled from a browser over USB serial.

---

## What you need

- Raspberry Pi Pico W
- USB cable (data, not charge-only)
- Cheap buzzer wired to **GP15** and **GND**
- Chrome or Edge browser (Web Serial API required — Firefox will not work)
- The `susos-terminal.html` file

---

## First-time setup

### Step 1 — Wipe the Pico W clean

Before flashing MicroPython it is strongly recommended to erase everything on the Pico W first using the official flash nuke utility.

1. Download **flash_nuke.uf2** from the official Raspberry Pi repository:
   **https://datasheets.raspberrypi.com/soft/flash_nuke.uf2**
2. Hold the **BOOTSEL** button on the Pico W and plug it into USB while holding it.
3. A drive called **RPI-RP2** will appear on your computer.
4. Drag and drop `flash_nuke.uf2` onto the drive.
5. The Pico W will reboot, erase its flash, and remount as **RPI-RP2** again.

### Step 2 — Flash MicroPython

1. Download the latest **Raspberry Pi Pico W** MicroPython firmware (`.uf2`) from:
   **https://micropython.org/download/RPI_PICO_W/**
   Download the latest stable release.
2. The Pico W should still be mounted as **RPI-RP2**. If not, hold BOOTSEL and replug.
3. Drag and drop the `.uf2` file onto the drive.
4. The Pico W will reboot automatically. The drive will disappear — this is normal.
   MicroPython is now running.

### Step 3 — Install SusOS

1. Open `susos-terminal.html` in Chrome or Edge.
2. Click **Connect** in the top-right corner and select your Pico W's serial port
   (usually listed as something like *USB Serial Device* or *Pico W*).
3. Click **Install OS to Pico**.
4. The terminal will upload `main.py` to the Pico W and verify the byte count.
5. The Pico W will soft-reset and SusOS will start automatically.
   You will see the `$` prompt.

---

## Importing songs

SusOS plays **RTTTL** files through the buzzer on GP15.

1. Find or create an `.rtttl` file. Many free RTTTL ringtones are available online.
2. With the Pico W connected, click **Import RTTTL** in the top-right corner.
3. Select your `.rtttl` file. It will be uploaded to `/songs/` on the Pico W.
4. Play it with `sus sound play <filename>`.

If you place a file with `startup` in the filename in `/songs/`, SusOS will play it automatically on every boot.

---

## Commands

```
sus system info              System information
sus system ram               RAM usage with visual bar
sus system storage           Storage usage with visual bar
sus system cpu               CPU frequency
sus system temperature       Built-in temperature sensor reading

sus network scan             Scan for nearby WiFi networks (5 seconds)
sus network connect <ssid> <password> <attempts>
                             Connect to a WiFi network
sus network disconnect       Disconnect from WiFi
sus network ping <host>      Ping an IP address or URL (TCP/port 80)
sus network rename <name>    Set device hostname (persists across reboots)
sus network status           Show current network status and IP info

sus ble connect              Start BLE advertising (Nordic UART / LightBlue)
sus ble disconnect           Stop BLE
sus ble send <message>       Send a message via BLE UART
sus ble receive              Listen for incoming BLE messages (10 seconds)

sus sound play <filename>    Play an RTTTL file from /songs/
sus sound list               List all songs in /songs/
sus sound delete <filename>  Delete a song from /songs/

sus help                     Show all commands
```

Filenames with spaces are supported — type the full name including spaces.

---

## Terminal shortcuts

| Key | Action |
|-----|--------|
| `Enter` | Send command |
| `Up / Down` | Navigate command history |
| `Ctrl+C` | Interrupt current command |
| `Ctrl+L` | Clear the terminal |

---

## Notes

- **sus network ping** uses a TCP connection to port 80, not ICMP, because raw ICMP is unavailable in MicroPython without extra modules. It still gives a round-trip time.
- **BLE** uses the Nordic UART Service and is compatible with the LightBlue app (iOS/Android).
- The hostname set by `sus network rename` is saved to `/config.json` on the Pico W and reapplied automatically on every boot.
- Songs are stored in `/songs/` on the Pico W's flash. The `/songs/` folder is created automatically on first boot.
- Also, I haven't tried this on pico without wireless support or pico 2.
- Any feedback for future updates is recommended.
