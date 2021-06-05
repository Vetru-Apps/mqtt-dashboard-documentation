# Changelog

## App Changelog

### 1.0.0 <small>- June 5, 2021</small>

We are releasing a pretty big update with version 1.0.0. While many of the upgrades may pass unnoticed as they are not UI-related, with v1.0.0-b65 we are pushing many stability and performance enhancements, tweaking the UI to make it feel more polished, releasing a new standard of tile - which will **replace** the old one -, providing new controls to the user to customize the app even more, and generally enhancing the functionalities.

There is a **compatibility warning** with regards to v1.0.0-b65:

- The app is **not** compatible with old versions of *Light* and *Thermostat* compound tiles. You will have to manually upgrade. We are sorry for the inconvenience.
- Compatibility with old backup files will be granted until **end August 2021**. Starting from September 2021, you will be able to restore older backups, but some information might be lost (e.g., icons, color, tile-specific options).
- Compatibility with older versions of the app (b64 and lower) while using *JSON Export over MQTT* will be granted until **end August 2021**. Starting from September 2021, you will be able to share your tiles over MQTT, but some information might be lost (e.g., icons, color, tile-specific options).

As such, please **update the app** on every device you intend to use and be sure to **create a new database backup** to ensure future compatibility.

The new version brings **lots** of enhancements:

- Modified how we store information under the hood. This will allow to add cool stuff faster in the future.
- Redesigned 'Light' and 'Thermostat' tiles to not use a monolithic JSON entity, rather working on distinct topics.
- Added new options to already existing tiles (e.g. to progress and toggle tiles).
- Fixed connection stability issued.
- Added new options to the broker connection (option to not automatically connect, buttons to manually connect/disconnect, better TLS capabilities).
- General improvements and UI tweaks.

## Documentation Changelog

### 1.0.0 <small>- May 31, 2021</small>

- First documentation commit.