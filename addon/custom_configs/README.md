# Custom Configuration Files for Kessel Staufix Control

This directory contains custom configuration files for the Kessel Staufix Control device that uses the A5-3F-7F EEP profile.

## Files Included

1. **EEP.xml** - Modified EEP definition file that includes the A5-3F-7F profile for the Kessel Staufix Control
2. **enoceanmqtt.devices** - Device configuration file with the correct EEP profile mapping
3. **mapping.yaml** - Home Assistant mapping configuration for the device sensors

## How to Use

These files are automatically copied to `/data/custom_configs/` when the addon starts.

To use them in your configuration:

1. Set the device file path in the addon configuration to: `/data/custom_configs/enoceanmqtt.devices`
2. Set the EEP file path in the addon configuration to: `/data/custom_configs/EEP.xml`
3. Set the mapping file path in the addon configuration to: `/data/custom_configs/mapping.yaml`

## Device Configuration

The device configuration includes:
- Device ID: 04:1D:2C:8C
- EEP: A5-3F-7F
- Manufacturer: KESSEL
- Model: Staufix Control

## Sensors Available

The following sensors will be available in Home Assistant:
- Water Level (0-100%)
- Pump Status (On/Off)
- Alarm Status (On/Off)
- Battery Status (0-100%)

## Changes Made

The A5-3F-7F profile was added to support the Kessel Staufix Control device with proper data point mappings for all sensors.
