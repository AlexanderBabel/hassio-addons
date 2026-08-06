# Changelog

## [29.3-4] - 2026-08-06

Big one. LinuxServer deprecated the `daapd` image this add-on was built on and
points at the official OwnTone container, so the image was rebuilt on top of
that. librespot-java went with it: it has had no release since the Spotify API
changes of 2025, so Spotify Connect had stopped working on a lot of setups. It
is replaced by the actively maintained Rust librespot.

Your `owntone.conf` is kept as it is. Two things worth doing after updating:

- Pick "Home Assistant" in Spotify again, the old librespot-java credentials
  cannot be reused.
- If you want OwnTone's log in the add-on log tab, set
  `logfile = "/dev/stderr"` in `/config/owntone/owntone.conf`.

### Upstream
- Update to OwnTone 29.3 (from 28.10)
- Replace librespot-java 1.6.5 with librespot 0.8.0

### Changed
- Base image is now the official `owntone/owntone` container, services run
  under OpenRC instead of s6
- The Spotify pipe moved from `/music/librespot-java` to `/music/Spotify`
- librespot is configured through `/config/owntone/librespot.conf` instead of
  `librespot-java.toml`, and that file lists the options worth changing
- Volume, pause, stop and seek are passed to OwnTone directly, so they take
  effect immediately instead of after the buffer
- Start at 50% volume
- Image is pulled from `ghcr.io/alexanderbabel/owntone`
- No JRE in the image anymore
- The Spotify audio cache in `/config` is capped at 1 GB

### Removed
- `/config/owntone/librespot-java.toml` and `credentials.json` are unused now
  and can be deleted


## [28.10-ls172] - 2025-01-05
### Upstream
- Update to linuxserver/daapd:28.10-ls172
- Update librespot-java to 1.6.5


## [28.8-ls126] - 2023-12-29

This release fixes an issue with the latest versions of Home Assistant OS. For more details take a look at the conversation on the in the [Home Assistant Community](https://community.home-assistant.io/t/homepod-connect-spotify-on-homepods-with-spotify-connect/482227/71?u=alexbabel).

### Upstream
- Update to linuxserver/daapd:28.8-ls126


## [28.8-ls118] - 2023-09-06
### Upstream
- Update to linuxserver/daapd:28.8-ls118


## [28.6-ls112] - 2023-05-17

This release fixes an issue with connecting to Spotify. See [librespot-org/librespot-java#614](https://github.com/librespot-org/librespot-java/issues/614)

### Upstream
- Update to linuxserver/daapd:28.6-ls112
- Update to librespot-java to 1.6.3


## [28.6-ls110] - 2023-05-02
### Upstream
- Update to linuxserver/daapd:28.6-ls110


## [28.6-ls106] - 2023-03-26
### Upstream
- Update to linuxserver/daapd:28.6-ls106

### Bug fixes
- owntone.conf is no longer overwritten


## [28.6-ls105] - 2023-03-07
### Upstream
- Update to linuxserver/daapd:28.6-ls105


## [28.5-ls100] - 2023-01-23
### Upstream
- Update to linuxserver/daapd:28.5-ls100


## [28.5-ls96] - 2022-11-25
### Added
- Add automatic mount of the /media directory

### Upstream
- Update to linuxserver/daapd:28.5-ls96


## [28.5-ls94] - 2022-10-31
### Added
- Initial release of this add-on
- Add librespot-java to the docker image
- Add custom configurations to make Spotify Connect work out of the box
- Use linuxserver/daapd:28.5-ls94 as the base
