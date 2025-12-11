# Home Assistant Add-on: EnOcean to MQTT Bridge

EnOcean to MQTT Bridge

![Supports aarch64 Architecture][aarch64-shield] ![Supports amd64 Architecture][amd64-shield]

Access your EnOcean devices from Home Assistant through MQTT.

![downloads](https://img.shields.io/badge/dynamic/json?color=41BDF5&logo=home-assistant&label=addon%20usage&suffix=%20active%20installs&cacheSeconds=15600&url=https://analytics.home-assistant.io/addons.json&query=$.f93730fa_ha_enoceanmqtt_aseracorp.total)

## Fork Information

This fork includes support for the **Kessel Staufix Control** device (EEP A5-3F-7F). Custom configuration files are included in the `/data/custom_configs/` directory:

- Modified EEP.xml with A5-3F-7F profile
- Pre-configured device file for Kessel Staufix Control
- Home Assistant mapping configuration

To use the Kessel Staufix Control device, configure the addon with these paths:
- Device file: `/data/custom_configs/enoceanmqtt.devices`
- EEP file: `/data/custom_configs/EEP.xml`
- Mapping file: `/data/custom_configs/mapping.yaml`

[aarch64-shield]: https://img.shields.io/badge/aarch64-yes-green.svg
[amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg
