# Hiding Unwanted Sensors in Home Assistant

The Kessel Staufix Control creates multiple sensors in Home Assistant due to the standard A5-30-03 EEP profile. However, only the **WA0 (Alarm)** sensor is actually useful for this device.

## Sensors Created

The addon creates these sensors:
- ✅ **binary_sensor.e2m_staufix_control_wake** (WA0) - **This is the ALARM sensor you want**
- ❌ binary_sensor.e2m_staufix_control_di0 - Not used
- ❌ binary_sensor.e2m_staufix_control_di1 - Not used
- ❌ binary_sensor.e2m_staufix_control_di2 - Not used
- ❌ binary_sensor.e2m_staufix_control_di3 - Not used
- ✅ sensor.e2m_staufix_control_rssi - Signal strength (useful)
- ✅ sensor.e2m_staufix_control_last_seen - Last update (useful)
- ❌ sensor.e2m_staufix_control_t_raw - Not used
- ❌ sensor.e2m_staufix_control_tempc - Not used

## How to Hide Unwanted Sensors

### Option 1: Disable in Home Assistant UI (Recommended)

1. Go to **Settings** → **Devices & Services** → **MQTT**
2. Find your **e2m_staufix_control** device
3. Click on it to see all entities
4. For each unwanted sensor (DI0, DI1, DI2, DI3, t_raw, tempC):
   - Click on the entity
   - Click the gear icon (⚙️) in the top right
   - Toggle **"Enabled"** to OFF
   - Click **Update**

### Option 2: Rename the Important Sensor

To make it clearer which sensor is the alarm:

1. Go to the **binary_sensor.e2m_staufix_control_wake** entity
2. Click the gear icon (⚙️)
3. Change the **Name** to "Staufix Control Alarm"
4. Change the **Entity ID** to `binary_sensor.staufix_control_alarm`
5. Click **Update**

## Creating Automations

Once you've renamed the alarm sensor, you can create automations:

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
          message: "The Staufix has detected backwater!"
          data:
            priority: high
            ttl: 0
```

## Why Can't We Filter These in the Addon?

The mapping file feature in the enoceanmqtt library has a bug that causes an `AttributeError` when we try to filter sensors. The library's code expects a different mapping format than what we can provide. Until this is fixed in the upstream library, the workaround is to disable unwanted sensors in the Home Assistant UI.

## Summary

**Keep these sensors:**
- `binary_sensor.e2m_staufix_control_wake` → Rename to "Alarm"
- `sensor.e2m_staufix_control_rssi` → Signal strength
- `sensor.e2m_staufix_control_last_seen` → Last update

**Disable these sensors:**
- All DI0-DI3 sensors
- All temperature sensors (t_raw, tempC)
