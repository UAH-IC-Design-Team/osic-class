# demo04 - Inverter
[Git repo for today's demo](https://github.com/UAH-IC-Design-Team/demo04)

# Schematic and Simulation

```
* ngspice commands
.options list act
.options method=gear
.temp 25

.control
save all
tran 0.1 40u 

write inverter_test.raw
.endc

```
# Layout

# LVS


