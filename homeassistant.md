# Home Assistant configuration

Integration into Home assistant is possible using the natively provided MQTT HVAC integration of HA. As the MQTT topic for the states is different than the inputs for temperature and mode, automations are needed to re-write the MQTT messages.
## General structure:
 1. MQTT HVAC configuration
 2. Helper automation for rewriting MQTT commands for temperature control
 3. Helper automation for rewriting MQTT commands for mode selection

### MQTT HVAC configuration
Adjust the name, MQTT topics and MAC-address to your esp32_mqtt_eq3 configuration. 

```
   - platform: mqtt
     name: Badezimmer
    device:
     identifiers: '00:1A:22:0B:42:94'
    modes:
      - "off"
      - "auto"
      - "heat"
    precision: 0.5
    min_temp: 4.5
    max_temp: 29.5
    temp_step: 0.5
    mode_state_topic: "/badezimmerradout/status"
    mode_state_template: >
      {% if value_json.mode == "manual" %}
        {% if value_json.temp == "4.5" %}
          off
        {% else %}
          heat
        {% endif %}
      {% elif value_json.mode == "auto" %}
        auto
      {% endif %}
    mode_command_topic: "/ha_badezimmerradin/trv_mode"
    temperature_state_topic: "/badezimmerradout/status"
    temperature_state_template: "{{ value_json.temp }}"
    temperature_command_topic: "/ha_badezimmerradin/trv_temp"
    current_temperature_topic: "/badezimmerradout/status"
    current_temperature_template: "{{ value_json.temp }}"
    away_mode_state_topic: "/badezimmerradout/status"
    away_mode_state_template: >
      {% if value_json.mode == "holiday" %}
        on
      {% else %}
        off
      {% endif %}
      
```

### Temperature Helper
Here as well, adjust the MQTT topics and BT names to your configuration. Add it to the automations.yaml

```
- id: '1573053873261'
  alias: Badezimmer_Temperature_Helper
  description: ''
  trigger:
  - platform: mqtt
    topic: /ha_badezimmerradin/trv_temp
  condition: []
  action:
    service: mqtt.publish
    data_template:
      payload: "{% if trigger.payload == \"4.5\" %}\n  00:1A:22:0B:42:94 off\n{% else\
        \ %}\n  00:1A:22:0B:42:94 settemp {{trigger.payload}}\n{% endif %}"
      topic: /badezimmerradin/trv
```

### Mode Helper
Adjust the automation to fit your environment and append it to your automations.yaml.

```
- id: '1573063154330'
  alias: Badezimmer_Mode_Helper
  description: ''
  trigger:
  - platform: mqtt
    topic: /ha_badezimmerradin/trv_mode
  condition: []
  action:
    service: mqtt.publish
    data_template:
      payload: "{% if trigger.payload == \"heat\" %}\n  00:1A:22:0B:42:94 manual\n\
        {% else %}\n  00:1A:22:0B:42:94 {{trigger.payload}}\n{% endif %}"
      topic: /badezimmerradin/trv
```
