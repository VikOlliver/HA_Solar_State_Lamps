# Home Assistant Solar Power State Lamps
Use 3 RGB lights to indicate solar power, battery charge, and system load on Home Assistant.

The intent is to give the family a convenient display of lights that informs them of the current state of the solar power system at a glance. The current implementation uses 3 RGB Athom bulbs dangling from a pendant fitting in the main foyer. When they are all red, the family should not use power-hungry devices.

<img width="431" height="610" alt="image" src="https://github.com/user-attachments/assets/aad38f4e-2907-4c5b-9de1-87e7fe5a7939" />

The lights are driven by three scripts, each one behaving in a different way to provide an intuitive red/yellow/green scale. Here the lights indicate low solar generation on the roof (red), half-full battery (yellow), and low system load (green). The lights make gradual proportionate transitions between hues.

The top bulb is controlled by the rgb_bulb_as_solar.yaml automation script and indicates solar power generation. When power generation is low, the bulb is red. When high, it slowly turns greener through yellow, and if exceptionally high will turn gradually blue. When zero power is generated the bulb turns white. After 10pm the bulb turns a dim amber and acts as a night light.

The middle bulb uses rgb_bulb_as_battery_status.yaml to indicate the battery charge state. When the battery is low it is red, and fades to green when the battery is full. If the battery is below 10% the bulb will glow dim red.

The lower bulb is controlled by rgb_bulb_as_load.yaml which is green with low load, fading to red at high load.

These scripts use the [Sungrow WiNet S/WiNet S2 Extraction Tool](http://homeassistant.local:8123/hassio/addon/b3e7ace5_winet-extractor/info) to extract the status of a solar inverter system, though other sensors can certainly be used. I am no expert in YAML at this point, so apologies for the code. I took heavy inspiration from the "Powerhue" blueprint.

The sensor used is defined in the "variables" section. Unfortunately the variables are sometimes aspirational and hard-coded values are used simply because I have not yet learned enough YAML - "energy_lights" for example, I could not get to work as a variable for some reason, so you'll need to change that manually.

<code>variables:
  power_sensor: sensor.sh15t_a2452418716_total_load_active_power
  power_threshold_low: 0
  power_threshold_high: 7
  default_power_low: 0
  default_power_high: 0.1
  rest_color:
    - 0
    - 0
    - 0
  rest_brightness: 0
  active_brightness: 138
  current_power: "{{ states(power_sensor) | float }}"
  update_interval: 20
  energy_lights: light.athom_rgbct_light_9adff8_rgbcct_bulb</code>

  The power thresholds indicate the expected range of sensor values. The default power represents the point at which the bulb gives up on being a power meter and goes out (or turns into a nightlight) etc.

  To install these scripts I just created a new Automaton, went into "edit YAML" and copy pasted the stuff I'd written.

  Good luck and keep the lights burning.
