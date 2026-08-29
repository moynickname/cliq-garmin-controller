# CLIQ Light Controller for Garmin Edge

An independent Connect IQ controller that keeps CLIQ Bluetooth bike lights
usable without the original mobile application, which is no longer available
in the major app stores.

The application connects directly from a Garmin Edge to a nearby CLIQ light
over Bluetooth Low Energy. It can:

- turn the light on and off;
- select modes 1 through 4;
- show the selected state;
- display battery percentage;
- show BLE connection status.

No phone, account, internet connection, analytics, or cloud service is used.

## Compatibility

| Hardware | Status |
| --- | --- |
| Garmin Edge 840 | Tested |
| CLIQ Bluetooth bike light | Tested with one hardware revision |
| Other Garmin or CLIQ models | Untested; changes may be required |

The application looks for the known CLIQ BLE service and, for firmware that
does not advertise service UUIDs, a device name beginning with `CLIQ_`. It then
verifies the required service and characteristics before enabling control.

## Install the ready-to-use application

1. Open the latest GitHub Release.
2. Download `CLIQ-Light-Edge840.prg` and `SHA256SUMS.txt`.
3. Verify the download if desired:

   ```sh
   shasum -a 256 -c SHA256SUMS.txt
   ```

4. Connect the Garmin Edge 840 to a computer by USB.
5. Copy `CLIQ-Light-Edge840.prg` to the device's `GARMIN/APPS` directory.
6. Safely eject the Garmin and open **CLIQ Light** from its apps/widgets list.
7. Make sure only the light you intend to control is powered on and nearby
   during the first connection.

To uninstall it, remove the application through the Garmin interface or delete
its PRG file from `GARMIN/APPS` while the device is connected by USB.

Garmin treats sideloaded applications as developer content rather than an
official Connect IQ Store release. Unsigned PRG files will not run; use the
signed file attached to this project's release.

## Build from source

Requirements:

- Garmin Connect IQ SDK;
- Visual Studio Code with the Monkey C extension;
- a Connect IQ developer signing key.

Open this repository in Visual Studio Code, then run:

```text
Monkey C: Build for Device → Edge 840
```

Choose an output directory outside the repository. The extension can generate
a developer key if one is not already configured. Never commit that private
key. Copy the resulting PRG to `GARMIN/APPS` to test it.

## How it works

The application registers the CLIQ BLE profile, scans for a compatible light,
connects, enables notifications, and writes small commands for power and mode
selection. Battery percentage uses the standard Bluetooth Battery Service.

The Glance is only a lightweight launcher. It does not initialize Bluetooth or
control the light until the full application is opened.

The minimum interoperability details are documented in [PROTOCOL.md](PROTOCOL.md)
so others can study, verify, and reuse the findings without needing proprietary
software or raw captures.

## Project structure

```text
source/                  Monkey C application source
resources/               Strings and launcher icon
manifest.xml             App type, permissions, and Edge 840 target
monkey.jungle            Build configuration
PROTOCOL.md              Minimal BLE interoperability notes
SECURITY.md              Security and disclosure guidance
```

Generated files, debug metadata, developer keys, and local SDK state are
intentionally excluded from version control.

## Safety and limitations

- Test controls while stationary before relying on them during a ride.
- Do not interact with the screen when it would distract you from the road.
- Confirm the physical light state; UI state may update before a BLE response.
- Keep only the intended CLIQ light powered on and nearby while connecting.
- Automatic synchronization depends on notifications produced by the light.
- Third-party Glances can have firmware-specific rendering issues on Edge x40
  devices; this does not necessarily affect the full controller.

## Independent project

This community project is not affiliated with, endorsed by, or supported by
Garmin or CLIQ. Product names and trademarks belong to their respective owners.
No original CLIQ software, firmware, artwork, keys, or proprietary assets are
distributed here.

The software is provided without warranty. Use it only with hardware you own
or are authorized to control.

## License

The original source code and documentation in this repository are available
under the [MIT License](LICENSE). The protocol values describe observed
functional behavior and are provided for interoperability.
