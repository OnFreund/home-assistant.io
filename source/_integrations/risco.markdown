---
title: Risco Alarm
description: Instructions on how to integrate Risco alarms into HA using Risco Cloud.
ha_category:
  - Alarm
  - Binary Sensor
ha_release: "0.115"
ha_iot_class: Cloud Polling
ha_config_flow: true
ha_codeowners:
  - "@OnFreund"
ha_domain: risco
---

This integration connects with Risco Alarms over [Risco Cloud](https://riscocloud.com/).

## Configuration

This integration can be configured using the integrations panel in the
Home Assistant frontend.

Menu: **Configuration** -> **Integrations**.

Click on the `+` sign to add an integration and click on **Risco**.
You will be prompted for your username, password, and pin code (you can create a specific user for this purpose).
An Alarm Control Panel entity will be created for each partition in your site, and binary sensors for each of your zones.

Additionally, 4 sensors will be created to store events, depending on the category (Status, Alarm, Trouble and Other). Each sensor
has the event timestamp as the state, and other event information in attributes.

If you have multiple sites, only the first site will be used.

## Options

You can configure additional behavior by clicking on **Options** in the relevant box in the Integration panel:

{% configuration_basic %}
How often to poll Risco (in seconds):
  description: "The lower this is, the faster your entities will reflect changes, but the more resource-intensive it'll be."
Require pin code to arm:
  description: When checked, you'll need to enter your pin code when arming through Home Assistant.
Require pin code to disarm:
  description: When checked, you'll need to enter your pin code when disarming through Home Assistant.
{% endconfiguration_basic %}

Apart from these options, you can also define a custom mapping between your Home Assistant Alarm states and the Risco arming modes.
This is an advanced configuration, and unless you're using group arming, the default mapping should probably be best.
This is a two-way mapping, meaning you can map:

- What Home Assistant state your partition entity will report when Risco is armed in a specific mode.
- Which arming mode to use when arming from Home Assistant using one of its modes. Note that in this step, you can only choose combinations that map to each other in the previous step.

The default mapping:

|Risco Arming Mode | Home Assistant State |
|---|---|
| Arm (AWAY) | Armed Away |
| Partial Arm (STAY) | Armed Home |
| Group A | Armed Home |
| Group B | Armed Home |
| Group C | Armed Home |
| Group D | Armed Home |

And in the reverse direction:

| Home Assistant Mode | Risco Arming Mode |
|---|---|
| Arm Away | Arm |
| Arm Home | Partial Arm |

## Services

### Service `risco.bypass_zone`

This service marks a zone as bypassed so that the alarm isn't triggered when the zone is triggered.

Note you can only bypass a zone when the partitions it belongs to are disarmed, and it will take effect next time you arm.

Risco automatically un-bypasses the zone after the alarm is disarmed again.

| Service Data Attribute | Required | Description |
| ---------------------- | -------- | ----------- |
| `entity_id`            | no     | String or list of strings of entity_ids of zones. Use entity_id: all to target all zones. |

### Service `risco.unbypass_zone`

This undoes a zone bypass. You can only unbypass a zone when the partitions it belongs to are disarmed.

| Service Data Attribute | Required | Description |
| ---------------------- | -------- | ----------- |
| `entity_id`            | no     | String or list of strings of entity_ids of zones. Use entity_id: all to target all zones. |

## Supported Plaforms:

- [Alarm Control Panel](/integrations/alarm_control_panel/)
- [Binary Sensor](/integrations/binary_sensor/)
