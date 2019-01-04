# EpiFi HTTP Documentation
## Data Format
The endpoint is `/data`. You must `POST` JSON data to that endpoint. If an error occurs, a 400 status code and JSON message will be returned with one field, `error`, that contains an error message. If the `POST` is successful, 204 status code will be returned with no content.

For authentication, the API uses basic authentication, so a username and password must be provided with each request.

The JSON data must be formatted in the following way. There are three required keys: `sensor_id`, `sample_time`, and `data`. There is an optional key, `metadata`. The `sensor_id` must be a unique ID of the sensor that the data was measured from.  The unique ID must be ASCII and *cannot* contain spaces and the following characters `/`, `+` or `#`. The `sample_time` field contains the time the data was sampled, as a Unix timestamp **in microsecond**. The `data` field contains a dictionary, where the keys are a measurement names and the values is the measured values. For example, a temperature and humidity sensor would have the following data field:

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
    "sensor_id": [sensor id],
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
    "sensor_id": "9dd45376d1bd4d68ae5c9ecdc5747436",
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

Here is an example using `curl`:

```bash
curl \
    --header "Content-Type: application/json" \
    -u <username>:<password> \
    -d '{ "sensor_id": "9dd45376d1bd4d68ae5c9ecdc5747436", "sample_time": 1546295488000000, "data": {"temperature": 70, "humidity": 42}, "metadata": {"firmware": "v1.0", "model": "abc"}}
' \
    <server>/data
```

Replacing `<username>` and `<password>` with the username and password of the service, and replacing `<server>` with the server name.

## Metadata
The metadata field is designed for values that do not change very often, like the firmware version. **Do not put values that change often in the metadata field**. This includes timestamps, measurements, hashes that change often, etc. This will have a negative impact on the performance of your database.

## Sensor Registration
Sensors must be registered with EpiFi before it will start to take data from it. To register a sensor, you must add it to a deployment inside of MongoDB.
