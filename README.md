# SmartDisplay (ESPHome + ESP32-2432S028R)

Simple touch panel for Home Assistant on the **ESP32-2432S028R (2.8” ILI9341 + XPT2046)**.
Shows date/time and four round buttons that toggle your HA entities directly.

![SmartDisplay](images/screenshot.jpg)

## Features
- 2.8” 320×240 TFT (ILI9341) with resistive touch (XPT2046)
- Four on-screen buttons (circles) with Material Design Icons
- Button press triggers HA services (switch/fan/light) via ESPHome API
- Button fill color reflects the HA entity state (via HA text sensors)
- Dutch day/month names & 24-hour time

## Hardware
- **Board**: ESP32-2432S028R (2.8″ TFT + touch)
  - Buy link (NL): <https://www.tinytronics.nl/nl/development-boards/microcontroller-boards/met-wi-fi/jingcai-esp32-2432s028r-2.8-inch-tft-lcd-display-320*240-pixels-met-resistief-touchscreen-esp32>

## 3D Printed Case
STL files (external): <https://www.thingiverse.com/thing:6829981>

> Note: This repository does not include STL files—download them from Thingiverse.

## Folder Structure
```
.
├── LICENSE
├── README.md
├── .gitignore
├── secrets.example.yaml        # copy to secrets.yaml and fill in
├── esphome/
│   └── smartdisplay.yaml       # main ESPHome config
├── fonts/
│   └── materialdesignicons-webfont.ttf (place here, not committed)
└── images/
    └── .gitkeep                # add your own photos/screenshots
```

## Setup (ESPHome)
1. Copy `secrets.example.yaml` to **`secrets.yaml`** and fill Wi‑Fi credentials.
2. Download MDI webfont TTF and place it at **`fonts/materialdesignicons-webfont.ttf`**  
   - TTF source: Material Design Icons Webfont (Templarian).
3. Open `esphome/smartdisplay.yaml` and adjust the **entity IDs** to your Home Assistant setup:
   - `switch.infrarood_bureau`
   - `fan.pwm_fan_867b6c_pwm_fan`
   - `light.gamekamer`
   - `light.bureau`
4. (Optional but recommended) Set Wi‑Fi power save off:
   ```yaml
   wifi:
     power_save_mode: none
   ```
5. (Optional) Add API encryption (remember to paste the same key in HA when adding the device):
   ```yaml
   api:
     encryption:
       key: "YOUR_32_BYTE_BASE64_KEY"
     reboot_timeout: 0s
   ```
6. Compile & flash from ESPHome.

## Touch Calibration
The config includes a working calibration for XPT2046 and aligns hot-zones with drawn circles.  
If your touch areas feel offset, tweak the `touchscreen.calibration` and/or `transform` settings.

## Troubleshooting: Rapid connect/disconnect in logs
- Make sure API encryption **matches** on both sides (either both encrypted with the same key, or both unencrypted).
- Disable ESP32 Wi‑Fi power save: `power_save_mode: none`.
- Add device to HA **once** by **IP** (avoid duplicate entries).
- Close ESPHome Dashboard live logs (they open extra API sessions).
- Optional logger tuning:
  ```yaml
  logger:
    level: INFO
    logs:
      api.connection: ERROR
      time: WARN
  ```

## License
MIT. See `LICENSE`.

---

_If you build the case, please star the Thingiverse model and this repo!_
