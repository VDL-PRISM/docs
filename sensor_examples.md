# Sensor Examples

To add sensor support to your gateway, you must add to the `home_assistant_configuration` part of the `device-init.yaml` file. Here is an example of `device-init.yaml`:


```
hostname: gateway
password: "[CHANGE ME]"

ngrok:
  authtoken: "[CHANGE ME]"
  tunnels:
    ssh:
      proto: tcp
      addr: 22
      remote_addr: "[CHANGE ME]"
    ui:
      proto: http
      subdomain: "[CHANGE ME]"
      addr: 8123

home_assistant_configuration:
  homeassistant:
    name: Home
    unit_system: imperial
    time_zone: America/Denver

  frontend:

  http:
    api_password: "[CHANGE ME]"

  logger:
    default: warning
    logs:
      homeassistant.bootstrap: info
      homeassistant.util.package: info
      custom_components.prisms_influxdb: info
      custom_components.sensor.air_quality: info
      coapthon: warning

  prisms_influxdb:
    host: "[CHANGE ME]"
    port: "[CHANGE ME]"
    username: "[CHANGE ME]"
    password: "[CHANGE ME]"
    ssl: "[CHANGE ME]"
    verify_ssl: "[CHANGE ME]"
    home_id: "[CHANGE ME]"
    batch_time: 10
    blacklist:
      - persistent_notification.invalid_config

  [...sensor configuration goes here...]

```

Notice at the end of the file is where the sensor configuration goes. Below are a few examples of how to set up a sensor.


## WeMo Devices

[Wemo Insight Smart Plug](http://www.belkin.com/us/p/P-F7C029/)

```
home_assistant_configuration:
  ...other components...

  wemo:
```

The WeMo component automatically detects WeMo devices so no more configuration is required.


## Z-Wave Devices

To use a Z-Wave device, you need a USB Z-Wave adapter, such as [this](https://www.amazon.com/Aeotec-Aeon-Labs-ZW090-Stick/dp/B00X0AWA6E/ref=sr_1_3?ie=UTF8&qid=1494883458&sr=8-3&keywords=aeotec). Here is an example of a Z-Wave sensor: [Z-Wave Multisensor](https://www.amazon.com/Aeotec-Aeon-Labs-ZW100-Multisensor/dp/B0151Z8ZQY/ref=sr_1_1?ie=UTF8&qid=1494883458&sr=8-1-spons&keywords=aeotec&psc=1&smid=A13BNE3P7C8THK)

```
home_assistant_configuration:
  ...other components...

  zwave:
```

The Z-Wave component automatically detects Z-Wave devices so no more configuration is required.

## Utah Modified Dylos Devices

```
home_assistant_configuration:
  ...other components...

  sensor:
    - platform: air_quality
      update_time: 15
      discover_time: 60
```
