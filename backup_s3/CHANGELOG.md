# Changelog

## [15.0.9] - 2024-09-28

### Upstream
- Update to 15.0.9 base image

### Bug fixes
- Replace `--quiet` with `--no-progress` for the s3 sync command

## [13.2.0] - 2023-04-14

### Breaking Changes

- This release switches to a prebuilt image and therefore drops support for architectures other than arm64 and amd64.
- The versioning is now synchronized with the base hassio addon image
- If you are running on arm64 or amd64, this version is not breaking for you.

### Upstream
- Update to 13.2.0 base image

## [2.5.4] - 2023-04-02
### Bug fixes
- Broken build of new container: Disable init step in config.yaml 

## [2.5.3] - 2023-03-26
### Upstream
- Update to 13.1.4 base image 
