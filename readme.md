# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),

# Streaming Engine Service .NET Core

## [4.0.20] - 10.07.2026

> **⚠ Important — before installing:** delete the C\*\*\* devices and SRT custom inputs repository files from `C:\ProgramData\ORBNET\Streaming Engine`:
> - `C***Devices.json`
> - `SrtInputs.json`

### Added
- **C\*\*\* Streams:**
  - Configured setups are now automatically detected and grouped into dedicated UI sections when device issues occur: misconfigured device, outdated configuration, link-down interface, unreachable device, and orphaned setups. Each row can be discarded, which stops and removes the associated SRT input.
  - Serial-mismatch banner appears when a configured network interface reports a different device than expected. The banner clears automatically once the correct device is reachable again.
  - View changes dialog for outdated setups shows a side-by-side comparison of the configured stream against what the device currently reports.
  - Local stream name validation prevents empty values, whitespace, and duplicates.

### Changed
- **C\*\*\* Streams:**
  - Setups now remain linked to their owning device across device serial changes, preventing accidental orphaning during interface fallbacks.
  - Set up button is disabled while a device is misconfigured, preventing new setups from being created against the wrong device.

## [4.0.19]

### Added
* Streamlink aggregation controls now always display all three aggregation buttons. Previously, only the buttons for active interfaces were shown dynamically; all three buttons are now always visible (inactive interfaces are displayed in gray).

### Changed
* Popup player failover has been improved. When the currently viewed substream becomes unavailable, the popup automatically switches to another live substream and recovers correctly when the aggregate stream returns, even if all substreams were previously offline.
* Dynamic substream discovery has been added. Newly available substreams now appear automatically without requiring a page refresh.
* Popup substream list now refreshes continuously, matching the behavior of the tile view. This resolves an issue where only the active substream button was displayed.

### Fixed
* Fixed aggregate stream latency displaying 0 ms during startup. Latency now remains N/A until a valid measurement is available. This fix also applies to the latency indicator in the status bar.
* Fixed tile initialization when the configured source was offline at startup. Tiles no longer display a green status indicator with missing xFill buttons, and aggregation buttons now appear automatically once streams become available without requiring a page refresh.
* Fixed SRT caller-mode auto-reconnect. SRT inputs are now automatically reinitialized when video streaming stops but the publishing point remains active, eliminating the need to manually stop and restart the SRT input from the UI.
* Fixed automatic restart of WebTransport (WT) and HLS tile players after stream interruptions, allowing video playback to recover without requiring a page refresh.
* Fixed inactive retry streams being exposed in the UI. Internal retry mechanisms are now hidden from users to reduce confusion.
* Fixed the Last Activity column on the Active Streams page to display a human-readable date and time instead of a Unix timestamp.

## [4.0.16]

### Fixed
* Latency buttons stayed visible when all streams were stopped.

## [4.0.15] - 24.06.2026

### Fixed
* Streams that cannot start over **WebTransport** automatically fall back to **HLS** only on mobile.
* SRT custom input failing to stop.
* Latency display (corrected color and text).
* Source list not refreshing on the Views page.
* Device statistics not updating on the Views page.
* Removed leftover "Snapshot" label from the Views page.

## [4.0.13]

### Added
* Automatic fallback from **WebTransport** to **HLS** when a stream cannot start over WebTransport.

### Fixed
* Live video no longer opens in a private/incognito browser context.
* Hidden the "X recordings available" and "X logs available" indicators in the remote retrieve view.
* Corrected mobile styling for live video.

## [4.0.12]

### Fixed
* Memory leak in Google Chrome triggered by the Views page.
* Memory not released when closing the live popup player.
* Incorrect device status display for **C\*\*\*\*** streams on the Views page.
* Tile click not opening the live popup player when WebTransport is disabled.

## [4.0.11]

### Added
* Explicit backend handling for **Opus** audio streams.

### Fixed
* No video in the WebTransport player when the stream carried an Opus audio codec.
* Audio/video startup and synchronization (corrected timing handling).

## [4.0.09]

### Added
* Recording tamper detection using **ECDSA** signatures (a `.sig` file is generated per recording).
* Standalone **RecordingVerifier** project for validating recording integrity.
* Recording tamper check in **PlaybackController**.

### Fixed
* Cameras could not be dragged onto a view after collapsing and reopening the left-hand view list during live playback.
* MSE `SourceBuffer` was never trimmed, causing browser out-of-memory crashes (**critical**).
* `fragmentQueue` array growing without bounds.
* Debug logging emitted a console log on every fragment append, accumulating string allocations over time.
* MSE URL revocation bug in `resetMSE`.

