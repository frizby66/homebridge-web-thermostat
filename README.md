# homebridge-web-thermostat

#### Homebridge plugin to control a web-based thermostat

## Description

homebridge-web-thermostat exposes a thermostat to HomeKit and makes it controllable via HTTP requests. The plugin will poll your thermostat at regular intervals and present you with this information when requested. The plugin also allows you so control a number thermostat variables via HomeKit such as the target temperature.

## Installation

1. Install [homebridge](https://github.com/nfarina/homebridge#installation-details)
2. Install this plugin: `npm install -g homebridge-web-thermostat`
3. Update your `config.json` file

## Configuration

```json
"accessories": [
     {
       "accessory": "Thermostat",
       "name": "Thermostat",
       "apiroute": "http://myurl.com"
     }
]
```

### Core
| Key | Description |
| --- | --- |
| `accessory` | Must be `Thermostat` |
| `name` | Name to appear in the Home app |
| `apiroute` | Root URL of your Thermostat device (excluding the rest of the requests) |
| `pollInterval` _(optional)_ | Time (in seconds) between when homebridge will check the `/status` of your thermostat (`60` is default) |

### Optional fields
| Key | Description |
| --- | --- |
| `temperatureDisplayUnits` _(optional)_ | Whether you want °C (`0`) or °F (`1`) as your units (`0` is default) |
| `currentHumidity` _(optional)_ | (`true` or `false`) Whether to include `currentRelativeHumidity` as a field in `/status` (`false` is default) |
| `targetHumidity` _(optional)_ | (`true` or `false`) Whether to include `targetRelativeHumidity` as a field in `/status` and be able to set it via `/targetRelativeHumidity` (`false` is default) |
| `maxTemp` _(optional)_ | Upper bound for the temperature selector in the Home app (`30` is default) |
| `minTemp` _(optional)_ | Lower bound for the temperature selector in the Home app (`15` is default) |
| `enableThresholds` _(optional)_ | (`true` or `false`) whether you want the thermostat accessory to have heating and cooling temperature bounds (`false` is default) |
| `coolingThresholdTemperature` _(optional)_ | Cooling threshold temperature if thresholds are enabled (`30` is default) |
| `heatingThresholdTemperature` _(optional)_ | Heating threshold temperature if thresholds are enabled (`20` is default) |

### Additional options
| Key | Description |
| --- | --- |
| `timeout` _(optional)_ | Time (in milliseconds) until the accessory will be marked as "Not Responding" if it is unreachable (`5000` is default) |
| `http_method` _(optional)_ | The HTTP method used to communicate with the thermostat (`GET` is default) |
| `username` _(optional)_ | Username if HTTP authentication is enabled |
| `password` _(optional)_ | Password if HTTP authentication is enabled |
| `model` _(optional)_ | Appears under "Model" for your accessory in the Home app |
| `serial` _(optional)_ | Appears under "Serial" for your accessory in the Home app |
| `manufacturer` _(optional)_ | Appears under "Manufacturer" for your accessory in the Home app |

## API Interfacing

Your API should be able to:

1. Return thermostat info when it receives `/status` in the JSON format like below:
```
{
    "targetHeatingCoolingState": INT_VALUE_0_TO_3,
    "targetTemperature": FLOAT_VALUE,
    "currentHeatingCoolingState": INT_VALUE_0_TO_2,
    "currentTemperature": FLOAT_VALUE
}
```

**Note:** You must also include the `currentRelativeHumidity` and `targetRelativeHumidity` fields, respectively, if enabled in the `config.json` (read [Configuration](#configuration))

2. Set `targetHeatingCoolingState` when it receives:
```
/targetHeatingCoolingState/{INT_VALUE_0_TO_3}
```

3. Set `targetTemperature` when it receives:
```
/targetTemperature/{INT_VALUE}
```

4. Set `targetRelativeHumidity` (**if enabled in the `config.json`**) when it receives:
```
/targetRelativeHumidity/{FLOAT_VALUE}
```

### HeatingCoolingState Key

| Number | Name |
| --- | --- |
| `0` | Off |
| `1` | Heat |
| `2` | Cool |
| `3` | Auto |
