# Configuration

Go to **Settings → Devices & services**, find **Bird Buddy Livestream** and select **Configure**.

## Settings

| Setting | Default | Description |
|---|---|---|
| Stream continuously | Off | Keeps the stream running. See [Continuous streaming](#continuous-streaming). |
| Retry every | 120 s | Wait time before retrying, or between checks while the feeder sleeps |
| Stop the stream after | 5 min | Maximum stream duration |
| Wait for the stream at most | 90 s | How long to keep trying to wake the feeder. Raise to 180 s if streams often fail to start. |
| go2rtc address | Empty | For example `http://192.168.1.10:1984` |
| go2rtc RTSP port | 8554 | |
| go2rtc ffmpeg template | Empty | Advanced; leave empty |
| Transcode to H.264 | On | Required for browsers other than Safari |
| Preview image | Last frame | What the card shows while nobody is watching |
| Preview entity | Empty | Used with **Another entity's picture** |
| Preview image file | Empty | Used with **Image file** |
| Show status on the preview | On | Overlays the status, such as "Waking up" or "Live" |
| Stop when nobody is watching | Off | Requires go2rtc. Experimental: can stop a stream that is still being watched. |
| Open the stream on the first click | On | Requires go2rtc |
| Placeholder source | Default | Advanced; change only if the holding screen stays black |

Option keys and ranges are listed in the [advanced guide](advanced.md#all-options).

## Preview image

The card cannot show live video while the feeder sleeps. Choose what it shows instead:

| Option | Description |
|---|---|
| Last frame | A still from the previous stream. Empty until the first stream. |
| Latest bird | The most recent visitor from your Bird Buddy feed, with its species. Also exposed as the `last_visitor` and `last_visitor_at` attributes. |
| Another entity's picture | The picture of the entity set in **Preview entity** |
| Image file | A fixed image, such as `/media/local/birdbuddy.jpg`. Home Assistant must be allowed to read the folder. |
| None | No preview |

## go2rtc

Bird Buddy streams HEVC video, which plays only in Safari. [go2rtc](https://github.com/AlexxIT/go2rtc) converts it for every browser, buffers short network drops and lets the stream open on the first click.

1. Install and start the **go2rtc** add-on from **Settings → Add-ons → Add-on store**, or use an existing go2rtc or Frigate instance.
2. Enter the go2rtc address, such as `http://IP-OF-HOME-ASSISTANT:1984`.
3. Keep the RTSP port at `8554` and **Transcode to H.264** on.

The integration registers the stream in go2rtc automatically.

## Waking the feeder in advance

Press **Prepare livestream** about a minute before watching, or use an automation:

```yaml
automation:
  - alias: Prepare Bird Buddy livestream
    triggers:
      - trigger: state
        entity_id: binary_sensor.someone_home
        to: "on"
    actions:
      - action: button.press
        target:
          entity_id: button.bird_buddy_prepare_livestream
      - wait_for_trigger:
          - trigger: state
            entity_id: sensor.bird_buddy_livestream_status
            to: streaming
        timeout: "00:02:00"
```

## Continuous streaming

For feeders on permanent power, such as USB-C or sufficient solar, enable **Stream continuously** to record with Frigate or another NVR.

- The feeder does not record its own visits while streaming.
- Bird Buddy turns the camera off after dark and during firmware updates. The status sensor then shows **Asleep**, with the reason in its `detail` attribute, and the stream resumes automatically.

Point Frigate at go2rtc, not at the integration:

```yaml
cameras:
  birdbuddy:
    ffmpeg:
      inputs:
        - path: rtsp://192.168.1.10:8554/birdbuddy_9f5cc1c488ec
          input_args: preset-rtsp-restream
          roles: [detect, record]
    detect:
      width: 768
      height: 1024
```

The stream name is `birdbuddy_` followed by the first 12 characters of the feeder ID. It appears in the log line starting with "Stream runs through go2rtc". Expect gaps overnight.
