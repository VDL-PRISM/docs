# Deployment Documentation
## Setup Private Network
Plug in the provided wireless access point. The network name is `prisms_setup` and the password is `cleanair!`.

Connect the Dylos that you are deploying to the access point using Ethernet cables. Power on the Dylos sensors.

Connect the configuring device (Chromebook, laptop, etc.) either through Ethernet or WiFi.

Wait for Dylos to show something on the LCD display.

**Warning**: Port number 4 on the access point doesn’t work. We are getting a new access point, but in the meantime, avoid this port.


## Configure Dylos
You will need to configure each Dylos sensor with the wireless network name and password of the home you are deploying to. This is **not** the network name and password of the access point you used in the previous step.

You must configure each Dylos sensor individually. Connect to the Dylos by going to the following URL: `http://monitorXXXX.local:3210/`. Replace the `XXXX` with the monitor ID. There is a label on the front of the Dylos sensor with its ID. For example, replace the `XXXX` with `B001` to make `http://monitorB001.local:3210`.

This should bring up a webpage that allows you to configure the network name and password of the wireless network. Select the wireless network from the list on the left. Have the participant enter the password on the right. **Note**: Only networks with WPA or WPA2 are supported. Open networks (networks with no password) and WEP networks are not supported for security reasons.

Once the Dylos is connected and an IP address is shown, that Dylos is ready to be used. Repeat this process over for each Dylos you are configuring.

## Setting up Gateway
Plug the gateway into a spare Ethernet port on the participants home network.

## Dylos Display
The Dylos display has useful information that can help with debugging the system. The display should look something like this:

```
+---------------+
|       ok 13:05|
| 101    5 13:05|
+---------------+
```

On the top row, the “ok” mean that the air quality reading is not zero. This means that the Dylos is measuring the air quality correctly. If one of the air quality values is zero, then the display will show “not okay”. Next to the “ok” is the last time that data was read from the Dylos sensor. It uses a military time format and GMT time zone. This means the hours will be 6 or 7 hours ahead (depending on if it is day light savings or not). The most important thing is that this value is updating every minute.

On the button row, the first value (`101` in this example) is the last part of its IP address. If there is no value there, that means the Dylos is *not* connected to the wireless network. The next value (`5` in this example), is the number of samples stored on the Dylos. If everything is working properly, this value should be less than 10. If the value is high and keeps increasing, this means the gateway is pulling data from it. The last value (`13:05`) is the last time the gateway pulled data from the Dylos sensor. This should update every 2 to 5 minutes.

## Troubleshooting
- Dylos monitors do not receive an IP address
    - Make sure the monitor is within range of the home access point. Check that the monitor has been configured with the correct network name and password.
- Dylos monitor shows a “not okay” value.
    - Make sure that the fan is running. If it is not running, restart the Dylos sensor by unplugging and plugging it back in. After waiting 5 minutes, if the fan continues not to run or if it is running and the monitor still shows “no” values, there may be a problem with the monitor hardware. Contact Phil or Kyeong.
- The record count increases over time
    - The gateway computer is not pulling data from the Dylos monitor. If the monitor has an IP address, give it time to reconnect (~5 minutes). Make sure the gateway computer is running. Otherwise, unplug the power cable from the Dylos monitor. Re-connect the power cable and press the power button.  Wait 3-5 minutes and the monitor should re-connect to the network.
- If anything weird occurs, restart the Dylos sensor and report it to Phil.

#research/prisms
#documentation
