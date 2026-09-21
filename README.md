NWEQ JMA Live

NWEQ JMA Live displays Japanese earthquake information and Level 5 special warnings as transparent broadcast-style overlays in OBS Studio.

The browser-source pages read the public [Japan Meteorological Agency disaster-information XML feeds](https://www.data.jma.go.jp/developer/xml/feed/) directly. No OBS plugin installation or local server is required.

## Features

- JMA earthquake information from `eqvol.xml`
- JMA Level 5 special warnings from `extra.xml`
- Transparent 1920 × 1080 OBS overlays
- NWEQ broadcast-style Japanese typography
- Alert sound when a new matching bulletin arrives
- No changes to existing OBS scenes or configuration

## Requirements

- Windows 10 or Windows 11
- OBS Studio with Browser Source support
- An internet connection for receiving JMA bulletins

## Installation

1. Download and extract the complete package.
2. In OBS, add a **Browser** source for each overlay you want.
3. Enable **Local file** and choose the matching HTML file.

Use these browser-source settings:

| Overlay | Local file | Width | Height |
| --- | --- | ---: | ---: |
| Earthquake information | `earthquake.html` | 1920 | 1080 |
| Level 5 special warning | `level5.html` | 1920 | 1080 |

Enable **Control audio via OBS** if you want the alert sound on a separate OBS Audio Mixer channel.

## Custom alert sound

The included sound is `alert.wav`. To replace it:

1. Replace `alert.wav` with another short WAV file.
2. Keep the filename exactly `alert.wav`.
3. Refresh the OBS Browser sources.

The sound plays only when a new matching bulletin arrives after the browser source starts. The bulletin already displayed at startup does not trigger the sound.

## Supported information

### Earthquake information

The earthquake overlay watches JMA earthquake and seismic-intensity bulletins. It can display the official headline, hypocenter, magnitude, and maximum observed intensity when those fields are available.

### Level 5 weather warnings

The weather overlay displays JMA bulletins whose official warning names contain `レベル５` or `特別警報`.

JMA Level 5 special warnings and a municipality's `緊急安全確保` evacuation notice are related but distinct information. NWEQ displays the wording contained in the JMA bulletin and does not convert it into a municipal evacuation notice.

### General breaking news

JMA does not publish general editorial breaking news such as political, criminal, or business stories. The NWEQ `ニュース速報` test overlay is therefore separate from this JMA receiver.

## How it works

Each browser page polls its matching public Atom feed every 60 seconds:

- `https://www.data.jma.go.jp/developer/xml/feed/eqvol.xml`
- `https://www.data.jma.go.jp/developer/xml/feed/extra.xml`

Each page downloads its Atom feed directly from JMA, follows the matching bulletin's XML link, and extracts the fields needed by the overlay. Nothing is installed as a Windows service and no local port is opened.

## First-run check

The live pages are intentionally fully transparent after startup. They remember the newest retained feed item without displaying it. A telop and sound are triggered only when a different, newly received matching bulletin appears.

Use `earthquake-test.html` and `level5-test.html` to check positioning, scaling, and transparency in OBS without waiting for a real alert. These test pages use clearly marked sample data.

## Troubleshooting

### OBS shows a blank page

- Confirm that **Local file** is enabled and the correct HTML file is selected.
- Set the source dimensions to **1920 × 1080**.
- Confirm that the computer can open the JMA feed URLs listed above.
- Refresh the Browser source cache in OBS.

### No alert is visible

An empty live source is normal while no new matching bulletin is received. Use the included test pages to verify the visual layout.

### No sound is heard

- Enable **Control audio via OBS** in the Browser source properties.
- Check that the source is visible in the OBS Audio Mixer.
- Confirm that `alert.wav` is in the same folder as `earthquake.html` and `level5.html`.
- Check the source's mixer volume and monitoring settings.

The initial bulletin does not play sound. This prevents an old retained feed item from sounding like a newly issued alert whenever OBS starts.

## Data source and limitations

Weather and earthquake data is provided by the Japan Meteorological Agency. Review the [JMA weather-data portal](https://www.data.jma.go.jp/developer/) and applicable terms before redistribution.

JMA notes that its public pull service may stop or be delayed during maintenance or other disruptions. It is not a guaranteed emergency-delivery service. Always confirm critical information through official emergency channels.

NWEQ JMA Live is an independent display tool and is not affiliated with or endorsed by the Japan Meteorological Agency or NHK.

