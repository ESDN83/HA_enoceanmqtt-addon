# :warning: use https://github.com/ESDN83/ha-enocean-mqtt-slim instead. This version was only a test


# Home Assistant EnOcean MQTT Addon - Kessel Staufix Control Edition

This is a modified version of the [HA_enoceanmqtt addon](https://github.com/ChristopheHD/HA_enoceanmqtt-addon) with specific support for the **Kessel Staufix Control** backwater alarm system.

## Features

✅ Support for Kessel Staufix Control (EEP A5-30-03)  
✅ Automatic MQTT discovery in Home Assistant  
✅ Real-time backwater alarm notifications  
✅ Fixes for USB stick base ID communication issues  
✅ Pre-configured device and EEP profiles  

## What is Kessel Staufix Control?

The Kessel Staufix Control is an EnOcean-based backwater alarm system that monitors for sewage backflow. It sends wireless telegrams when water is detected in the drainage system.

## Installation

### Step 1: Add Repository to Home Assistant

1. Open Home Assistant
2. Go to **Settings** → **Add-ons** → **Add-on Store**
3. Click the three dots menu (⋮) in the top right
4. Select **Repositories**
5. Add this URL: `https://github.com/ESDN83/HA_enoceanmqtt-addon`
6. Click **Add**

### Step 2: Install the Addon

1. Find "EnOcean MQTT (Kessel Staufix)" in the Add-on Store
2. Click **Install**
3. Wait for installation to complete (builds Docker image locally)

### Step 3: Configure the Addon

Go to the **Configuration** tab and set:

```yaml
enocean_port: /dev/serial/by-id/usb-EnOcean_GmbH_EnOcean_USB_300_DC_FT55ZDV5-if00-port0
device_file: /data/custom_configs/enoceanmqtt.devices
mqtt:
  host: core-mosquitto
  port: 1883
  user: your_mqtt_user
  pwd: your_mqtt_password
  discovery_prefix: homeassistant
  prefix: enoceanmqtt
  client_id: enocean_gateway
  keepalive: 60
mapping_files:
  eep_file: /data/custom_configs/EEP.xml
  mapping_file: ""
logging:
  debug: false
  log_packets: false
  log_file: /config/enoceanmqtt.log
```

**Important:** Adjust the `enocean_port` to match your USB stick path. Find it in **Settings** → **System** → **Hardware**.

### Step 4: Configure Your Device

Edit the device file at `/data/custom_configs/enoceanmqtt.devices` to match your Kessel Staufix Control device ID:

```ini
[staufix_control]
address = 0x05834fa4  # Replace with your device ID
rorg = 0xA5
func = 0x30
type = 0x03
sender = 0xffd1f401  # Replace with your gateway base ID + offset
```

### Step 5: Add Manual Sensor Configuration

The auto-discovered sensors don't work correctly due to field name mismatches. Add this to your Home Assistant `configuration.yaml`:

```yaml
mqtt:
  binary_sensor:
    - name: "Staufix Control Alarm"
      unique_id: "staufix_control_alarm_manual"
      state_topic: "enoceanmqtt/staufix_control"
      value_template: "{% if value_json.AL == 0 %}ON{% else %}OFF{% endif %}"
      device_class: problem
      icon: mdi:pipe-valve
      device:
        identifiers: ["05834FA4"]  # Replace with your device ID
        name: "Kessel Staufix Control"
        model: "Staufix Control"
        manufacturer: "Kessel"
```

**Note:** The logic is inverted because the Kessel device sends `AL=0` when water is detected (alarm condition) and `AL=1` when no water (normal).

### Step 6: Start and Verify

1. Start the addon
2. Check logs for successful connection
3. Trigger the alarm to test
4. Verify the sensor updates in Home Assistant

## Troubleshooting

### "Waiting for device base ID"

This addon includes a patch that automatically sets a default base ID if the USB stick doesn't respond. If you still see this message indefinitely:

1. Restart Home Assistant completely
2. Ensure no other software (like ioBroker) is using the USB stick
3. Check that the USB stick is properly connected

### Sensors Show "Unknown"

1. Verify telegrams are being received in the addon logs
2. Check MQTT messages in Developer Tools → MQTT (listen to `enoceanmqtt/#`)
3. Ensure the manual sensor configuration is added to `configuration.yaml`
4. Restart Home Assistant after adding the configuration

### Wrong Device ID

Find your device ID by:
1. Enable `log_packets: true` in addon configuration
2. Trigger the alarm
3. Check logs for telegrams from your device
4. Update the device ID in the configuration

## Creating Automations

Example automation for backwater alarm notifications:

```yaml
automation:
  - alias: "Backwater Alarm Notification"
    trigger:
      - platform: state
        entity_id: binary_sensor.staufix_control_alarm
        to: "on"
    action:
      - service: notify.mobile_app
        data:
          title: "⚠️ Backwater Alarm!"
          message: "The Staufix has detected backwater in the drainage system!"
          data:
            priority: high
            ttl: 0
```

## Technical Details

### What's Different from the Original Addon?

1. **Custom EEP Profile:** Added A5-30-03 profile definition for Kessel Staufix Control
2. **Base ID Patch:** Runtime patch to bypass USB stick base ID query issues
3. **Pre-configured Device:** Includes sample device configuration
4. **Documentation:** Comprehensive setup guide for Kessel devices

### Files Included

- `addon/custom_configs/EEP.xml` - Custom EEP profile for A5-30-03
- `addon/custom_configs/enoceanmqtt.devices` - Sample device configuration
- `addon/skip_base_id.patch` - Patch to fix base ID wait issue
- `MANUAL_SENSOR_CONFIG.yaml` - Working sensor configuration

## Support

For issues specific to this Kessel Staufix edition:
- Open an issue on [GitHub](https://github.com/ESDN83/HA_enoceanmqtt-addon/issues)

For general EnOcean MQTT questions:
- See the [original addon documentation](https://github.com/ChristopheHD/HA_enoceanmqtt-addon)

## Credits

- Original addon by [ChristopheHD](https://github.com/ChristopheHD/HA_enoceanmqtt-addon)
- Kessel Staufix Control support added by ESDN83
- Based on [enocean-mqtt](https://github.com/embyt/enocean-mqtt) by embyt

## License

Same as the original addon - see LICENSE file.
