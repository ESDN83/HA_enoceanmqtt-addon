# Kessel Staufix Control Integration Summary

## What We've Done

1. **Forked the Repository**: You've forked the HA_enoceanmqtt-addon to https://github.com/ESDN83/HA_enoceanmqtt-addon

2. **Added Custom Configuration Files**:
   - `addon/custom_configs/EEP.xml` - Contains the A5-3F-7F profile definition
   - `addon/custom_configs/enoceanmqtt.devices` - Pre-configured device file for your Kessel Staufix Control
   - `addon/custom_configs/mapping.yaml` - Home Assistant sensor mappings
   - `addon/custom_configs/README.md` - Documentation for the custom configs

3. **Modified the Dockerfile**: Updated to copy custom configuration files to `/data/custom_configs/` in the container

4. **Updated Documentation**: Added information about the Kessel Staufix Control support to the addon README

5. **Committed Changes**: All changes are committed locally with a descriptive commit message

## Next Steps

### 1. Push to GitHub

You need to authenticate and push the changes. See `GITHUB_AUTH_GUIDE.md` for detailed instructions.

Quick command (after getting your Personal Access Token):
```bash
cd /Users/erik.seel/AI/HA_enoceanmqtt-addon
git push https://ESDN83:YOUR_TOKEN_HERE@github.com/ESDN83/HA_enoceanmqtt-addon.git master
```

### 2. Install in Home Assistant

1. Add your repository to Home Assistant:
   - Settings → Add-ons → Add-on Store → ⋮ → Repositories
   - Add: `https://github.com/ESDN83/HA_enoceanmqtt-addon`

2. Install the "EnOcean to MQTT Bridge" addon from your repository

3. Configure the addon with these settings:
   ```yaml
   device_file: /data/custom_configs/enoceanmqtt.devices
   mapping_files:
     eep_file: /data/custom_configs/EEP.xml
     mapping_file: /data/custom_configs/mapping.yaml
   ```

### 3. Verify the Device

After starting the addon, your Kessel Staufix Control should appear in Home Assistant with these sensors:
- `sensor.kessel_staufix_control_water_level` (0-100%)
- `binary_sensor.kessel_staufix_control_pump_status` (on/off)
- `binary_sensor.kessel_staufix_control_alarm_status` (on/off)
- `sensor.kessel_staufix_control_battery` (0-100%)

## Device Information

- **Device ID**: 04:1D:2C:8C
- **EEP**: A5-3F-7F
- **Manufacturer**: KESSEL
- **Model**: Staufix Control

## Troubleshooting

If the device doesn't appear:
1. Check the addon logs for any errors
2. Verify the device ID matches your actual device
3. Ensure the EnOcean USB stick is properly connected
4. Check that MQTT is properly configured

## Files Modified

- `addon/Dockerfile` - Added COPY command for custom configs
- `addon/README.md` - Added fork information
- `addon/custom_configs/*` - All new configuration files

Your forked addon now fully supports the Kessel Staufix Control device with the correct A5-3F-7F EEP profile!
