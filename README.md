
# Home Assistant Dahua Integration
The `Dahua` integration allows you to integrate your [Dahua](https://www.dahuasecurity.com/) cameras in Home Assistant.

Supports motion events, alarm events (and others), enabling/disabling motion detection, switches for infrared, illuminator (white light), security lights (red/blue flashers), 
and sirens.

NOTE: Using the switch to turn on/off the infrared light will disable the "auto" mode. Use the service to enable auto mode again (or the camera UI).

## Installation

### HACS install
To install with [HACS](https://hacs.xyz/):

1. Click on HACS in the Home Assistant menu
2. Click on `Integrations`
3. Click the top right menu (the three dots)
4. Select `Custom repositories`
5. Paste the repository URL (`https://github.com/rroller/dahua`) in the dialog box
6. Select category `Integration`
7. Click `Add`
8. Click `Install`
9. Add integration

### Manual install
To manually install:

```bash
# Download a copy of this repository
$ wget https://github.com/rroller/dahua/archive/dahua-main.zip

# Unzip the archive
$ unzip dahua-main.zip

# Move the dahua directory into your custom_components directory in your Home Assistant install
$ mv dahua-main/custom_components/dahua <home-assistant-install-directory>/config/custom_components/
```

> :warning: **After executing one of the above installation methods, restart Home Assistant. Also clear your browser cache before proceeding to the next step, as the integration may not be visible otherwise.**

### Setup
1. Now the integration is added to HACS and available in the normal HA integration installation, so...
2. In the HA left menu, click `Configuration`
3. Click `Integrations`
4. Click `ADD INTEGRATION`
5. Type `Dahua` and select it
6. Enter the details:
    1. **Username**: Your camera's username
    2. **Password**: Your camera's password
    3. **Address**: Your camera's address, typically just the IP address
    4. **Port**: Your camera's HTTP port. Default is `80`
    5. **RTSP Port**: Your camera's RTSP port, default is `544`. Used to live stream your camera in HA
    6. **RTSP Streams**: The RTSP stream you want to use (Main, Sub, or Both). If both, two camera entities will be created
    7. **Events**: The integration will keep a connection open to the camera to capture motion events, alarm events, etc.
       You can select which events you want to monitor and report in HA. If no events are selected then the connection will no be created.
       If you want a specific event that's not listed here open an issue and I'll add it.

![Dahua Setup](static/setup1.png)


# Known supported cameras
This integration should word with most Dahua cameras. It has been tested with very old and very new Dahua cameras.
The following are confirmed to work:

* IPC-HDW5831R-ZE
* IPC-T5442T-ZE
* IPC-HDW3849HP-AS-PV
* Please let me know if you've tested with additional cameras

# Services and Entities
## Services
Service | Parameters | Description
:------------ | :------------ | :-------------
`camera.enable_motion_detection` | | Enables motion detection
`camera.disable_motion_detection` | | Disabled motion detection
`dahua.set_infrared_mode` | `entity_id`: camera.cam13_main <br /> `mode`: Auto, On, Off <br /> `brightness`: 0 - 100 inclusive| Sets the infrared mode. Useful to set the mode back to Auto

## Camera
This will provide a normal HA camera entity (can take snapshots, motion detection, etc)

## Switches
Switch |  Description |
:------------ | :------------ |
Motion | Enables or disables motion detection on the camera
Siren | If the camera has a siren, will turn on the siren. Note, it seems sirens only stay on for 10 to 15 seconds before switching off

## Lights
Light |  Description |
:------------ | :------------ |
Infrared | Turns on/off the infrared light. Using this switch will disable the "auto" mode. If you want to enable auto mode again then use the service to enable auto. When in auto, this switch will not report the on/off state.
Illuminator | If the camera has one, turns on/off the illuminator light (white light). Using this switch will disable the "auto" mode. If you want to enable auto mode again then use the service to enable auto. When in auto, this switch will not report the on/off state.
Security | If the camera has one, turns on/off the security light (red/blue flashing light). This light stays on for 10 to 15 seconds before the camera auto turns it off.

## Binary Sensors
Sensor |  Description |
:------------ | :------------ |
Motion | A sensor that turns on when the camera detects motion

# Local development
If you wish to work on this component, the easiest way is to follow [HACS Dev Container README](https://github.com/custom-components/integration_blueprint/blob/master/.devcontainer/README.md). In short:

* Install Docker
* Install Visual Studio Code
* Install the devcontainer Visual Code plugin
* Clone this repo and open it in Visual Studio Code
* View -> Command Palette. Type `Tasks: Run Task` and select it, then click `Run Home Assistant on port 9123`
* Open Home Assistant at http://localhost:9123
