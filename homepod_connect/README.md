# HomePod Connect

This add-on allows you to stream music directly to your HomePods from Spotify.

You can find more details on the [Home Assistant Community post](https://community.home-assistant.io/t/homepod-connect-spotify-on-homepods-with-spotify-connect/482227) and in the Documentation tab.

## Features
- OwnTone & librespot-java inside one Docker image
- Ready out of the box with zeroconf & Home Assistant integration
- Control OwnTone through Home Assistant
- Metadata support (visible in Home Assistant)
- Fully customizable through the config files in `/config/owntone`

## Requirements
- A Spotify Premium account
- At least one HomePod
- Correct set up in the Home app
  - Home -> Home Settings -> Allow Speaker & TV Access -> Anyone On the same Network
  - Home -> Home Settings -> Allow Speaker & TV Access -> Require Password: Disabled
- VSCode addon or any other text editor to edit the configuration

## Comfort features
Additionally to this addon, I recommend to use the Home Assistant OwnTone integration. With that, you can control which HomePod should play music.

[![Open your Home Assistant instance and start setting up a new integration.](https://my.home-assistant.io/badges/config_flow_start.svg)](https://my.home-assistant.io/redirect/config_flow_start/?domain=forked_daapd)

<!-- Second, there is a blueprint that allows you to automatically play music in a room as soon as someone enters it.
You can find it in the Home Assistant Community. -->
