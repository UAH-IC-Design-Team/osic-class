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
## Tap and Diff
Sky130 is a tap and diff process. Tap and diff layers are active reagions meant to form heavily doped implants. Tap is doped the same charge as the subtraight it is over and diff is doped the oposite charge as the subtraight it is over. For example, if tap is over P-Subtraight, it is P-Doped or if diff is over an nwell, it is also P-Doped. 

Unfortinately, there is another common name for tap layers: substratediff layers. They are equivialient (look in Magic's layer list), but the naming is extra confusing.  

# LVS




