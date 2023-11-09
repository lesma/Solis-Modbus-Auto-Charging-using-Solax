# Installation

1. Click Settings > Devices & Services > Helpers > + "Create Helper"

2. Use each of the items below to create helpers (number, toggle or button).  Name them exactly the same as specified in each title.  Your max/min and step sizes will be dependant on your own needs, but my numbers here should serve as a suitable starting point for most (if not all) installations.


### Base Load
* input_number.base_load
* Number
* Minimum Value: 0
* Maximum Value: 1
* Step Size: 0.01
* Unit of Measurement: kW

### Battery Capacity
* input_number.battery_capacity
* Number
* Minimum Value: 1.0
* Maximum Value: 25.0
* Step Size: 0.01
* Unit of Measurement: kWh
* Mode: Box

### Boost Charge
* input_number.boost_charge
* Number
* Minimum Value: 0
* Maximum Value: 15
* Step Size: 1
* Unit of Measurement: kWh

### Expected Consumption
* input_number.expected_consumption
* Number
* Minimum Value: 0
* Maximum Value: 50
* Step Size: 0.5
* Unit of Measurement: kWh

### Expected Consumption Tomorrow
* input_number.expected_consumption_tomorrow
* Number
* Minimum Value: 0
* Maximum Value: 50
* Step Size: 0.5
* Unit of Measurement: kWh

### Flux Discharge
* input_boolean.flux_discharge
* Toggle

### Force Charge SoC
* input_number.force_charge_soc
* Number
* Minimum Value: 0
* Maximum Value: 50
* Step Size: 1
* Unit of Measurement: %

### Offpeak Window
* input_number.offpeak_window
* Number
* Minimum Value: 1
* Maximum Value: 7
* Step Size: 1
* Unit of Measurement: Hours

### Overdischarge SoC
* input_number.overdischarge_soc
* Number
* Minimum Value: 0
* Maximum Value: 50
* Step Size: 1
* Unit of Measurement: %

### Reset Consumption Defaults
* input_button.reset_consumption_defaults
* Button

### Target Usable SoC
* input_number.target_usable_soc
* Number
* Minimum Value: 0
* Maximum Value: 20
* Step Size: 1
* Unit of Measurement: kWh

### Flux Charge SoC
* input_number.flux_charge_soc
* Number
* Minimum Value: 0
* Maximum Value: 100
* Step Size: 1
* Unit of Measurement: %

### Flux Discharge Cutout SoC
* input_number.flux_discharge_cutout_soc
* Number
* Minimum Value: 10
* Maximum Value: 100
* Step Size: 1
* Unit of Measurement: %

### Hybrid Battery Charge Power
* input_number.hybrid_battery_charge_power
* Number
* Minimum Value: 1
* Maximum Value: 9000
* Step Size: 100
* Unit of Measurement: W