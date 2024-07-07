# PrusaSlicerv.2.8.0 start command

```
M109 S0
M190 S0
start_print EXTRUDER_TEMP={first_layer_temperature[initial_extruder]} BED_TEMP={first_layer_bed_temperature[initial_extruder]}
```

# Klipper config

```

# This file contains pin mappings for the stock 2020 Creality Ender 3
# V2. To use this config, during "make menuconfig" select the
# STM32F103 with a "28KiB bootloader" and serial (on USART1 PA10/PA9)
# communication.

# If you prefer a direct serial connection, in "make menuconfig"
# select "Enable extra low-level configuration options" and select
# serial (on USART3 PB11/PB10), which is broken out on the 10 pin IDC
# cable used for the LCD module as follows:
# 3: Tx, 4: Rx, 9: GND, 10: VCC

# Flash this firmware by copying "out/klipper.bin" to a SD card and
# turning on the printer with the card inserted. The firmware
# filename must end in ".bin" and must not match the last filename
# that was flashed.

# See docs/Config_Reference.md for a description of parameters.

[include mainsail.cfg]

[virtual_sdcard]
path: ~/printer_data/gcodes

[display_status]

[pause_resume]

[stepper_x]
step_pin: PC2
dir_pin: PB9
enable_pin: !PC3
microsteps: 16
rotation_distance: 40
endstop_pin: ^PA5
position_endstop: 0
position_max: 235
homing_speed: 50

[stepper_y]
step_pin: PB8
dir_pin: PB7
enable_pin: !PC3
microsteps: 16
rotation_distance: 40
endstop_pin: ^PA6
position_endstop: 0
position_max: 235
homing_speed: 50

[stepper_z]
step_pin: PB6
dir_pin: !PB5
enable_pin: !PC3
microsteps: 16
rotation_distance: 8
endstop_pin: probe:z_virtual_endstop
#position_endstop: 0.0
position_max: 250
position_min: -3

[bltouch]
sensor_pin: ^PB1
control_pin: PB0
x_offset: -40
y_offset: -5.5
z_offset: 3.25

[safe_z_home]
home_xy_position: 117.5,117.5
z_hop: 10
z_hop_speed: 5

[bed_mesh]
speed: 120
horizontal_move_z: 5
mesh_min: 15,15
mesh_max: 188,191
probe_count: 5,5
algorithm: bicubic
fade_start: 1
fade_end: 10
fade_target: 0


[extruder]
max_extrude_only_distance: 100.0
step_pin: PB4
dir_pin: PB3
enable_pin: !PC3
microsteps: 16
rotation_distance: 34.406
nozzle_diameter: 0.400
filament_diameter: 1.750
heater_pin: PA1
sensor_type: EPCOS 100K B57560G104F
sensor_pin: PC5
control: pid
# tuned for stock hardware with 200 degree Celsius target
pid_Kp: 21.527
pid_Ki: 1.063
pid_Kd: 108.982
min_temp: 0
max_temp: 250

[heater_bed]
heater_pin: PA2
sensor_type: EPCOS 100K B57560G104F
sensor_pin: PC4
control: pid
# tuned for stock hardware with 50 degree Celsius target
pid_Kp: 54.027
pid_Ki: 0.770
pid_Kd: 948.182
min_temp: 0
max_temp: 130

[fan]
pin: PA0

[mcu]
serial: /dev/serial/by-id/usb-1a86_USB_Serial-if00-port0
restart_method: command

[printer]
kinematics: cartesian
max_velocity: 300
max_accel: 3000
max_z_velocity: 5
max_z_accel: 100

[gcode_macro START_PRINT]
gcode:
    {% set BED_TEMP = params.BED_TEMP|default(60)|float %}
    {% set EXTRUDER_TEMP = params.EXTRUDER_TEMP|default(190)|float %}
    
    # Wait for bed to reach temperature
    M190 S{BED_TEMP}

    # Use absolute coordinates
    G90
    # Home the printer
    G28
    # Auto Leveling
    # Calibrate bed after bed and nozzle temp reached
    BED_MESH_CALIBRATE
    # Move the nozzle near the bed
    G1 Z5 F3000
    # Move the nozzle very close to the bed
    #G1 Z0.15 F300
    # Set and wait for nozzle to reach temperature
    M109 S{EXTRUDER_TEMP}

```
