# demo06 - Ngspice Scripting

## Variables, Vectors and Plots
### Variables (Ngspice manual 17.5.66)
```
set [var]
set [var = value]

* value may be injected into a command using a $
echo $var

```
### Plots
A plot is a collection of vectors. The two commands to create and remove plots are setplot (17.5.69) and destroy (17.5.19). 

When you run a simulation (such as an ac or tran) it automatically creates a plot that the saved vectors are stored in. 

### Vectors
Vectors are complicated. The manual says, "There is no straightforward way to initialize a new vector." Read on let (17.5.39), unlet (17.5.93) and compose (17.5.13) for the three differnt commands for crud operations on vectors. The big thing to note is that all vector commands operate on the vectors of the current plot. You cannot access vectors stored in a plot different from the current plot.

```
let vec = expr

* The value of the vec can be accessed with $&
let vec2 = $&vec / 2

```

## Parameters
Separate from variables and vectors are spice parameters. There are three different types of parameters: device, model and global. 

### Device Parameter
```
alter dev = expr
alter dev param = expr
alter @dev[param] = expr
```

### Model Parameter
```
altermod mod param = expr
altermod @mod[param] = expr

```

### Global Parameter
```
alterparam param = value

alterparam subname param = value

```

## Example
Below is a simple example
```
* ngspice commands
.options method=gear

.control
save all
let run = 0
let dVg = 0.3
let Vg_start = 0
let Vg_cur = $&Vg_start
let Vg_end = 1.8
let total_runs = ($&Vg_end - $&Vg_start) / $&dVg

while $&run lt $&total_runs
	alter Vg Vg_cur
	dc vdd 0 1.8V 10e-3

	plot i(vss)
	let vg_cur = vg_cur + dvg
	let run = run + 1

end

.endc

```

