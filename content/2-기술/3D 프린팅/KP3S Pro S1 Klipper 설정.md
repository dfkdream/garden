설정 후 초기 세팅: [[재설치 후 초기 세팅법]] 참고
# printer.cfg
```
[include mainsail.cfg]
[virtual_sdcard]
path: /home/dfkdream/printer_data/gcodes
on_error_gcode: CANCEL_PRINT

[mcu]
serial: /dev/serial/by-id/usb-1a86_USB_Serial-if00-port0
restart_method: command
baud: 115200

[include macros.cfg]

# This file contains common pin mappings for the Kingroon KP3S printer,
# which uses a modified MKS Robin board.
# To use this config, the firmware should be compiled for the
# STM32F103. When running "make menuconfig", enable "extra low-level
# configuration setup", select the 28KiB bootloader, and serial (on
# USART3 PB11/PB10) communication. Also, select "Enable extra low-level
# configuration options" and configure "GPIO pins to set at
# micro-controller startup" to "!PC6,!PD13" to disable the LCD as it is not
# compatible with klipper

# Note that the "make flash" command does not work with MKS Robin
# boards. After running "make", run the following command:
#   ./scripts/update_mks_robin.py out/klipper.bin out/Robin_nano.bin
# Copy the file out/Robin_nano.bin to an SD card and then restart the
# printer with that SD card.

# See docs/Config_Reference.md for a description of parameters.

[stepper_x]
step_pin: PE3
dir_pin: !PE2
enable_pin: !PE4
full_steps_per_rotation: 200
microsteps: 32
rotation_distance: 40
endstop_pin: !PA15
position_endstop: 0
position_min: 0
position_max: 200
homing_speed: 50
homing_retract_dist: 0
second_homing_speed: 3

[stepper_y]
step_pin: PE0
dir_pin: PB9
enable_pin: !PE1
full_steps_per_rotation: 200
microsteps: 32
rotation_distance: 40
endstop_pin: !PA12
position_endstop: 0
position_min: 0
position_max: 200
homing_speed: 50
homing_retract_dist: 0
second_homing_speed: 3

[stepper_z]
step_pin: PB5
dir_pin: !PB4
enable_pin: !PB8
full_steps_per_rotation: 200
microsteps: 32
rotation_distance: 8
#endstop_pin: !PA11
endstop_pin: probe:z_virtual_endstop
#position_endstop: 0
position_min: -3
position_max: 200
#homing_retract_dist: 10
homing_retract_dist: 1
second_homing_speed: 5

[safe_z_home]
home_xy_position: 100, 100
speed: 50
z_hop: 10
#z_hop: 0
z_hop_speed: 5

[thermistor Kingroon_Calibrated B3950]
temperature1: 25.0
resistance1: 103180.0
temperature2: 150.0
resistance2: 1366.2
temperature3: 250.0
resistance3: 168.6

[extruder]
step_pin: PD6
dir_pin: !PD3
enable_pin: !PB3
full_steps_per_rotation: 200
microsteps: 32
gear_ratio: 3:1
#rotation_distance: 7.940
rotation_distance: 23.820
nozzle_diameter: 0.400
filament_diameter: 1.750
heater_pin: PC3
sensor_type: Kingroon_Calibrated B3950
sensor_pin: PC1
#control: pid
#pid_kp: 16.603
#pid_ki: 0.543
#pid_kd: 127.017
min_temp: 0
max_temp: 300
pressure_advance: 0.085
max_extrude_only_accel: 2000

[heater_bed]
heater_pin: PA0
sensor_type: Kingroon_Calibrated B3950
sensor_pin: PC0
#control: pid
#pid_kp: 68.493
#pid_ki: 1.202
#pid_kd: 976.028
min_temp: 0
max_temp: 130

[bed_screws]
screw1: 25,30
screw2: 175,30
screw3: 175,185
screw4: 25,185

[fan]
pin: PB1
kick_start_time: 0.6
cycle_time: 0.15
shutdown_speed: 0
hardware_pwm: false

[heater_fan nozzle_fan]
pin: PB0
heater: extruder
heater_temp: 50.0
kick_start_time: 0.6
cycle_time: 0.15
fan_speed: 1.0
shutdown_speed: 1

[printer]
kinematics: cartesian
max_velocity: 250
max_accel: 1800
max_accel_to_decel:1800
#max_accel: 1000
#max_accel_to_decel:1000
max_z_velocity: 5
#max_z_accel: 100
max_z_accel: 50
square_corner_velocity: 5.0

[input_shaper]
shaper_freq_x: 35.29411765
shaper_freq_y: 28.57142857
shaper_type: ei

[verify_heater heater_bed]
max_error: 120
check_gain_time: 60
hysteresis: 5
heating_gain: 2

[verify_heater extruder]
max_error: 120
check_gain_time: 30
hysteresis: 5
heating_gain: 2

[static_digital_output display_reset]
pins: !PC6, !PD13

[temperature_sensor MCU]
sensor_type: temperature_mcu

[temperature_sensor host]
sensor_type: temperature_host
sensor_path: /sys/class/thermal/thermal_zone0/temp

[exclude_object]

[gcode_arcs]

[probe]
pin: ^PE6
samples: 3
sample_retract_dist: 0.5
x_offset: 30
y_offset: 1
#z_offset: 2

[bed_mesh]
speed: 150
mesh_min: 30, 30
mesh_max: 170, 170
algorithm: bicubic
probe_count: 6, 6

[force_move]
enable_force_move: True

[respond]

[idle_timeout]
timeout: 1800

#*# <---------------------- SAVE_CONFIG ---------------------->
#*# DO NOT EDIT THIS BLOCK OR BELOW. The contents are auto-generated.
#*#
#*# [extruder]
#*# control = pid
#*# pid_kp = 18.142
#*# pid_ki = 0.806
#*# pid_kd = 102.050
#*#
#*# [heater_bed]
#*# control = pid
#*# pid_kp = 70.754
#*# pid_ki = 1.329
#*# pid_kd = 941.911
#*#
#*# [stepper_z]
#*# position_endstop = -1.003
#*#
#*# [probe]
#*# z_offset = 2.370
#*#
#*# [bed_mesh default]
#*# version = 1
#*# points =
#*# 	  -0.088750, -0.080000, -0.084583, -0.116667, -0.149583, -0.189583
#*# 	  -0.018333, -0.002083, -0.000417, -0.017500, -0.055833, -0.090833
#*# 	  -0.001667, 0.017500, 0.027917, 0.025833, -0.014583, -0.036667
#*# 	  -0.044583, 0.000417, 0.020833, 0.026250, -0.002917, -0.017500
#*# 	  -0.074583, -0.038333, -0.015417, -0.012917, -0.016667, -0.031250
#*# 	  -0.104167, -0.077917, -0.062500, -0.074167, -0.083333, -0.102917
#*# x_count = 6
#*# y_count = 6
#*# mesh_x_pps = 2
#*# mesh_y_pps = 2
#*# algo = bicubic
#*# tension = 0.2
#*# min_x = 30.0
#*# max_x = 170.0
#*# min_y = 30.0
#*# max_y = 170.0
```
# macros.cfg
```
[gcode_macro START_PRINT]
gcode:
  {% set BED_TEMP = params.BED_TEMP|default(63)|float %}
  {% set EXTRUDER_TEMP = params.EXTRUDER_TEMP|default(200)|float %}
  G90
  {% if "xyz" in printer.toolhead.homed_axes %}
    M104 S140
    M140 S{BED_TEMP}
    M190 S{BED_TEMP}
    G92 E0 ; reset extruder
    # move to start position
    G1 X5 Y10 F3000
    G1 Z0.3 F200
    M104 S{EXTRUDER_TEMP}
    M109 S{EXTRUDER_TEMP}
    # line purge
    G1 X90 Y10 Z0.3 F1500.0 E18 ; Draw the first line
    G1 X90 Y12 Z0.3 F3000.0 ; Move to side a little
    G1 X5 Y12 Z0.3 F1500.0 E22 ; Draw the second line
    G92 E0 ; reset extruder
    G1 Z1.0 F3000 ; move z up little to prevent scratching of surface
  {% else %}
    {action_respond_info("Printer not homed")}
    CANCEL_PRINT
  {% endif %}

[gcode_macro END_PRINT]
gcode:
  M104 S0; turn off extruder
  M140 S0 ; turn off bed
  M106 S255 ; turn on fan
  G91; relative positioning
  G1 Z1.0 F3000 ; move z up little to prevent scratching of print
  G90; absolute positioning
  G1 X100 Y100 F1000 ; prepare for part removal
  {% if printer.gcode_move.position.z < 50 %}
    G1 Z50 F200
  {% endif %}
  #M84 ; disable motors
  TEMPERATURE_WAIT SENSOR=extruder MAXIMUM=160 ; wait until extruder is cold enough
  M106 S0; turn off fan

[gcode_macro PID_CALIBRATE_ALL]
gcode:
  {% set BED_TEMP = params.BED_TEMP|default(60)|float %}
  {% set EXTRUDER_TEMP = params.EXTRUDER_TEMP|default(200)|float %}
  PID_CALIBRATE HEATER=extruder TARGET={EXTRUDER_TEMP}
  PID_CALIBRATE HEATER=heater_bed TARGET={BED_TEMP}

[gcode_macro PURGE_EXTRUDER]
gcode:
  {% set EXTRUDER_TEMP = params.EXTRUDER_TEMP|default(200)|float %}
  M106 S0
  M104 S{EXTRUDER_TEMP}
  M109 S{EXTRUDER_TEMP}
  M83
  G1 E50 F300
  M104 S0
  M106 S255
  TEMPERATURE_WAIT SENSOR=extruder MAXIMUM=160 ; wait until extruder is cold enough
  M106 S0

[gcode_macro FORCE_Z_HOP]
gcode:
  FORCE_MOVE stepper=stepper_z distance=50 velocity=5 accel=300

[delayed_gcode startup]
initial_duration: .01
gcode:
  #BED_MESH_PROFILE LOAD=default
  #SET_HEATER_TEMPERATURE heater=heater_bed TARGET=63

[gcode_macro _CLIENT_VARIABLE]
variable_use_custom_pos: True
variable_custom_park_x : 0.0
variable_custom_park_y : 200.0
gcode:
```