# Plexamp Touchscreen Music Player

Complete setup guide for building a dedicated Plexamp music player using a Raspberry Pi 4 with a touchscreen interface and high-quality digital audio output to an AV receiver.

## What You'll Get

- Full Plexamp UI on a 7" touchscreen
- Touch controls for browsing and playback
- Optical (TOSLINK) digital audio output to your AVR
- Access to all Plex playlists and music library
- Auto-start on boot - just power on and play

---

## Hardware Requirements

Gather the following components before starting. All items can be purchased from Amazon, Adafruit, or the official Raspberry Pi store.

| Component | Details |
|-----------|---------|
| **Raspberry Pi 4 Model B** | 4GB RAM version recommended (2GB minimum). Available from raspberrypi.com or authorized retailers. |
| **MicroSD Card** | 64GB or larger, Class 10 or A1/A2 rated. SanDisk Ultra or Samsung EVO are reliable choices. |
| **HiFiBerry Digi+ Standard** | S/PDIF digital output HAT with optical TOSLINK and coaxial outputs. Purchase from hifiberry.com or Amazon. |
| **7" Touchscreen Display** | Any HDMI touchscreen with USB touch input. Official Raspberry Pi 7" display or third-party alternatives work. |
| **TOSLINK Optical Cable** | Fiber optic audio cable. Length depends on your setup - measure distance from Pi location to AVR. |
| **USB-C Power Supply** | Official Raspberry Pi 4 power supply (5.1V 3A) recommended. Third-party supplies must provide adequate power. |
| **Micro HDMI Cable** | Micro HDMI (Pi end) to standard HDMI (display end). Pi 4 uses micro HDMI, not full-size. |
| **USB Keyboard** | Required for initial setup and Plexamp login. Keep connected until touchscreen is verified working. |
| **Ethernet Cable** | Highly recommended. WiFi works but ethernet is more stable and reliable for streaming audio. |
| **MicroSD Card Reader** | To flash the SD card from your computer. Many laptops have built-in readers. |

---

## Software Requirements

