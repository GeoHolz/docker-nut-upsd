# Network UPS Tools server

Docker image for Network UPS Tools server and Shoutrrr integration for alert

## Usage

This image provides a complete UPS monitoring service (USB driver only).

Start the container:

```console
# docker run \
	--name nut-upsd \
	--detach \
	--publish 3493:3493 \
	--device /dev/bus/usb/xxx/yyy \
	--env SHUTDOWN_CMD="my-shutdown-command-from-container" \
	geoholz/nut-upsd-shoutrrr:0.1.2
```
or via compose file : 
```
version: "3.6"
services:
  nutups-shoutrrr:
    container_name: "nutups-shoutrrr"
    devices:
      - "/dev/bus/usb/00X/00X:/dev/bus/usb/00X/00X"
    environment:
      - "API_PASSWORD=MySecret"
      - "UPS_NAME=ups"
      - "UPS_DESC=UPS"
      - "UPS_DRIVER=usbhid-ups"
      - "UPS_PORT=auto"
      - "ADMIN_PASSWORD="
      - "SHUTDOWN_CMD=echo 'System shutdown not configured!'"
      - "NOTIFY_URL=gotify://GOTIFY_URL:443/TOKEN/?title=UPS-Alert&priority=1"
    image: "geoholz/nut-upsd-shoutrrr:0.1.2"
    ports:v
      - "3493:3493/tcp"
    restart: "always"
```



## Auto configuration via environment variables

This image supports customization via environment variables.

### NOTIFY_URL

Shoutrr URL for alert, see https://containrrr.dev/shoutrrr/v0.8/services/overview/

### UPS_NAME

*Default value*: `ups`

The name of the UPS.

### UPS_DESC

*Default value*: `Eaton 5SC`

This allows you to set a brief description that upsd will provide to clients that ask for a list of connected equipment.

### UPS_DRIVER

*Default value*: `usbhid-ups`

This specifies which program will be monitoring this UPS.

### UPS_PORT

*Default vaue*: `auto`

This is the serial port where the UPS is connected.

### API_USER

*Default vaue*: `upsmon`

This is the username used for communication between upsmon and upsd processes.

### API_PASSWORD

*Default vaue*: `secret`

This is the password for the upsmon user.

### SHUTDOWN_CMD

*Default vaue*: `echo 'System shutdown not configured!'`

This is the command upsmon will run when the system needs to be brought down. The command will be run from inside the container.