## [4.0.08]

### Added
* **Allow Hardware Acceleration** setting.
* **Preferred Media Framework** setting.
* **FFmpeg 64-bit Path** setting.
* **FFmpeg 32-bit Path** setting.
* **CUDA 64-bit Path** setting.

### Changed
* Upgraded **FFmpeg** from `6.0` to `7.1`.
* `AllowHardwareAcceleration` and `PreferredMediaFramework` settings are now applied at runtime.

## [4.0.06]

### Added
* **C\*\*\*\* statistics overlay and status popup** in the popup player and on each tile view (device status, FTP transfer state, transfer progress, destination address, etc.).
* **C\*\*\*\*HeartbeatAdapter**: maps C\*\*\*\* device status to the standard heartbeat format used across the UI.
* Multi-interface failover for remote retrieve/record commands (retries across all network interfaces, skipping any that are down).

### Changed
* Remote Retrieve & Remote Record for **C\*\*\*\*** streams is now fully supported from tile views, the popup player, and the Remote Streams Options page.
* Remote retrieve/record commands now display a confirmation or error toast.

### Fixed
* Status and stream polling loops for C\*\*\*\* devices no longer exit on HTTP timeout or transient errors.
* Status polling now falls back across all interfaces on failure instead of using only the active one.

## [4.0.05]

### Added
* Remote Retrieve & Remote Record for **C\*\*\*\*** streams (start/stop from tile views and popup player; not yet available from the Remote Streams Options page).
* Aggregate streams for C\*\*\*\* SRT streams.

## [4.0.04]

### Added
* Configurable latency for **C\*\*\*\* SRT (caller)** streams during setup and editing (the applied value is the highest latency configured between SE and the C\*\*\*\* device).
* Automatic background retry for C\*\*\*\* SRT stream connections not established at startup.
* Automatic removal of previously configured C\*\*\*\* SRT streams after **3** consecutive successful fetches in which the stream identity is missing (deleted or changed on the device). Streams are not removed on fetch errors, empty stream lists, or when a network interface is not reporting/enabled.

### Changed
* Improved usability of the C\*\*\*\* stream discovery UI.
* C\*\*\*\* SRT streams now auto-start after setup.

## [4.0.02]

### Added
* New dedicated log file for recordings.

### Changed
* Reworked the popup live view and the live view on tiles.
* Replaced "Open recordings/logs folder" with "Copy recordings/logs path".

### Fixed
* Left-hand menu had a fixed length that forced unnecessary scrolling.
* Stream keys containing spaces are now normalized (e.g. `AXIS h265` → `AXIS_h265`).
* Performance page: Select/Deselect All now respects the active filter.
* Performance page: only streams active during the selected interval are listed.
* Graphs now display time in the browser's local timezone.
* Corrected device status display for the Operator role.

## [4.0.0] - 18.05.2026

### Added
* Role-based customization for **Admin** and **Operator**.
* Page categories and subcategories.
* **System Overview** and **Diagnostics** views for Admin.
* **Dashboard** for the Operator role.
* Additional stream actions on the **Active Streams** page.
* Retrieve buttons on the live view popup.
* Registration of **C\*\*\*\* devices** with automatic discovery of their streams (new "C\*\*\*\* Stream Config" section and "Discovered Streams" subsection under **Stream Config → Stream Inputs**, route `/inbound`).
* `MinimumSpamTime` configuration option (default `3s`).
* `EnableMinimumThreadCount` (default `false`) and `MinimumThreadCount` (default `512`) configuration options for spam-prone environments.
* Spam protection: requests from the same IP for the same source are blocked within the `MinimumSpamTime` window.
* Enhanced logging for server event handlers.

### Changed
* General UI refresh and a new page selection mode.
* Reworked the recording playback page.
* Split the Stream Settings page into **Recording & Storage Settings** and **Remote Streams Options**.
* HLS player latency is now added to the internal latency; the player displays the total.
* Publishing-point snapshots are fetched in a single centralized request instead of per page.
* Reduced default `ReconnectTimeout` from `10s` to `3s`; sources that fail to start within this window are disposed.
* Set `MinimumPartialSegmentDuration` to `1000ms` to better absorb LL-HLS lag.
* Smoother live-edge seek synchronization in the HLS player.
  * **Note:** for correct LL-HLS playback, the camera GOP must be `< 2 × FPS`.

### Fixed
* RTSP server hanging under high request volume.
* HLS player not checking whether LL-HLS was enabled in the configuration.
* Stream disconnect/reconnect handling in live view.
* ONVIF camera add and status-retrieval issues.
* Double-clicking on the Views page.

## [3.0.12]

