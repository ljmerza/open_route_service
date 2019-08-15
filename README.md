# open_route_service

[![GitHub Release][releases-shield]][releases]
[![GitHub Activity][commits-shield]][commits]
[![hacs_badge](https://img.shields.io/badge/HACS-Default-orange.svg?style=for-the-badge)](https://github.com/custom-components/hacs)
[![License][license-shield]](LICENSE.md)

![Project Maintenance][maintenance-shield]
[![BuyMeCoffee][buymecoffeebadge]][buymecoffee]

[![Community Forum][forum-shield]][forum]

_Homeassistant Custom Component sensor provides travel time from [openrouteservices.org](openrouteservices.org)._

**This component will set up the following platforms.**

Platform | Description
-- | --
`sensor` | Show travel time between two places.

![example][exampleimg]

## Setup

You need to register for an API key [here](https://openrouteservice.org/dev).

Openroute Services offers a Free Plan which includes 1.000 free requests (For reverse geocoding) per day. More information can be found [here](https://openrouteservice.org/plans/)

##  Configuration

To enable the sensor, add the following lines to your `configuration.yaml` file:

```yaml
# Example entry for configuration.yaml
sensor:
  - platform: open_route_service
    api_key: "YOUR_API_KEY"
    origin_latitude: "51.222975"
    origin_longitude: "9.267577"
    destination_latitude: "51.257430"
    destination_longitude: "9.335892"
```

## Configuration options

Key | Type | Required | Description
-- | -- | -- | --
`api_key` | `string` | `true` | Your application's API key (get one by following the instructions above).
`origin_latitude` | `string` | `true` | The starting latitude for calculating travel distance and time. Must be used in combination with origin_longitude. Cannot be used in combination with origin_entity_id
`origin_longitude` | `string` | `true` | The starting longitude for calculating travel distance and time. Must be used in combination with origin_latitude. Cannot be used in combination with origin_entity_id
`destination_latitude` | `string` | `true` | The finishing latitude for calculating travel distance and time. Must be used in combination with destination_longitude. Cannot be used in combination with destination_entity_id
`destination_longitude` | `string` | `true` | The finishing longitude for calculating travel distance and time. Must be used in combination with destination_latitude. Cannot be used in combination with destination_entity_id
`origin_entity_id` | `string` | `true` | The entity_id holding the starting point for calculating travel distance and time. Cannot be used in combination with origin_latitude / origin_longitude
`destination_entity_id` | `string` | `true` | The entity_id holding the finishing point for calculating travel distance and time. Cannot be used in combination with destination_latitude / destination_longitude
`name` | `string` | `false` | A name to display on the sensor. The default is "HERE Travel Time".
`mode` | `string` | `false` | You can choose between: `cycling-regular`, `driving-car` or `foot-walking`. The default is `driving-car`.
`route_mode` | `string` | `false` | You can choose between: `fastest`, or `shortest`. The default is `fastest`
`unit_system` | `string` | `false` | You can choose between `metric` or `imperial`. Defaults to `metric` or `imperial` based on the Home Assistant configuration.
`scan_interval` | `integer` | `false` | "Defines the update interval of the sensor in seconds. Defaults to 300 (5 minutes)."

## Dynamic Configuration

Tracking can be set up to track entities of type `device_tracker`, `zone`, `sensor` and `person`. If an entity is placed in the origin or destination then every 5 minutes when the platform updates it will use the latest location of that entity.

```yaml
# Example entry for configuration.yaml
sensor:
  # Tracking entity to entity
  - platform: here_travel_time
    app_id: "YOUR_APP_ID"
    app_code: "YOUR_APP_CODE"
    name: Phone To Home
    origin_entity_id: device_tracker.mobile_phone
    destination_entity_id: zone.home
```

## Entity Tracking

- **device_tracker**
  - If the state is a zone, then the zone location will be used
  - If the state is not a zone, it will look for the longitude and latitude attributes
- **zone**
  - Uses the longitude and latitude attributes
- **sensor**
  - If the state is a zone, then will use the zone location
  - All other states will be passed directly into the HERE API
    - This includes all valid locations listed in the *Configuration Variables*

## Updating sensors on-demand using Automation

You can also use the `homeassistant.update_entity` service to update the sensor on-demand. For example, if you want to update `sensor.morning_commute` every 2 minutes on weekday mornings, you can use the following automation:

```yaml
automation:
- id: update_morning_commute_sensor
  alias: "Commute - Update morning commute sensor"
  initial_state: 'on'
  trigger:
    - platform: time_pattern
      minutes: '/2'
  condition:
    - condition: time
      after: '08:00:00'
      before: '11:00:00'
    - condition: time
      weekday:
        - mon
        - tue
        - wed
        - thu
        - fri
  action:
    - service: homeassistant.update_entity
      entity_id: sensor.morning_commute
```

<a href="https://www.buymeacoffee.com/eifinger" target="_blank"><img src="https://www.buymeacoffee.com/assets/img/custom_images/black_img.png" alt="Buy Me A Coffee" style="height: auto !important;width: auto !important;" ></a><br>

[buymecoffee]: https://www.buymeacoffee.com/eifinger
[buymecoffeebadge]: https://img.shields.io/badge/buy%20me%20a%20coffee-donate-yellow.svg?style=for-the-badge
[commits-shield]: https://img.shields.io/github/commit-activity/y/eifinger/open_route_service?style=for-the-badge
[commits]: https://github.com/eifinger/open_route_service/commits/master
[customupdater]: https://github.com/custom-components/custom_updater
[customupdaterbadge]: https://img.shields.io/badge/custom__updater-true-success.svg?style=for-the-badge
[exampleimg]: https://github.com/eifinger/open_route_service/blob/master/example.png?raw=true
[forum-shield]: https://img.shields.io/badge/community-forum-brightgreen.svg?style=for-the-badge
[forum]: https://community.home-assistant.io/t/custom-component-open-route-service-travel-time/131941
[license-shield]: https://img.shields.io/github/license/eifinger/open_route_service.svg?style=for-the-badge
[maintenance-shield]: https://img.shields.io/badge/maintainer-Kevin%20Eifinger%20%40eifinger-blue.svg?style=for-the-badge
[releases-shield]: https://img.shields.io/github/release/eifinger/open_route_service.svg?style=for-the-badge
[releases]: https://github.com/eifinger/open_route_service/releases