# Dylos Documentation
## Download the Image
Download the Dylos image from [here](https://www.files.app.lundrigan.org/prisms-dylos-v1.0.2.img.zip).

## Writing Image to the SD Card
In order to copy the image file to an SD card, you need special software. Download an app called Etcher [here](https://etcher.io).

Once you have Etcher downloaded and installed, plug in your SD card and run Etcher. Select the image that you previously downloaded. Etcher typically detects if an SD card is plugged in and selects the correct drive for you. Confirm that this is the correct drive. Lastly, click the “Flash!” button. Once the process is complete, you can remove the SD card (but keep it handy because we will be using it in the next step).

## Configure SD Card
To configure the SD card, you will need to be able to mount an ext4 file system. This can be done on the Linux operating system. If you don’t have access to a Linux computer, then you can use the Beagle Bone Black (BBB). To use the BBB, you must first power on the BBB *without* the SD card and then plug-in and [mount the SD card](http://askubuntu.com/questions/37767/how-to-access-a-usb-flash-drive-from-the-terminal-how-can-i-mount-a-flash-driv).

In the root directory of the SD card, there should be a `device-init.yaml` file. This file allows you to customize your Dylos. The first time the Dylos runs, it looks for this file and configures itself. This file will only be used on the first boot so any changes made after the first boot will not take affect.

The configuration file is a [YAML file](http://yaml.org) which has a specific format and syntax. If you are unfamiliar with YAML, you should take a few minutes to familiarize yourself. It is not a complicated format, but it is worth understanding. There are lots of resources online for learning more about it ([here](https://learnxinyminutes.com/docs/yaml/) and [here](https://www.youtube.com/watch?v=W3tQPk8DNbk)).

Currently, `device-init.yaml` only supports one configuration option: `hostname`. Edit the file with your choice of hostname. Typically we name a sensor `monitor` followed by its number (i.e. `monitor101`).

## Install & Update Software
Plug in the SD card into the BBB of the Dylos and power it up. The first boot will take about 5 minutes to complete because it is configuring itself. You can tell when it is finished with information is displayed on the LCD screen.

To make sure the Dylos have the most up-to-date software, `ssh` into the Dylos (usually `ssh root@[hostname].local` works fine) and run the following commands:

```
cd ~/dylos
git pull git@github.com:VDL-PRISM/dylos.git
~/pyenv/versions/3.5.2/envs/dylos/bin/pip --no-cache-dir install -r ~/dylos/requirements.txt

cd ~
git clone https://github.com/VDL-PRISM/wifi_connect.git
~/pyenv/versions/3.5.2/envs/dylos/bin/pip --no-cache-dir install -r ~/wifi_connect/requirements.txt
cp ~/wifi_connect/prisms-wifi.service /etc/systemd/system/
systemctl enable prisms-wifi.service
systemctl start prisms-wifi.service
```

These commands update the Dylos software and install a new application for configuring WiFi on the Dylos.

## Configure WiFi
Plug the Dylos through Ethernet in the same network your computer is connected to. Connect directly to the Dylos by going to the following URL: `http://[hostname].local:3210/`. Replace the `[hostname]` with the hostname you configured your Dylos sensor with in the second step.

This should bring up a webpage that allows you to configure the network name and password of the wireless network. Select the wireless network from the list on the left and enter the password on the right. **Note**: Only networks with WPA or WPA2 are supported. Open networks (networks with no password) and WEP networks are not supported for security reasons.

Once the Dylos is connected and an IP address is shown, the Dylos is ready to be connected to a gateway. The gateway will automatically discover the Dyos after about 5 minutes.

#documentation
#research/prisms
