# EpiFi MQTT Documentation

## Data Format
Sensors publish data on the following topic: `epifi/v1/<sensor_id>` where the `<sensor_id>` is a unique ID for the sensor. The unique ID must be ASCII and *cannot* contain spaces and the following characters `/`, `+` or `#`.

The data published must be formatted as JSON. There are two required keys: `sample_time` and `data`., with an optional key, `metadata`. The `sample_time` field contains the time the data was sampled, as a Unix timestamp **in microsecond**. The `data` field contains a dictionary, where the keys are measurement names and the values are the measured values. For example, a temperature and humidity sensor would have the following data field:

```json
{
    "temperature": 70,
    "humidity": 42
}
```

The metadata field is a dictionary that contains arbitrary data that is specific to the sensor. For example, it can contain the firmware version of the sensor.

Here is the general structure of the data:

```json
{
    "sample_time": [Unix time in microseconds],
    "data": {
        ...[measured data]...
    },
    "metadata": {
        ..[optional metadata]...
    }
}
```

And here is an example:

```json
{
    "sample_time": 1546295488000000,
    "data": {
        "temperature": 70,
        "humidity": 42
    },
    "metadata": {
        "firmware": "v1.0",
        "model": "abc"
    }
}
```

## Metadata
The metadata field is designed for values that do not change very often, like the firmware version. **Do not put values that change often in the metadata field**. This includes timestamps, measurements, hashes that change often, etc. This will have a negative impact on the performance of your database.


## Users
Before a sensor can publish data, it must be added as a user. *The sensor ID must be the username of the sensor.* To add a user, run the following command:

```
python cli.py mqtt_add_sensor <sensor_id>
```

This will generate a password for you and set up Mosquitto with that username and password. The default access control for EpiFi is a sensor is only able to publish on its own topic (`epifi/v1/<username>`).

## Sensor Registration
Sensors must be registered with EpiFi before it will start to take data from it. To register a sensor, you must add it to a deployment inside of MongoDB.
