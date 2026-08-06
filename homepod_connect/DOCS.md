# HomePod Connect

This addons allows to use OwnTone and Spotify Connect with librespot on Home Assistant OS.


## Configuration

(You can use the VSCode add-on to customize the configuration.)

You can configure OwnTone through its configuration file. It can be found at `/config/owntone/owntone.conf`.

librespot is configurable through `/config/owntone/librespot.conf`, where you can set the name shown in Spotify (`LIBRESPOT_NAME`) and any extra [librespot flags](https://github.com/librespot-org/librespot) (`LIBRESPOT_OPTS`). The file itself lists the flags most people want, e.g. audio quality, autoplay or the initial volume.

There is no account to configure. Spotify dropped username/password logins, so you pick "Home Assistant" from the device list in the Spotify app once and the add-on stays paired with that account. To pair it with a different account, stop the add-on, delete `/config/owntone/librespot` and pick it again from there.

After adjusting something, please restart the add-on.


## Access OwnTone

You can access OwnTone through `[Home Assistant IP]:3689`


## OwnTone Credentials

Username: `admin`

Password: `owntoneadmin8765`