### Changed
* Updated core dependencies and internal components.
* Enhanced ONVIF support and publishing functionality.
* Improved overall streaming stability and cross-environment compatibility.

### Fixed
* Live video issues on certain system configurations.
* General bug fixes and performance improvements.

## [3.0.11]

### Changed
* Improved ONVIF integration (profiles, PTZ configuration, device handling).
* Enhanced PTZ functionality (presets, controls, validation checks).
* Improved handling of multiple media profiles.
* UI and stability improvements for camera control and playback.

### Fixed
* Reconnect issues and display inconsistencies in live view.

## [3.0.10]

### Changed
* Completed recording and retrieval functionality improvements.
* Enhanced load-balancing behavior and configuration consistency.
* Improved backchannel communication for streaming workflows.

### Fixed
* Various fixes for remote retrieval and system reliability.

## [3.0.9] - 24.03.2026

### Added
* Button in the top-right menu bar to download Streaming Engine logs as a `.zip` directly from the browser.
* **FTP support** for remote file transfers.
* Enhanced logging in the live player audio section.

### Changed
* Reworked XFILL aggregate logic: XFILL streams are now grouped by prefix, with each distinct suffix shown within the aggregate.
* Expanded remote recordings retrieval (broader provider support and new configuration options).
* Improved audio handling and stability in the live player.
* Improved PTZ controls and device compatibility.
* Default configuration updates:
  * "Enable Net Core Streaming Server" and "Bypass Management Client Stream Activation" are now **enabled** by default.
  * RTSP bypass is now **enabled** by default for better performance.
  * Camera thumbnail preview ("Show thumbnail of the camera") is now **disabled** by default.

### Fixed
* Crash on the Stream Settings page.
* Crash in the live player caused by an audio timer.
* Snapshot retrieval for ONVIF cameras.
* General fixes for settings, streaming edge cases, and stability.

## [3.0.8]

### Added
* **XFill stream support**: grouping, custom keys, aggregate logic, and live video integration.
* Minimum-duration settings for the LL-HLS player.

### Changed
* Enhanced remote recordings (delete action, new columns, latency color coding).

### Fixed
* Metrics reload and stream settings issues.
* LL-HLS player lag.

## [3.0.6]

### Added
* Remote retrieval and playback for local and remote recordings.
* Recording download functionality.

### Changed
* Improved PTZ for ONVIF cameras (continuous mode, presets, joystick handling).
* Recording folder improvements.
* UI updates for recordings and latency buttons.

## [3.0.4]

### Added
* Stream metrics (bitrate, FPS) with graphs for streams and error logs.
* CPU and memory usage graphs.
* New options for metrics and charts.

### Changed
* Improved PTZ controls and presets.
* Updated WebTransport player.

## [3.0.2]

### Added
* Local **NTP Server** and **NTP Sink**.
* Start/Stop recording buttons on tiles and in the live player.
* Device status popups (**LiveDeviceStatusPopup** and **FullDeviceStatusPopup**).
* Overlay for device statistics.

### Changed
* Improved local recordings display on tiles and the popup live player.

### Fixed
* Audio and WebTransport fixes.

## [3.0.0]

### Added
* **WebTransport (HTTP/3)** support for low-latency streaming on tiles and in the live player.
* Audio mute/unmute support (WebTransport and HLS).
* Low-latency player with latency metrics and average latency.
* **User management and roles** system, including UI restrictions for the Operator role.
* **DataCommandChannel** and **PTZ-over-SRT** commands.
* Basic recording manager.

### Changed
* Replaced Milestone branding with **XProtect** in the UI.

### Fixed
* Various stability fixes (reconnect logic, camera cache, installer, memory leaks).

## [2.9.98] 

### Added
* Enable AAC Empty Audio Stream setting: always-on silent AAC stream for destinations like YouTube/X.
* Timeline (seek bar) to embedded video players on webpages.

### Fixed
* Memory leak on webpages.
* Outbound streams state handling.
* Frames for memory pipe transport.
* CSS styling issues in the web application.
  
## [2.9.97] 

### Added
* Enforced **automatic redirection** of unauthorized users to the login page for all protected routes.
* Added support for **refresh tokens** to extend JWT-based authentication sessions securely.
* Added **Custom Inbound Streams** on route `/customstreams` on web application.
* Added **Saved Transcoding Profiles** on route `/transcoding-profiles` on web application.
* Added **HLS Web Streaming** on route `/webstreaminghls` on web application.
* Added **ONVIF Devices** on route `/onvif-devices` on web application.
* Added **Milestone Cameras** on route `/milestonecams` on web application.

