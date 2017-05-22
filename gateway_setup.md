# Gateway

## Introduction

The gateway is the central hub of communication for a PRISMS sensor system. Sensors transmit their data to a gateway and the gateway takes care of uploading the data to the a remote database:

![High Level Architecture](images/High-Level-Architecture.pdf)

The gateway consists of multiple components. The major component is [Home Assistant](https://home-assistant.io). Home Assistant is the common foundation that all sensors must communicate with. Out of the box, Home Assistant supports many different types of sensors, with the ability to add custom sensors. The PRISMS sensor component communicates with PRISMS sensors, integrating them into Home Assistant. The PRISMS InfluxDB component takes any data that is given to Home Assistant and uploads it to a [InfluxDB server](https://github.com/influxdata/influxdb). The PRISMS InfluxDB component also provides data persistence in case the gateway loses power and has not been able to upload all of the collected data. The source for the PRISMS components can be found [here](https://github.com/VDL-PRISM/home-assistant-components).  The last component, [ngrok](https://ngrok.com) is a service that allows you to connect to your gateway after it has been deployed. This is helpful for remote debugging and monitoring. It is not required.

![Gateway Components](images/Gateway.pdf)

We have built a custom disk image to simplify the building and setting up a gateway. The image comes pre-installed with the PRISMS components and other helpful software. The following instructions outline how to build and set up a gateway.

_Note: We assume that you have access to a server running InfluxDB to upload data._

## Gateway Parts
- [Raspberry Pi 3](https://www.raspberrypi.org/products/raspberry-pi-3-model-b/)
- Micro USB power supply ([example](https://www.adafruit.com/product/1995))
- microSD card -- at least 8 GB, 32 GB is recommended ([example](https://www.amazon.com/dp/B010Q57T02/ref=twister_B011BRUOMO))
- Computer with SD card reader
- Case for Raspberry Pi (optional)

![Parts for gateway](images/parts.jpg)

## Building a Gateway

To build a gateway, you must first download and flash the custom disk image to the SD card. Then you must customize the disk image for your specific deployment. Lastly, you build the gateway.

### Download the Image
Download the PRISMS Gateway image from [here](https://github.com/VDL-PRISM/gateway-image-builder/releases/download/v2.0.0/v2.0.0-prisms-gateway.img.zip).

### Writing Image to the SD Card
In order to copy the image file to an SD card, you need special software that "flashes" the SD card with the disk image. Many tools exist, but we like an app called [Etcher](https://etcher.io).

Once you have Etcher downloaded and installed, plug in your SD card into your computer and run Etcher.

![SD card plugged into computer](images/sd_card.jpg)

Select the image PRISMS image you downloaded. Etcher detects if an SD card is plugged in and selects the correct drive for you. Confirm that this is the correct drive. Lastly, click the “Flash!” button. Once the process is complete, remount the SD card by unplugging the SD card and plugging it back in.

![Flashing the SD card using Etcher](images/etcher.png)

### Configure SD Card
With the SD card mounted, open up the “boot” drive and locate the `device-init.yaml` file in the top level directory.

![Boot partition of SD card](images/boot.png)

![`device-init.yaml` file on SD card](images/device-init.png)

This file allows you to customize your gateway for a deployment. Each time the gateway starts, it looks at this file and configures itself. The configuration file is a [YAML file](http://yaml.org) which has a specific format and syntax. If you are unfamiliar with YAML, you should take a few minutes to familiarize yourself. It is not a complicated format, but it is worth understanding. There are lots of resources online for learning more about it ([here](https://learnxinyminutes.com/docs/yaml/) and [here](https://www.youtube.com/watch?v=W3tQPk8DNbk)).

There are four main parts to this file: `hostname`, `password`, `ngrok`, `home_assistant`.

![Contents of `device_init.yaml`](images/config.png)

- `hostname` changes the name of the computer. For most cases, the default ("gateway") works fine. Be careful not to include a [underscore in the hostname](https://en.wikipedia.org/wiki/Hostname#Restrictions_on_valid_hostnames).

- `password` changes the password of the gateway computer. **Make sure to change this.**

- `ngrok` allows you to configure the [ngrok service](https://ngrok.com), which allows you to connect to your gateway from anywhere. To read more about how to configure ngrok, see the [ngrok documentation](https://ngrok.com/docs#config). If you don't plan on using ngrok, then you can delete the whole `ngrok` section.

- `home_assistant_configuration` is where all configuration for Home Assistant goes. Home Assistant is broken into different components. The default `device-init.yaml` provides a few basic components, one of which uploads data to an InfluxDB server (called `prisms_influxdb`). Enter the InfluxDB server information into this part of the configuration. We assume that you already have set up an InfluxDB instance with a username and password. To make the gateway useful, you must add sensors to this part of the configuration. This tells Home Assistant what sensors to pull data from. A list of all components and how to set them up is described in the [Home Assistant documentation](https://home-assistant.io/components). We have given a few examples of common sensors [here](sensor_examples.md). Add any sensor configuration to the end of this section.

The default `device-init.yaml` has 12 places that need to be changed, as marked by "[CHANGE ME]". Once you have updated these 12 locations and added some sensors to the Home Assistant configuration, save your changes. Your gateway is now customized!

If you are new to YAML, it might be a good idea to run the file through a YAML Linter to make sure you didn't make any mistakes in the format. Copy the contents of the file and paste it into [this website](http://www.yamllint.com). It will tell you if your YAML format is correct or not.

### Build Gateway
Unmount the SD card from your computer and plug it into the Raspberry Pi. If you have a case, put the case on the Raspberry Pi. Plug the gateway into an Ethernet cable that is connected to the Internet, then connect the power cable. The first time the gateway boots, it must be connected to the Internet. The gateway's first boot will take about 15 minutes to complete.

### Finish
Home Assistant provides a web interface that shows the values of all the sensors connected. To access this web interface, go to `http://[gateway-ip-address]:8123`. If you are using a Mac, Linux, or Windows computer with iTunes[^1], you can use the gateway's hostname to connect to it. If you left the hostname as `gateway`, then you could go to [`http://gateway.local:8123`](http://gateway.local:8321). You must be on the same network as the gateway for this to work.

If you have ngrok setup, you can connect remotely through `https://[subdomain].ngrok.io` replacing `[subdomain]` with the subdomain you configured ngrok with.


[^1]: Using `.local` requires that you have Bonjour or zeroconf running on your computer. This is turned on by default for Mac and Linux computers. Installing iTunes on a Windows computer also turns on this feature.