Below is a nice example of some features possible with ngspice
```
* Requires an iterator variable to be passed in!

* ngspice commands
.options list acct opts
.options method=gear
.control
save all
* Go to the const plot
setplot const

let diff_lsb = 1.8 / 1024
let diff_step = $&diff_lsb / 2
let steps_per_iter = 8
let iter_offset = $&steps_per_iter * diff_step

let vdiff = -0.9 + diff_step / 2 + $&iter_offset * $iterator
let vstart = $&vdiff
let vmax = $&vdiff + $&iter_offset 
let vdelta = $&diff_step;

let total_runs = ceil(($&vmax - $&vdiff) / $&vdelta)
let runs = 0
let all_runs = $iterator * $&total_runs
let runs_start = $&runs

let out_bits = vector($&total_runs*10)
reshape out_bits[10][$&total_runs]
let in_diff_v= vector($&total_runs) 
let vsampled_p = vector($&total_runs) 
let vsampled_n = vector($&total_runs) 

echo
echo AWS iterator = $iterator
echo vstart = $&vstart
echo vmax = $&vmax
echo vdiff = $&vdiff
echo diff_lsb = $&diff_lsb
echo diff_step = $&diff_step
echo steps_per_iter = $&steps_per_iter
echo iter_offset = $&iter_offset
echo total_runs = $&total_runs
echo runs = $&runs
echo

* Insert vector names and set only one scale
set wr_vecnames
set wr_singlescale

* set the hcopy type
set hcopydevtype=svg


while $&runs lt $&total_runs
	echo
	echo run = $&runs
	echo Vdiff = $&vdiff
	echo
	* Alter the voltages
	alter \\\\@Vp[pulse] = [ 0 $&vdiff 1n 2.5n 1n 1 1 ] $ vector
	alter \\\\@Vn[pulse] = [ 0 $&vdiff 1n 2.5n 1n 1 1 ] $ vector

	* Run the tran
	* tran creates a new plot starting with tran1
	tran 0.5n 700n uic

	set pltfile1 = plot_converg_\{$&all_runs\}_\{$&vdiff\}.svg
	set pltfile2 = plot_input_v_\{$&all_runs\}_\{$&vdiff\}.svg
	set pltfile3 = plot_clks_\{$&all_runs\}_\{$&vdiff\}.svg
	set pltfile4 = plot_raw_bits_\{$&all_runs\}_\{$&vdiff\}.svg
	set plttitle = run\{$&all_runs\}_vin\{$&vdiff\}
	hardcopy $pltfile1 x1.vsampled_p x1.vsampled_n x1.vsampled_p-x1.vsampled_n x1.sw_sample-2 x1.comp_out_p+2 x1.comp_out_n-2 title $plttitle
	*hardcopy $pltfile2 vin_p vin_n vss vdd vbias title $plttitle
	hardcopy $pltfile3 x1.x1.cycle0 x1.x1.cycle1 x1.x1.cycle2 x1.x1.cycle3 x1.x1.cycle4 x1.x1.cycle5 x1.x1.cycle6 x1.x1.cycle7 x1.x1.cycle8 x1.x1.cycle9 x1.x1.cycle10 x1.x1.cycle11 x1.x1.cycle12 x1.x1.cycle13 x1.x1.cycle14 x1.x1.cycle15 x1.controller_clk+2 title $plttitle
	hardcopy $pltfile4 x1.comp_out_p-2 x1.comp_out_n-4 x1.x1.raw_bit1 x1.x1.raw_bit2+2 x1.x1.raw_bit3+4 x1.x1.raw_bit4+6 x1.x1.raw_bit5+8 x1.x1.raw_bit6+10 x1.x1.raw_bit7+12 x1.x1.raw_bit8+14 x1.x1.raw_bit9+16 x1.x1.raw_bit10+18 x1.x1.raw_bit11+20 x1.x1.raw_bit12+22 x1.x1.raw_bit13+24 title $plttitle



	* Measure the max to find the output
	meas tran max_bit0 MAX v(bits1) from=650n to=655n
	meas tran max_bit1 MAX v(bits2) from=650n to=655n
	meas tran max_bit2 MAX v(bits3) from=650n to=655n
	meas tran max_bit3 MAX v(bits4) from=650n to=655n
	meas tran max_bit4 MAX v(bits5) from=650n to=655n
	meas tran max_bit5 MAX v(bits6) from=650n to=655n
	meas tran max_bit6 MAX v(bits7) from=650n to=655n
	meas tran max_bit7 MAX v(bits8) from=650n to=655n
	meas tran max_bit8 MAX v(bits9) from=650n to=655n
	meas tran max_bit9 MAX v(bits10) from=650n to=655n
	meas tran avg_vsampled_p AVG v(x1.vsampled_p) from=605n to=610n
	meas tran avg_vsampled_n AVG v(x1.vsampled_n) from=605n to=610n


	* Create variables
	set max_bit0 = $&max_bit0
	set max_bit1 = $&max_bit1
	set max_bit2 = $&max_bit2
	set max_bit3 = $&max_bit3
	set max_bit4 = $&max_bit4
	set max_bit5 = $&max_bit5
	set max_bit6 = $&max_bit6
	set max_bit7 = $&max_bit7
	set max_bit8 = $&max_bit8
	set max_bit9 = $&max_bit9
	set vdiff = $&vdiff
	set vsampled_p = $&avg_vsampled_p
	set vsampled_n = $&avg_vsampled_n

	* Switch to constants plot
	setplot const
	*compose out_bits values $&out_bits $p_max $n_max
	let in_diff_v[$&runs] = $vdiff
	let vsampled_p[$&runs] = $vsampled_p
	let vsampled_n[$&runs] = $vsampled_n
	let out_bits[0][$&runs] = $max_bit0
	let out_bits[1][$&runs] = $max_bit1
	let out_bits[2][$&runs] = $max_bit2
	let out_bits[3][$&runs] = $max_bit3
	let out_bits[4][$&runs] = $max_bit4
	let out_bits[5][$&runs] = $max_bit5
	let out_bits[6][$&runs] = $max_bit6
	let out_bits[7][$&runs] = $max_bit7
	let out_bits[8][$&runs] = $max_bit8
	let out_bits[9][$&runs] = $max_bit9

	* set the iterators
	echo run $&runs
	let vdiff = vdiff + vdelta
	let runs = runs + 1
	let all_runs = all_runs + 1

	* Destroy the transient plot to release memory
	destroy tran1

end

* switch to the const plot
setplot const
compose def_scale start=1 stop=$&total_runs step=1
setscale def_scale
echo Writing out_bits.txt
wrdata out_bits.txt vsampled_p vsampled_n in_diff_v out_bits

echo
echo Total Runs = $&runs
echo Run from = $&runs_start to = $&runs
echo Vdiff from = $&vstart to $&vdiff - $&vdelta
echo
.endc
```
