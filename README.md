# SBC Reto6 firmware

Public ESP32 application images for the SBC Reto6 OTA exercise.

These are OTA application images for classic ESP32 (4 MB flash), built with ESP-IDF 5.5.5. They are intended for a device already provisioned with the Reto6 1.0.0 bootstrap firmware and its OTA partition table.

## Firmware files

| Version | File | Size (bytes) | Intended demonstration |
| --- | --- | ---: | --- |
| 1.1.0 | [Reto6-1.1.0.bin](Reto6-1.1.0.bin) | 1086544 | Dashboard RPC: 1.0.0 to 1.1.0 |
| 1.2.0 | [Reto6-1.2.0.bin](Reto6-1.2.0.bin) | 1086544 | Scheduled check: 1.1.0 to 1.2.0 |
| 1.3.0 | [Reto6-1.3.0.bin](Reto6-1.3.0.bin) | 1086544 | Startup check: 1.2.0 to 1.3.0 |

The three images have the same application features and distinct embedded version numbers, allowing each OTA trigger to be tested separately. Their `project_name` is `Reto6`.

## ThingsBoard configuration

Set the following **Shared attributes**, all with type **String**, for the intended target version:

- `targetFwVer`: the version, for example `1.1.0`.
- `targetFwUrl`: the HTTPS raw file URL below.
- `targetFwSha256`: the full SHA-256 value from the matching release JSON.

| Version | Raw download URL |
| --- | --- |
| 1.1.0 | https://raw.githubusercontent.com/weiyinzhangupm-hue/sbc-reto6-firmware/main/Reto6-1.1.0.bin |
| 1.2.0 | https://raw.githubusercontent.com/weiyinzhangupm-hue/sbc-reto6-firmware/main/Reto6-1.2.0.bin |
| 1.3.0 | https://raw.githubusercontent.com/weiyinzhangupm-hue/sbc-reto6-firmware/main/Reto6-1.3.0.bin |

For an immutable URL, replace `main` with the commit SHA containing these files.

Changing Shared attributes publishes a target; it does not immediately install it. The device checks on startup, at its configured daily time, or when receiving RPC method `checkFirmwareUpdate` with parameters `{}`.

An accepted RPC is not proof of installation. After the device reboots, verify `currentFwVer` and `otaLastResult=UPDATED` in ThingsBoard.

## Integrity

| Version | SHA-256 of the complete binary |
| --- | --- |
| 1.1.0 | `0c4a31506225a3f4dd751d29d0d463ebbdabf886a62afe400b123463a2c9bada` |
| 1.2.0 | `73cd4eb048183f7f7cd94936c813e432e85f2fc19d505e06f35752c2f9259139` |
| 1.3.0 | `fddd09406dfcb4f97dc789d74d74eb9bd4de14060e2e48efa0905ed67a9e8774` |

See `release-1.1.0.json`, `release-1.2.0.json`, and `release-1.3.0.json` for build metadata.

## Connection configuration

The public images have empty compiled Wi-Fi SSID, Wi-Fi password, and ThingsBoard device token. They reuse connection settings previously saved in the device's NVS by the private bootstrap firmware.

Do not use these images to provision a blank device. Configure and USB-flash the private bootstrap project first. Keep local `sdkconfig`, private bootstrap binaries, and device credentials out of this repository.

