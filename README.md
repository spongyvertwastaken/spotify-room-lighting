# Spotify Room Lighting

Spotify Room Lighting is a Windows desktop app that syncs Smart Life/Tuya room lights to the album artwork of the Spotify song currently playing.

This is a simple guide to install the app.

## What You Need

- A Windows PC.
- A Spotify account.
- Smart Life/Tuya lights already added in the Smart Life app.
- The installer file:

```text
Spotify Room Lighting_0.2.4_x64-setup.exe
```

## Install The App

run the setup.exe file

On launch you will see a dark screen with a spinner ("Starting Spotify Room Lighting") for a few seconds while the app and its bundled local backend start up. This is normal. If you see `backend loading` after that, give it another moment.

## Where Settings Are Saved

Settings are saved locally on your PC:

```text
%APPDATA%\SpotifyRoomLighting
```

This is where Spotify tokens and app settings live. 

## Step 1: Create Spotify Credentials

1. Go to the Spotify Developer Dashboard:

```text
https://developer.spotify.com/dashboard
```

2. Log in.
3. Click Create app.
4. Fill the app name and description with anything you want.
5. For Redirect URI, add exactly:

```text
http://127.0.0.1:8000/callback
```

6. Save the app.
7. Open the app you just created.
8. Copy the Client ID.
9. Click View client secret, then copy the Client Secret.

## Step 2: Add Spotify To The App

1. Open Spotify Room Lighting.
2. Click the gear icon in the top right.
3. Paste:

```text
Spotify Client ID
Spotify Client Secret
Redirect URI: http://127.0.0.1:8000/callback
```

4. Click Save.
5. Click Reconnect Spotify.
6. A browser tab opens.
7. Approve the Spotify login.
8. Return to the desktop app.
9. Play a song in Spotify.

The album art, song name, artist, and background colors should update automatically.

Spotify permissions used:

```text
user-read-currently-playing
user-read-playback-state
user-modify-playback-state
```

These allow now-playing display plus play, pause, previous, next, and volume control.

## Step 3: Create Tuya Cloud Credentials

Use this for Smart Life/Tuya lights. You do not need local keys or bulb IP addresses.

1. Go to Tuya IoT Developer Platform:

```text
https://iot.tuya.com
```

2. Log in with the account connected to your Smart Life setup.
3. Go to Cloud.
4. Create a Cloud Project.
5. Use:

```text
Industry: Smart Home
Development Method: Smart Home
Data Center: Western America Data Center
```

For most US/Canada Smart Life accounts, the app region in Spotify Room Lighting should be:

```text
us
```

the others can find their data center location here:

```text
https://developer.tuya.com/en/docs/iot/oem-app-data-center-distributed?id=Kafi0ku9l07qb
```

6. Open your project.
7. On the Overview tab, copy:

```text
Access ID / Client ID
Access Secret / Client Secret
```

8. Go to Service API.
9. Make sure these services are authorized:

```text
IoT Core
Authorization Token Management
Smart Home Basic Service
```

10. Go to Devices.
11. Link your Smart Life app account if you have not already.
12. Confirm your bulbs appear online.
13. Change Device Permission says Controllable.

## Step 4: Add Tuya To The App

1. Open Spotify Room Lighting.
2. Click the gear icon.
3. Fill:

```text
Tuya Access ID
Tuya Access Secret
Tuya Region: YOUR_REGION
Default Light Adapter: tuya-cloud
Smart Lights: enabled
```

4. In Light Devices JSON, add your bulbs.

Example:

```json
[
  {
    "name": "top 1",
    "type": "bulb",
    "brand": "HiFiLUZ",
    "adapter": "tuya-cloud",
    "device_id": "DEVICE_ID_HERE",
    "enabled": true,
    "palette_index": 0
  },
  {
    "name": "top 2",
    "type": "bulb",
    "brand": "HiFiLUZ", 
    "adapter": "tuya-cloud",
    "device_id": "DEVICE_ID_HERE",
    "enabled": true,
    "palette_index": 1
  }
]
```

5. Click Save.
6. Click Test Lights.
7. Play a Spotify song.
8. Turn on the light toggle in the top right of the app.

The lights should change to colors from the current album artwork.

## Example Six-Bulb Layout

Replace each `device_id` with the device ID from the Tuya Devices page:

