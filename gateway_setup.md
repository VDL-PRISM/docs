# Gateway Documentation
## Parts
- Raspberry Pi
- Power supply (micro USB)
- microSD card (at least 8 GB)
- Computer with SD card reader
- Case for Raspberry Pi (optional)

## Set Up
### Download the Image
Download the prisms gateway image from [here](https://github.com/VDL-PRISM/gateway-image-builder/releases/download/v1.1.0/prisms-gateway-v1.1.0.img.zip).

### Writing Image to the SD Card
In order to copy the image file to an SD card, you need special software. Download an app called Etcher [here](https://etcher.io).

Once you have Etcher downloaded and installed, plug in your SD card and run Etcher. Select the image that you previously downloaded. Etcher typically detects if an SD card is plugged in and selects the correct drive for you. Confirm that this is the correct drive. Lastly, click the “Flash!” button. Once the process is complete, you can remove the SD card (but keep it handy because we will be using it in the next step).

### Configure SD Card
Plug in the SD card back in and wait for it to mount. Open up the “boot” drive and locate the `device-init.yaml` file. This file allows you to customize your gateway. The first time the gateway runs, it looks for this file and configures itself. This file will only be used on the first boot so any changes made after the first boot will not take affect.

The configuration file is a [YAML file](http://yaml.org) which has a specific format and syntax. If you are unfamiliar with YAML, you should take a few minutes to familiarize yourself. It is not a complicated format, but it is worth understanding. There are lots of resources online for learning more about it ([here](https://learnxinyminutes.com/docs/yaml/) and [here](https://www.youtube.com/watch?v=W3tQPk8DNbk)).

Download an updated `device-init.yaml` file from [here](https://www.files.app.lundrigan.org/docs/device-init.yaml) and replace the `device-init.yaml` on the drive with this new file. Open the `device-init.yaml` file in a text editor. There are four main parts to this configuration: `hostname`, `password`, `ngrok`, `home_assistant`.

There are 12 configurations that need to be changed inside of this file. They have been marked with “[CHANGE ME]”. I’m assuming you know what each of these values should be. Once you have updated this file, save your changes. Your gateway is now customized!

### Finish
Plug in the SD card into the Raspberry Pi and power it up. The first boot will take about 5 minutes to complete because it is configuring itself. Once the gateway has rebooted twice, it is ready to go.

To confirm that everything is working, you can go to `http://gateway.local:8123/` in your web browser. Home Assistant’s interface should come up. Or if you have ngrok setup, you can connect remotely through `https://[subdomain].ngrok.io` replacing `[subdomain]` with the subdomain you configured ngrok with.


#research/prisms
#documentation