- **Plex Pass subscription** - Required for Plexamp headless. Subscribe at plex.tv
- **Plex Media Server** - Must be running on your network with a Music library configured
- **Raspberry Pi Imager** - Download from [raspberrypi.com/software](https://raspberrypi.com/software) (available for Windows, Mac, Linux)
- **SSH client** - For Windows: Download PuTTY from [putty.org](https://putty.org), or use Windows Terminal/PowerShell with built-in SSH. Mac/Linux have SSH built into Terminal.

---

## Plex Media Server Setup

Before setting up the Raspberry Pi, ensure your Plex Media Server is properly configured.

### Music Library Requirements

- Plex Media Server must be running on your network (can be on a NAS, PC, or server)
- At least one Music library must be configured and populated
- Music files should have proper metadata (artist, album, track names)
- Supported formats: FLAC, MP3, AAC, ALAC, and other common audio formats

### Recommended Folder Structure

For best results, organize your music library as:

```
Music/
  Artist Name/
    Album Name/
      01 - Track Name.flac
      02 - Track Name.flac
```

### Creating Playlists in Plex

You can create playlists in the Plex web interface that will be available in Plexamp:

1. Go to your Plex server web UI
2. Browse to a track or album
3. Click the three dots menu → Add to Playlist
4. Create a new playlist or add to an existing one
5. These playlists will automatically appear in Plexamp

---

## Hardware Assembly

### Installing the HiFiBerry Digi+ on the Raspberry Pi

The HiFiBerry Digi+ connects directly to the Raspberry Pi's 40-pin GPIO header. From the official HiFiBerry documentation:

1. **Power off the Raspberry Pi** and disconnect all cables before installation
2. Locate the 40-pin GPIO header on the Raspberry Pi (the double row of pins)
3. Align the HiFiBerry Digi+ connector with the GPIO header - the board should sit directly above the Pi
4. Gently but firmly press the HiFiBerry board onto the GPIO pins until fully seated
5. Ensure all pins are properly inserted and the board is level

**Important notes from HiFiBerry:**

- No soldering is required - the board simply plugs onto the GPIO header
- No additional power supply is needed - the Digi+ is powered by the Raspberry Pi
- The Digi+ uses GPIOs 2-3 (pins 3 and 5) for configuration and GPIOs 18-21 (pins 12, 35, 38, 40) for the sound interface
- GPIO 16 is also reserved by the Digi+
- Compatible with all Raspberry Pi models with the 40-pin GPIO header (Pi 3, Pi 4, Pi 5)

### Connecting the Touchscreen

1. Connect the HDMI cable from your touchscreen to the Raspberry Pi's HDMI port
2. For Pi 4: Use the micro HDMI port closest to the USB-C power port (HDMI0)
3. Connect the USB cable from the touchscreen to any USB port on the Pi (this enables touch input)
4. Connect power to the touchscreen if it has a separate power input

### Final Assembly

- Insert the flashed microSD card into the Raspberry Pi's card slot
- Connect ethernet cable (recommended) or configure WiFi
- Connect the TOSLINK optical cable from HiFiBerry Digi+ to your AVR
- Connect the USB-C power supply to the Raspberry Pi last

---

## AVR Configuration

Configure your AVR (Audio/Video Receiver) to receive the optical input from the HiFiBerry.

### Physical Connection

1. Locate an available optical (TOSLINK) input on your AVR
2. Common input labels: AUX1, AUX2, CD, DVD, GAME, CBL/SAT, or OPTICAL
3. Connect the TOSLINK optical cable from the HiFiBerry Digi+ optical output to your chosen AVR input
4. **Important:** Note which input you connected to - you must select this same input on your AVR to hear audio

### AVR Input Selection

1. Power on your AVR
2. Using your AVR remote or front panel SOURCE SELECT, select the input source where you connected the optical cable
3. For example: If you connected to the optical input labeled "CD", select CD as your input source
4. The AVR display should show the selected input name

### Denon AVR Input Assignment (if needed)

On Denon receivers, you may need to assign the digital input to your source. From the Denon documentation:

1. Press SETUP on your remote to access the setup menu
2. Navigate to: Input Assign (or Source Setup)
3. Select the input source you want to use (e.g., AUX1, CD, etc.)
4. Under "DIGITAL" assignment, select the optical input where your cable is connected
5. **INPUT MODE:** Set to "AUTO" (recommended) or "DIGITAL" to ensure digital audio is used

> **Note:** When INPUT MODE is set to AUTO, the AVR automatically detects and plays signals in this priority order: HDMI → DIGITAL → ANALOG

### Audio Mode for Music Playback

For best music quality, set your AVR sound mode to one of the following:

- **Stereo:** Standard 2-channel playback
- **Direct:** Bypasses tone controls for purest sound
- **Pure Direct:** Bypasses all processing and turns off display for lowest noise
- Avoid surround modes (Dolby, DTS) for music unless desired

### Verifying the Connection

When Plexamp is playing audio, your AVR should:

- Display "PCM" or "Stereo" as the incoming signal format
- Show sample rate information (e.g., 44.1kHz, 48kHz, or 96kHz)
- The digital signal indicator should light up on the AVR display
- **If no audio:** Verify the optical cable is fully seated, check input selection, and confirm Input Assign settings

---

## Step 1: Flash Raspberry Pi OS

1. Download and install Raspberry Pi Imager from [raspberrypi.com/software](https://raspberrypi.com/software)
2. Insert your microSD card into your computer
3. Open Raspberry Pi Imager
4. **Choose Device:** Raspberry Pi 4
5. **Choose OS:** Raspberry Pi OS (64-bit) with Desktop
6. **Choose Storage:** Select your microSD card
7. Click the gear icon (⚙️) to configure advanced settings:
   - Set hostname: `plexamp`
   - Enable SSH with password authentication
   - Set username and password
   - Configure WiFi (if not using ethernet)
   - Set your timezone
8. Click Write and wait for completion

---

## Step 2: Initial Boot and Update

1. Insert the microSD card into your Raspberry Pi
2. Connect the HiFiBerry Digi+ HAT to the GPIO pins (see Hardware Assembly section)
3. Connect the touchscreen via HDMI and USB (for touch input)
4. **Connect a USB keyboard** - needed to find IP address and for initial setup if touchscreen has issues
5. Connect network:
   - **Ethernet (Recommended):** Plug ethernet cable directly into the Pi. More stable for audio streaming.
   - **WiFi:** Works fine if ethernet isn't practical. Make sure you configured WiFi in Step 1 (Raspberry Pi Imager settings).
6. Connect the USB-C power supply to boot the Pi
7. Wait 1-2 minutes for the first boot to complete. The desktop should appear on the touchscreen.

> **Note:** If the touchscreen doesn't display anything, check HDMI connection. Use the USB keyboard to navigate if touch isn't responding initially.

### Find Your Pi's IP Address

You need the IP address to connect via SSH. Here are several ways to find it:

#### Method 1: On the Raspberry Pi (Recommended)

With a USB keyboard connected to the Pi:

- Click the network icon in the top-right corner of the Pi desktop (looks like two arrows or WiFi symbol)
- Hover over your connection - the IP address will be displayed
- Or open Terminal on the Pi (click the terminal icon in the taskbar) and type:

```bash
hostname -I
```

This displays the IP address (e.g., 192.168.1.100 or 10.0.0.120)

#### Method 2: From Your Router

- Log into your router's admin page (usually 192.168.1.1 or 192.168.0.1)
- Look for "Connected Devices", "DHCP Clients", or "Network Map"
- Find the device named "plexamp" (the hostname you set)

#### Method 3: Ping Command

From your Windows/Mac/Linux computer, open a terminal and type:

```bash
ping plexamp.local
```

This may work on some networks. If it responds, the IP will be shown.

### Connect via SSH

**Windows (using PowerShell or Command Prompt):**

```bash
ssh yourusername@192.168.1.xxx
```

Replace `yourusername` with the username you set in Raspberry Pi Imager, and the IP with your Pi's actual IP address. Type `yes` when asked about the fingerprint, then enter your password.

**Windows (using PuTTY):**

1. Open PuTTY
2. Enter the Pi's IP address in the "Host Name" field
3. Click "Open"
4. Login with your username and password

### Update the System

Once connected via SSH, run these commands to update all system packages:

```bash
sudo apt update && sudo apt upgrade -y
```

This may take several minutes. Wait for it to complete before continuing.

---

## Step 3: Configure HiFiBerry Digi+

1. Edit the boot configuration:

```bash
sudo nano /boot/firmware/config.txt
```

2. Find and comment out the onboard audio line by adding `#` in front:

```
#dtparam=audio=on
```

3. Add the HiFiBerry overlay at the bottom of the file:

```
# HiFiBerry Digi+
dtoverlay=hifiberry-digi
```

4. Save and exit (Ctrl+X, Y, Enter)

5. Reboot:

```bash
sudo reboot
```

6. After reboot, SSH back in and verify the HiFiBerry is recognized:

```bash
aplay -l
```

You should see `snd_rpi_hifiberry_digi` listed as a sound card.

---

## Step 4: Install Node.js

Plexamp requires Node.js 20. Install it:

```bash
sudo apt install -y nodejs npm
```

Verify the installation:

```bash
node -v
```

You should see v20.x.x or similar.

---

## Step 5: Install Plexamp Headless

Plexamp headless is available from Plex and requires a Plex Pass subscription.

### Download and Extract

Run these commands in your SSH session:

```bash
cd ~
curl -O https://plexamp.plex.tv/headless/Plexamp-Linux-headless-v4.12.4.tar.bz2
tar -xvf Plexamp-Linux-headless-v4.12.4.tar.bz2
```

> **Note:** Check [plex.tv/plexamp](https://plex.tv/plexamp) for the latest version number. Replace the version in the commands if newer is available.

### Claim Your Player

1. Start Plexamp for the first time:

```bash
node ~/plexamp/js/index.js
```

2. You will see a message asking for a claim token
3. On your computer or phone, open a web browser and go to:

```
https://plex.tv/claim
```

4. Log in with your Plex account if prompted
5. Copy the claim token shown on the page (it looks like: `claim-xxxxxxxxxxxx`)
6. Paste the token into your SSH terminal and press Enter
7. When asked for a player name, enter something descriptive like "Living Room" or "HT-Plexamp"
8. Plexamp will start. You should see "Starting Plexamp" followed by version info
9. Press Ctrl+C to stop Plexamp (we'll set it up as a service next)

---

## Step 6: Configure Plexamp as a Service

1. Create the systemd user directory:

```bash
mkdir -p ~/.config/systemd/user
```

2. Create the service file:

```bash
nano ~/.config/systemd/user/plexamp.service
```

3. Paste the following (replace `YOUR_USERNAME` with your actual username):

```ini
[Unit]
Description=Plexamp
After=default.target

[Service]
Type=simple
ExecStart=/usr/bin/node /home/YOUR_USERNAME/plexamp/js/index.js
Restart=on-failure
RestartSec=10

[Install]
WantedBy=default.target
```

4. Save and exit (Ctrl+X, Y, Enter)

5. Enable and start the service:

```bash
systemctl --user daemon-reload
systemctl --user enable plexamp
systemctl --user start plexamp
```

6. Enable user services to run at boot:

```bash
sudo loginctl enable-linger $USER
```

7. Verify it's running:

```bash
systemctl --user status plexamp
```

---

## Step 7: Configure Plexamp

Access the Plexamp web interface from another computer on your network.

1. Open a web browser and go to `http://<PI_IP_ADDRESS>:32500`
2. You'll see the Plexamp welcome screen

### Sign In to Plex

- Click to sign in with your Plex account
- Enter your Plex username/email and password
- Complete any two-factor authentication if enabled

### Select Music Library Source

- After login, you'll be prompted to "Select a library"
- Choose your Music library from your Plex Media Server
- This connects Plexamp to your music collection on your Plex server
- Click "Continue"

### Configure Audio Output

- Click the Settings gear icon (bottom right corner)
- Navigate to: Playback → Audio Output → Audio Device
- You'll see a list of available audio outputs:
  - Default
  - Default Audio Device
  - snd_rpi_hifiberry_digi: HifiBerry Digi HiFi wm8804-spdif-0
  - vc4-hdmi-0 (HDMI audio)
  - vc4-hdmi-1 (HDMI audio)
- **Select: snd_rpi_hifiberry_digi: HifiBerry Digi HiFi wm8804-spdif-0**

This routes audio through the optical output to your AVR.

### Test Audio Playback

- Make sure your AVR is powered on and set to the correct optical input (e.g., AUX1)
- Browse to an album or playlist in Plexamp
- Play a track
- You should hear audio through your AVR/speakers
- If no audio, verify the AVR shows it's receiving a digital signal

### Optional Plexamp Settings

While in Settings, you may want to configure:

- **Playback → Loudness Leveling:** Normalizes volume between tracks
- **Playback → Gapless Playback:** Enable for seamless album playback
- **Playback → Remote Control:** Shows the IP and port for remote control

---

## Step 8: Configure Touchscreen Kiosk Mode

1. Install required packages:

```bash
sudo apt install -y chromium unclutter
```

2. Create the autostart directory:

```bash
mkdir -p ~/.config/autostart
```

3. Create the Plexamp kiosk autostart file:

```bash
nano ~/.config/autostart/plexamp-kiosk.desktop
```

4. Paste the following:

```ini
[Desktop Entry]
Type=Application
Name=Plexamp Kiosk
Exec=bash -c "sleep 15 && /usr/bin/chromium --kiosk --noerrdialogs --disable-infobars --no-first-run --start-fullscreen --password-store=basic http://localhost:32500"
X-GNOME-Autostart-enabled=true
```

> **Note:** The 15-second delay (sleep 15) gives Plexamp time to start before Chromium tries to connect. The `--password-store=basic` flag prevents keyring password prompts.

5. Save and exit (Ctrl+X, Y, Enter)

6. Create the unclutter autostart (hides mouse cursor):

```bash
nano ~/.config/autostart/unclutter.desktop
```

7. Paste the following:

```ini
[Desktop Entry]
Type=Application
Name=Unclutter
Exec=unclutter -idle 0.5 -root
X-GNOME-Autostart-enabled=true
```

8. Save and exit

9. Reboot to test:

```bash
sudo reboot
```

---

## Step 9: First Touchscreen Login

After reboot, Chromium will launch in kiosk mode showing the Plexamp welcome screen. You need to log in once:

1. Connect a USB keyboard to the Pi temporarily
2. Use Tab and Enter keys to navigate through the Plex login
3. Enter your Plex credentials
4. Select your Music library
5. Once logged in, remove the keyboard - touch controls will work

---

## Useful Commands

### Check Service Status

```bash
systemctl --user status plexamp
```

### Restart Plexamp

```bash
systemctl --user restart plexamp
```

### Safe Shutdown

```bash
sudo shutdown -h now
```

### Reboot

```bash
sudo reboot
```

### Check Audio Devices

```bash
aplay -l
```

---

## Troubleshooting

### No Audio

- Verify HiFiBerry is detected: `aplay -l`
- Check Plexamp audio settings are set to HiFiBerry
- Verify AVR is set to correct input and detecting digital signal
- Check TOSLINK cable is properly connected

### Plexamp Not Loading on Touchscreen

- Check Plexamp service: `systemctl --user status plexamp`
- Try accessing `http://<IP>:32500` from another device
- The 15-second delay in the autostart allows Plexamp to start first

### Plexamp Shows Welcome Screen Instead of Library

If Chromium shows the welcome/login screen instead of your authenticated session after reboot, clear the Chromium cache:

```bash
pkill chromium
rm -rf ~/.cache/chromium
sudo reboot
```

After reboot, log in again with the USB keyboard. The session should persist after this.

### Touch Not Working

- Ensure USB cable from touchscreen is connected to Pi
- Some displays need specific drivers - check manufacturer documentation

### Plexamp Token Expired

- If offline for 30+ days, you may need to re-claim the player
- Stop service, run manually with `node ~/plexamp/js/index.js`, and enter new claim token

---

## Optional: Physical Power Button

You can add a physical button to safely shutdown and power on the Pi.

### Hardware Needed

- Momentary push button (normally open)
- Two jumper wires

### Wiring

- Connect one leg of button to GPIO 3 (Pin 5)
- Connect other leg to Ground (Pin 6)

GPIO 3 is special - it can shutdown the Pi when pressed, and also wake it from halt state.

### Configuration

Add to `/boot/firmware/config.txt`:

```
# Shutdown button on GPIO 3
dtoverlay=gpio-shutdown,gpio_pin=3
```

---

## Final Notes

- Always use safe shutdown (`sudo shutdown -h now`) rather than pulling power to avoid SD card corruption
- Keep your Plex Pass subscription active for Plexamp headless to continue working
- The touchscreen will not burn in under normal use - modern LCDs are resistant to image retention
- Plexamp updates can be downloaded from plex.tv and extracted over the existing installation

---

## License

This guide is provided as-is for personal use. Plexamp is a product of Plex, Inc. and requires a Plex Pass subscription.