## [2.9.96] - 24.06.2025

### Added
* **Inbound Stream Management** page available at route `/inbound`, including:
  * Display of active RTMP, RTMPS, and SRT ingest URLs of preconfigured inputs, with copy-to-clipboard functionality.
  * **SRT Input Manager** for configuring and managing inbound SRT inputs:
    * Allows users to **add**, **edit**, **start**, **stop**, and **remove** SRT Input servers.
    * Supports configuration of the following options:
      - **Publishing Point Name** and **UDP Port** for stream identification and network binding.
      - **Connection Mode** selection: **Caller** or **Listener**.
      - **Optional encryption**: AES-128, AES-192, or AES-256 enabled by providing a passphrase.
      - **Remote Address** and **Remote UDP Port** for Caller connections.
      - **Latency configuration** in milliseconds for buffering optimization.
      - Real-time status display: **Offline**, **Ready**, or **Streaming**.
    * The form dynamically adapts to the selected connection mode and performs validation to prevent duplicate names or conflicting ports.
  * **SRT Input Manager** for configuring Inbound SRT Inputs:
    * Add, edit, start, stop, and remove SRT input servers.
    * Supports Caller and Listener connection modes, AES-128/192/256 encryption or none, passphrase protection, and latency configuration.
    * Adaptive UI form fields respond to selected connection mode by dynamically adding or removing input fields.
    * Validates all input data before saving, including checks for naming conflicts and port duplication, and displays a warning to the user.
    * Displays real-time connection status: **Offline**, **Ready**, or **Streaming**.
  * **Published Inbound Streams list**:
    * Displays Stream Key (RTMP) / Publishing Point Name (SRT), Protocol (RTMP/SRT), and number of connected clients.
    * Allows users to view output endpoints and live stream preview via an integrated video player.
* Added documentation for APIs from Swagger.
* Added in webpages built-in **Video Player** to preview the live feed.
* Added **Timelapse** on route `/timelapse` on web application.


# Streaming Engine Load Balancer

## [2.9.98] 

### Changed
* Added dedicated **SQLite** Database.

### Fixed
* CSS styling issues in the web application.

## [2.9.97]

### Added
* RTMP Camera Stream Controls on route '/rtmp' on web application.

## [2.9.96] - 24.06.2025

### Added
* Integrated **SQLite** database to store user data.
* Implemented **RESTful APIs** with **JWT-based authentication** using **asymmetric keys**.
  Tokens are stored in **session-based cookies** for secure communication with the Streaming Engine Service .NET Core.
* Added support for a **self-signed certificate**, which is **automatically generated during setup installation** if the `CertificateThumbprint` field in `ORBStreamingEngineConfig.xml` is empty.

  **Important Note for Windows Users:**
  If we're using the autogenerated certificate, manual permission setup is required to allow the .NET Core Streaming Engine service to access the private key for HTTPS communication:
  1. Open **Control Panel** → **Manage Computer Certificates**.
  2. Go to **Personal** → **Certificates**.
  3. Find `Self-Signed Streaming Engine NetCore Certificate` under **Friendly Name**.
  4. Right-click → **All Tasks** → **Manage Private Keys**.
  6. Click **Add**, type `"network service"` → **Check Names** (should resolve to `NETWORK SERVICE`) → **OK** → **Apply**.

* Added **login page** page available at route `/login` using JWT and session cookies for user authentication.
* Enforced **automatic redirection** of unauthorized users to the login page for all protected routes
* Added support for **remote outbound stream control** (load/start/stop) for Streaming Engine nodes
  
### Changed
* Switched internal HTTP server to **HTTPS**, using **TLS** for secure communication.
* Changed **default port** from `45337` (HTTP) to `45338` (HTTPS) for web interface.

### Fixed
* RTSPS redirection to Streaming Engine


# [2.9.3] - 13.03.2022

- Added: HLS Streaming Capability
- Added: Service Recovery Options to restart the service on first, second and subsequent failures.
- Added: Auto Stream transcoding for HLS
- Changed: Switched to FFMPEG for RTMP and HLS Streaming


# [2.9.2] - 06.03.2022

- Added: Timelapse monitoring in case of broken frame grabbers
- Changed: License, Config files and Service logs are now located in ProgramData instead of AppData
- Changed: Installing Milestone plugins will copy files to Management Client and Event Server MIP Plugins folder
- Changed: Log level must be set to Debug to view timelapse saving logs
- Fix: Changing account login of the Streaming Engine Service will not break anything.
- Fix: Installing Streaming Engine Service to other driver than C:\ drive is now supported
- Fix: License Unique ID will be set as soon as first connection to Milestone is established.

