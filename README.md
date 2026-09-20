NWEQ JMA Live

NWEQ JMA Live displays Japanese earthquake information and Level 5 special warnings as transparent broadcast-style overlays in OBS Studio.

The receiver polls the public [Japan Meteorological Agency disaster-information XML feeds](https://www.data.jma.go.jp/developer/xml/feed/) and serves two local browser-source pages. No OBS plugin installation is required.

## Features

- JMA earthquake information from `eqvol.xml`
- JMA Level 5 special warnings from `extra.xml`
- Transparent 1920 × 1080 OBS overlays
- NWEQ broadcast-style Japanese typography
- Alert sound when a new matching bulletin arrives
- Local-only web server at `127.0.0.1:8765`
- No changes to existing OBS scenes or configuration

## Requirements

- Windows 10 or Windows 11
- OBS Studio with Browser Source support
- PowerShell 5.1 or later
- An internet connection for receiving JMA bulletins

## Installation

1. Download and extract the complete package.
2. Double-click `START-NWEQ-JMA.cmd`.
3. Keep the NWEQ receiver window open while using the overlays.
4. In OBS, add a **Browser** source for each overlay you want.

Use these browser-source settings:

| Overlay | URL | Width | Height |
| --- | --- | ---: | ---: |
| Earthquake information | `http://127.0.0.1:8765/earthquake` | 1920 | 1080 |
| Level 5 special warning | `http://127.0.0.1:8765/level5` | 1920 | 1080 |

Enable **Control audio via OBS** if you want the alert sound on a separate OBS Audio Mixer channel.

## Custom alert sound

The included sound is `alert.wav`. To replace it:

1. Stop the NWEQ receiver.
2. Replace `alert.wav` with another short WAV file.
3. Keep the filename exactly `alert.wav`.
4. Restart `START-NWEQ-JMA.cmd` and refresh the OBS Browser sources.

The sound plays only when a new matching bulletin arrives after the receiver starts. Previously published bulletins do not trigger the sound on startup.

## Supported information

### Earthquake information

The earthquake overlay watches JMA earthquake and seismic-intensity bulletins. It can display the official headline, hypocenter, magnitude, and maximum observed intensity when those fields are available.

### Level 5 weather warnings

The weather overlay displays JMA bulletins whose official warning names contain `レベル５` or `特別警報`.

JMA Level 5 special warnings and a municipality's `緊急安全確保` evacuation notice are related but distinct information. NWEQ displays the wording contained in the JMA bulletin and does not convert it into a municipal evacuation notice.

### General breaking news

JMA does not publish general editorial breaking news such as political, criminal, or business stories. The NWEQ `ニュース速報` test overlay is therefore separate from this JMA receiver.

## How it works

`jma-server.ps1` polls these public Atom feeds every 30 seconds:

- `https://www.data.jma.go.jp/developer/xml/feed/eqvol.xml`
- `https://www.data.jma.go.jp/developer/xml/feed/extra.xml`

It retrieves matching JMA XML bulletins, extracts the fields needed by the overlays, and serves the resulting state locally. The local server does not accept connections from other computers.

## Troubleshooting

### OBS shows a blank page

- Confirm that `START-NWEQ-JMA.cmd` is still running.
- Confirm that the Browser source URL begins with `http://127.0.0.1:8765/`.
- Refresh the Browser source cache in OBS.
- Make sure another application is not already using port `8765`.

### No alert is visible

The live pages wait for a matching JMA bulletin. If no current matching bulletin exists, the page remains in its waiting state.

### No sound is heard

- Enable **Control audio via OBS** in the Browser source properties.
- Check that the source is visible in the OBS Audio Mixer.
- Confirm that `alert.wav` exists beside `jma-server.ps1`.
- Check the source's mixer volume and monitoring settings.

## Data source and limitations

Weather and earthquake data is provided by the Japan Meteorological Agency. Review the [JMA weather-data portal](https://www.data.jma.go.jp/developer/) and applicable terms before redistribution.

JMA notes that its public pull service may stop or be delayed during maintenance or other disruptions. It is not a guaranteed emergency-delivery service. Always confirm critical information through official emergency channels.

NWEQ JMA Live is an independent display tool and is not affiliated with or endorsed by the Japan Meteorological Agency or NHK.