```json
[
  {
    "name": "top 3",
    "type": "bulb",
    "brand": "HiFiLUZ",
    "adapter": "tuya-cloud",
    "device_id": "DEVICE_ID_HERE",
    "enabled": true,
    "palette_index": 0
  },
  {
    "name": "top 2",
    "type": "bulb",
    "brand": "HiFiLUZ",
    "adapter": "tuya-cloud",
    "device_id": "DEVICE_ID_HERE",
    "enabled": true,
    "palette_index": 1
  },
  {
    "name": "bottom 1",
    "type": "bulb",
    "brand": "HiFiLUZ",
    "adapter": "tuya-cloud",
    "device_id": "DEVICE_ID_HERE",
    "enabled": true,
    "palette_index": 2
  },
  {
    "name": "top 1",
    "type": "bulb",
    "brand": "HiFiLUZ",
    "adapter": "tuya-cloud",
    "device_id": "DEVICE_ID_HERE",
    "enabled": true,
    "palette_index": 0
  },
  {
    "name": "bottom 3",
    "type": "bulb",
    "brand": "HiFiLUZ",
    "adapter": "tuya-cloud",
    "device_id": "DEVICE_ID_HERE",
    "enabled": true,
    "palette_index": 1
  },
  {
    "name": "bottom 2",
    "type": "bulb",
    "brand": "HiFiLUZ",
    "adapter": "tuya-cloud",
    "device_id": "DEVICE_ID_HERE",
    "enabled": true,
    "palette_index": 2
  }
]
```

## Brightness And Speed Settings

Brightness Min:

```text
Lowest random brightness the app will use.
```

Brightness Max:

```text
Highest random brightness the app will use.
```

Update Speed:

```text
Minimum spacing between Tuya light commands.
```

Good starting values:

```text
Brightness Min: 20
Brightness Max: 85
Update Speed: 350
```

Lower update speed can feel faster, but may annoy Tuya Cloud or make commands fail. Keep it around `300-500` unless you know your setup can handle more.

## Ambient Animation

By default the lights do not just hold one color per song. Each bulb gently
fades and cycles through the current album's colors, offset from the other
bulbs so the colors drift across the room, with a soft brightness "breath".
When you skip to a new song the lights jump straight to that song's colors, then
keep drifting. When playback is paused or stopped, the lights keep a calmer,
dimmer drift going.

In Settings:

```text
Breathing / cycle: turn the animation on or off (off = one steady color per bulb)
Speed: slow / medium / fast
Idle drift: keep drifting while paused or stopped
Transition (ms): how smooth the color fades are (0 = instant changes)
```

Slower speeds and fewer bulbs are gentler on Tuya Cloud. Very fast speeds with
many bulbs send a lot of commands and may be throttled.

## Tuya Data Center Error 28841107

If Test Lights shows:

```text
28841107: No permission. The data center is suspended.
```

Do this Tuya-side reset:

1. Open your Tuya Cloud project.
2. Go to Overview.
3. Click Edit.
4. In Data Center, remove the selected data center with the `X`.
5. Re-select the same data center immediately.
6. Save.
7. If Tuya asks to suspend/enable the data center, complete the email verification flow.
8. Make sure the same data center is enabled again.
9. Wait 30-60 seconds.
10. Refresh Tuya.
11. Restart Spotify Room Lighting.
12. Try Test Lights again.

Do not leave the project with no data center selected. The trick is basically turning the same Tuya data center off and on again.

Also check:

- Device Permission must say Controllable.
- Your Tuya Access ID/Secret must come from the same project as the devices.
- Region in the app must match the data center.
- For Western America Data Center, use `us`.
- Cloud Authorization IP Allowlist should be off unless your public IP is added.
- Devices must be online in Smart Life.

## Common Issues

Backend loading:

```text
The bundled backend is starting. Wait a few seconds.
```

Backend offline:

```text
The local backend did not start or port 8000 is blocked.
Close and reopen the app.
```

Spotify connected but nothing shows:

```text
Start playback in Spotify and make sure there is an active Spotify device.
```

Reconnect Spotify opens a browser:

```text
That is expected. Approve the login, then return to the app.
```

Lights turn off but do not change color:

```text
Check that each device is Controllable in Tuya and that the device IDs are correct.
```

Lights change one by one:

```text
That is normal for Tuya Cloud. The app sends commands per device.
```

Windows warns about the installer:

```text
The installer is unsigned. This is normal for a homemade build.
```

The setup file installs the desktop app in the correct layout with the bundled backend.

Each person needs their own Spotify Developer app credentials and Tuya Cloud credentials, because those are saved locally on their own PC.

## What The App Is Built With

- Desktop shell: Tauri v2.
- Frontend: React, Vite, TypeScript.
- Backend: bundled Python FastAPI sidecar.
- Spotify: Spotify Web API OAuth and player endpoints.
- Album colors: Pillow-based image palette extraction.
- Smart lights: Tuya Cloud API for Smart Life/Tuya devices.
- Installer: Windows NSIS installer from Tauri.

