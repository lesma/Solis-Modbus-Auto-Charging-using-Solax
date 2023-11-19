Copy below automations into your HA instance.

# Installation

1. Go to Settings > Automations & Scenes > "+ Create Automation" > Create new automation

2. Click the three dots top right and Choose Edit in YAML

3. Paste each individual automation below into the code field

4. Hit SAVE and allow the automations to be named exactly as detailed below.  This should happen automatically for you.

## Automation Code

### Flux - Charge
```
alias: Flux - Charge
description: >-
At just before 3pm, this automation charges the batteries to the % SoC defined in "Flux Charge SoC" before the 
expensive import rate from 4-7pm.
trigger:
- platform: time
  at: "14:59:00"
condition:
- condition: numeric_state
  entity_id: sensor.solax_battery_capacity
  below: input_number.flux_charge_soc
action:
- parallel:
    - service: select.select_option
      data:
        option: "15:00"
      target:
        entity_id: select.solax_charger_start_time_2
    - service: select.select_option
      data:
        option: "16:00"
      target:
        entity_id: select.solax_charger_end_time_2
mode: single
```

### Flux - Charge Cutout
```
alias: Flux - Charge Cutout
description: >-
Monitors battery SoC when Flux - Charge automation running.  Stops charge when battery reaches % set in 
"Flux Charge SoC". Resets Charge start and end time to 15:00
trigger:
- platform: numeric_state
  entity_id: sensor.solax_battery_capacity
  above: input_number.flux_charge_soc
condition:
- condition: time
  after: "15:00:00"
  before: "16:00:00"
action:
- parallel:
    - service: select.select_option
      data:
        option: "15:00"
      target:
        entity_id: select.solax_charger_start_time_2
    - service: select.select_option
      data:
        option: "15:00"
      target:
        entity_id: select.solax_charger_end_time_2
mode: single
```
### Flux - Discharge Cutout
```
alias: Flux - Discharge Cutout
description: >-
Stops Flux battery discharging if the remaining SoC drops to 50% between the
hours of 16:00 and 19:00 with Flux discharging enabled
trigger:
- platform: numeric_state
  entity_id: sensor.solax_battery_capacity
  below: input_number.flux_discharge_cutout_soc
condition:
- condition: time
  after: "16:00:00"
  before: "19:00:00"
  weekday:
    - mon
    - tue
    - wed
    - thu
    - fri
    - sat
    - sun
- condition: state
  entity_id: input_boolean.flux_discharge
  state: "on"
- condition: state
  entity_id: select.solax_charger_use_mode
  state: "Manual Mode"
action:
- service: select.select_option
  data:
    option: "Self Use Mode"
  target:
    entity_id: select.solax_charger_use_mode
mode: single
```

### Flux - Discharge
```
alias: Flux - Discharge
description: ->
Sets inverter to manual mode and enables force discharge at 16:01 when flux_discharge is set to on in dashboard.  
A separate automation "Flux - Discharge Cutout" monitors the force discharge and sets the inverter to "self use" 
when batter SoC is below a defined value
trigger:
- platform: time
  at: "16:01:00"
condition:
- condition: state
  entity_id: input_boolean.flux_discharge
  state: "on"
- condition: state
  entity_id: select.solax_charger_use_mode
  state: "Self Use Mode"
- condition: numeric_state
  entity_id: sensor.solax_battery_capacity
  above: input_number.flux_discharge_cutout_soc
  action:
- parallel:
    - service: select.select_option
      data:
        option: "Manual Mode"
      target:
        entity_id: select.solax_charger_use_mode
    - service: select.select_option
      data:
        option: "Force Discharge"
      target:
        entity_id: select.solax_manual_mode_select
mode: single
```

### Solar - Battery Charge Automation
```
alias: Solar - Battery Charge Automation
description: Main automatic battery charging function
trigger:
- platform: time
  at: "23:55:00"
condition: []
action:
- service: input_number.set_value
  data:
    value: "{{ states('sensor.house_load_today') }}"
  target:
    entity_id: input_number.expected_consumption
- delay:
    hours: 0
    minutes: 0
    seconds: 10
    milliseconds: 0
- service: select.select_option
  data:
    option: "{{ states('sensor.soc_charge_end_time_hhmm') }}"
  target:
    entity_id: select.solax_charger_end_time_1
mode: single
```

### Solar - Expected Consumption Low State Tracker
```
alias: Solar - Expected Consumption Low State Tracker
description: >-
If the number input number 'Expected Consumption' is overtaken by the Solis
sensor 'Solax House Load Today', the value from the sensor is set to the
number input to avoid peculiarities in battery SoC forecasting.
trigger:
- platform: state
  entity_id:
    - sensor.house_load_today
condition:
- condition: numeric_state
  entity_id: sensor.house_load_today
  above: input_number.expected_consumption
action:
- service: input_number.set_value
  data:
    value: "{{ states('sensor.house_load_today') }}"
  target:
    entity_id: input_number.expected_consumption
mode: single
```

### Solar - Expected Consumption Today : Updater
```
alias: "Solar - Expected Consumption Today : Updater"
description: >-
Updates todays expected consumption every hour after midday by dividing house
load today by the hour and then multiplying by 24
trigger:
- platform: time_pattern
  minutes: "1"
condition:
- condition: time
  weekday:
    - mon
    - tue
    - wed
    - thu
    - fri
    - sat
    - sun
  after: "12:00:00"
  before: "23:02:00"
action:
- service: input_number.set_value
  data:
    value: "{{ states('sensor.house_load_today_forecast') }}"
  target:
    entity_id: input_number.expected_consumption
mode: single
```
### Solar - Restore Consumption Defaults
```
alias: Solar - Restore Consumption Defaults
description: >-
  Sets todays expected consumption and tomorrows expected consumption to 10,
  Target SoC to 4.5, base load to 0.23, and boost charge to 0
trigger:
  - platform: state
    entity_id:
      - input_button.reset_consumption_defaults
condition: []
action:
  - service: input_number.set_value
    data:
      value: 10
    target:
      entity_id: input_number.expected_consumption
  - service: input_number.set_value
    data:
      value: 10
    target:
      entity_id: input_number.expected_consumption_tomorrow
  - service: input_number.set_value
    data:
      value: 4.5
    target:
      entity_id: input_number.target_usable_soc
  - service: input_number.set_value
    data:
      value: 0.23
    target:
      entity_id:
        - input_number.base_load
  - service: input_number.set_value
    data:
      value: 0
    target:
      entity_id: input_number.boost_charge
  - service: number.set_value
    data:
      value: "55"
    target:
      entity_id: number.solax_timed_charge_current
mode: single
```

### Solcast - API Poll Schedule
```
alias: Solcast - API Poll Schedule
description: New API call Solcast
trigger:
  - platform: time
    at: "06:00:00"
  - platform: time
    at: "10:00:00"
  - platform: time
    at: "14:00:00"
  - platform: time
    at: "18:00:00"
  - platform: time
    at: "23:50:00"
condition: []
action:
  - service: solcast_solar.update_forecasts
    data: {}
mode: single
```
