# Troubleshooting

### "The Bird Buddy is waking up"

Expected on the first attempt. Try again after about 30 seconds, or set up [go2rtc](configuration.md#go2rtc) so the stream opens on the first click.

### The stream never starts

Check whether the livestream works in the Bird Buddy app. If it does not, the cause is the feeder or its Wi-Fi signal. Close the app before streaming in Home Assistant, as only one session per account is allowed.

### The player stays blank

You are using a browser other than Safari without go2rtc. Set up [go2rtc](configuration.md#go2rtc).

### The picture drops out after a few seconds

Usually a weak Wi-Fi signal, or go2rtc is not configured. Moving the feeder closer to an access point helps more than any setting.

### The picture freezes while the timeline keeps running

The player missed the switch from the holding screen to the live stream. Either enable **Stream continuously** if the feeder is on permanent power, or disable **Open the stream on the first click**.

### The card shows no video

Expected while nobody is watching. Click the card to start the stream.

### The status sensor shows Asleep

The feeder is in night mode, offline, in off-grid mode or updating its firmware. The `detail` attribute shows the reason. A stream cannot start until the feeder is available again.

Still stuck? Enable debug logging as described in the [advanced guide](advanced.md#logging) or open an [issue](https://github.com/barloew/birdbuddy-livestream/issues).
