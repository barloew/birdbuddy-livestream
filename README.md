<img src="https://raw.githubusercontent.com/barloew/birdbuddy-livestream/main/custom_components/birdbuddy_livestream/brand/icon.png" alt="Bird Buddy Livestream" width="96" align="right">

# Bird Buddy Livestream

[![Release][release-badge]][release-url]
[![HACS][hacs-badge]][hacs-url]
[![Validate][validate-badge]][validate-url]

Live Bird Buddy camera feed in Home Assistant.

Adds a camera entity for each Bird Buddy feeder, so the live view can be shown on a dashboard, opened from a phone or started from an automation. For battery, signal and visitor data, use [ha-birdbuddy][ha-birdbuddy] alongside it.

## Features

- **Camera entity** per feeder with live video
- **Preview image** while nobody is watching: the latest visitor, another entity's picture or a fixed photo
- **Status sensor** and **prepare button** to wake the feeder ahead of time
- **Automatic stop timer** to protect the battery
- **Continuous mode** for feeders on permanent power, for recording with Frigate
- **go2rtc support** for playback in every browser and a stream that opens on the first click

## Requirements

- Home Assistant 2024.6 or later
- A Bird Buddy account with email and password. Accounts that sign in with Google or Facebook are not supported; invite a second, email-based account to the feeder instead.
- A Wi-Fi signal better than about −70 dBm at the feeder
- Recommended: [go2rtc][go2rtc]. Without it, video plays only in Safari.

## Installation

### HACS

[![Open in HACS][my-ha-badge]][my-ha-url]

Or add it manually:

1. In HACS, open the menu and select **Custom repositories**.
2. Add `https://github.com/barloew/birdbuddy-livestream` with category **Integration**.
3. Download **Bird Buddy Livestream** and restart Home Assistant.

### Manual

Copy `custom_components/birdbuddy_livestream` from the [latest release][release-url] to your `config/custom_components` folder and restart Home Assistant.

## Configuration

1. Go to **Settings → Devices & services → Add integration** and select **Bird Buddy Livestream**.
2. Enter your Bird Buddy email and password. A camera entity appears for each feeder.
3. Optional: open **Configure** and enter your go2rtc address, such as `http://192.168.1.10:1984`.

Click the camera card to start the stream. A sleeping feeder takes 20 to 60 seconds to wake up. Without go2rtc, the first click may time out; click again after half a minute.

Limitations:

- Bird Buddy allows one live session per account. Close the app before streaming in Home Assistant.
- The feeder does not record visits while streaming.
- Video runs 5 to 10 seconds behind real time.
- Newer feeders that stream over WebRTC are not supported yet.

## Documentation

- [Configuration](docs/configuration.md): all settings, preview images, go2rtc, continuous streaming with Frigate
- [Troubleshooting](docs/troubleshooting.md)
- [Advanced guide][advanced]: architecture, diagnostics, error reference

## Credits

Built on [pybirdbuddy][pybirdbuddy] and [ha-birdbuddy][ha-birdbuddy] by Joe Hansche, and [go2rtc][go2rtc] by AlexxIT.

## License

[MIT](LICENSE). Not affiliated with Bird Buddy. This integration uses the same private API as the Bird Buddy app and may break if Bird Buddy changes it.

[release-badge]: https://img.shields.io/github/v/release/barloew/birdbuddy-livestream
[release-url]: https://github.com/barloew/birdbuddy-livestream/releases
[hacs-badge]: https://img.shields.io/badge/HACS-Custom-41BDF5.svg
[hacs-url]: https://github.com/hacs/integration
[validate-badge]: https://github.com/barloew/birdbuddy-livestream/actions/workflows/validate.yml/badge.svg
[validate-url]: https://github.com/barloew/birdbuddy-livestream/actions/workflows/validate.yml
[my-ha-badge]: https://my.home-assistant.io/badges/hacs_repository.svg
[my-ha-url]: https://my.home-assistant.io/redirect/hacs_repository/?owner=barloew&repository=birdbuddy-livestream&category=integration
[advanced]: docs/advanced.md
[ha-birdbuddy]: https://github.com/jhansche/ha-birdbuddy
[pybirdbuddy]: https://github.com/jhansche/pybirdbuddy
[go2rtc]: https://github.com/AlexxIT/go2rtc
