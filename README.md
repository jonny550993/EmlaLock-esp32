# EmlaLock ESP32

A standalone ESP32 web dashboard for controlling and monitoring an EmlaLock session from a phone or computer on the local network.

## Features

- Wearer, Key Holder, and Alone modes.
- Live Time Passed and Time Left counters.
- Add or subtract session time where permitted.
- Responsive, dark-first dashboard with light and system theme options.
- Wi-Fi scanning, saved-network recovery, and local setup access point.
- Credentials stored locally in ESP32 NVS, not in the repository.
- Role changes locked until credentials are cleared.
- OTA firmware updates from this repository's GitHub releases.
- CI builds for all supported board targets.

## Supported boards

| Board | PlatformIO environment |
| --- | --- |
| ESP32 | `esp32dev` |
| ESP32-C3 | `esp32-c3-devkitm-1` |
| ESP32-S2 | `esp32-s2-saola-1` |
| ESP32-S3 | `esp32-s3-devkitc-1` |

## Setup

1. Flash the firmware for your board.
2. Connect to the `EmlaLock-Setup` Wi-Fi network using password `EmlaLock-Setup`.
3. Open `http://192.168.4.1`.
4. Select Wearer, Key Holder, or Alone.
5. Enter Wi-Fi and EmlaLock credentials.
6. Save and connect.

If the saved Wi-Fi is unavailable, the setup access point is started again for recovery. When editing settings, leaving a saved password or API-key field blank keeps the existing stored value.

## Build locally

Install [PlatformIO Core](https://platformio.org/install/cli), then run:

```bash
pio run -e esp32dev
pio run -e esp32-c3-devkitm-1
pio run -e esp32-s2-saola-1
pio run -e esp32-s3-devkitc-1
```

GitHub Actions builds all four environments on pushes and pull requests. Release builds are created manually from the Release workflow and attach one firmware binary per supported board.

## OTA updates

The device checks the latest release at:

`https://github.com/jonny550993/EmlaLock-esp32/releases/latest`

Release asset names must follow these patterns:

- `EmlaLock-X.Y.Z-esp32.bin`
- `EmlaLock-X.Y.Z-esp32c3.bin`
- `EmlaLock-X.Y.Z-esp32s2.bin`
- `EmlaLock-X.Y.Z-esp32s3.bin`

## Security notes

- EmlaLock credentials are stored locally in ESP32 NVS and are not committed to GitHub.
- The setup access point uses WPA2, but its default password is documented for first-time setup. Change the firmware constant before deployment if untrusted people could reach the setup network.
- HTTPS currently uses `WiFiClientSecure::setInsecure()` for compatibility. This disables certificate verification and is not suitable for hostile networks; certificate validation should be configured before using the device on untrusted networks.
- The local HTTP dashboard currently has no user authentication. Keep the device on a trusted private network and do not expose port 80 or the setup access point to the public internet.
- API credentials are currently sent to the EmlaLock API in query parameters to match the existing API integration. Avoid sharing device logs or network traces.

This project is not a certified security product. Review the deployment environment and hardware before use.

## EmlaLock API

API documentation: https://about.emlalock.com/docs/api/
