# Update Arduino board and CNC shield
This is what the CNC shield should look like. Make sure :  
* It is powered (wires on the bottom left of the image, connected to the PSU);
* The emergency button is connected (wires on the bottom right of the image);
* The A4899 stepper drivers are plugged corretly (potentiometer directed toward the bottom of the CNC shield).
* The Arduino Uno is connected to a computer. Not necesserly the RPi, you can connect it to another computer if you feel more confortable to upload Grbl and test the CNC shield.

## Upload the firmware : Grbl {pagestep}
To upload Grbl on the [Arduino board], just follow instructions available on [Grbl's wiki ](https://github.com/gnea/grbl/wiki/Compiling-Grbl).  

Now according to the number of axis you want, you will have a few changes to make on the `config.h` file located in `Arduino>libraries>grbl`. Open it with a text editor, then read from line 90 to 113.
Examples:
* Just X and Y axis ? Comment lines 105 and 106, and uncomment line 110 as follow:  
*105 : // #define HOMING_CYCLE_1 ((1<<X_AXIS)|(1<<Y_AXIS))  // OPTIONAL: Then move X,Y at the same time.*  
*106 : // #define HOMING_CYCLE_2                         // OPTIONAL: Uncomment and add axes mask to enable*  
*110 : #define HOMING_CYCLE_0 ((1<<X_AXIS)|(1<<Y_AXIS))  // NOT COMPATIBLE WITH COREXY: Homes both X-Y in one cycle.*  
* Just X and Z axis in this order ? Modify lines 105 and 106 as follow:  
*105 : #define HOMING_CYCLE_0 (1<<X_AXIS)                // Move X first.* 
*106 : #define HOMING_CYCLE_1 (1<<Z_AXIS) // Then move Z.*  
Then compile and flash Grbl to the Arduino, from the Arduino IDE : `File>Examples>grbl>grblUpload`.  

Once Grbl is compiled and flashed to the Arduino board, it is a good idea to have default settings in a first place. Open the Serial Monitor in the Arduino IDE (Tools > Serial Monitor), then check Grbl settings typing `$$` then Enter, into the Serial Monitor. The settings should look like this (according to [Grbl wiki : Grbl v1.1 Configuration](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Configuration)) :  
<pre>
Settings and sample values 	Description
$0=10 	Step pulse, microseconds
$1=25 	Step idle delay, milliseconds
$2=0 	Step port invert, mask
$3=0 	Direction port invert, mask
$4=0 	Step enable invert, boolean
$5=0 	Limit pins invert, boolean
$6=0 	Probe pin invert, boolean
$10=1 	Status report, mask
$11=0.010 	Junction deviation, mm
$12=0.002 	Arc tolerance, mm
$13=0 	Report inches, boolean
<b style="color:green">$20=0 	Soft limits, boolean</b>
<b style="color:green">$21=0 	Hard limits, boolean</b>
$22=1 	Homing cycle, boolean
<b style="color:green">$23=0 	Homing dir invert, mask</b>
$24=25.000 	Homing feed, mm/min
$25=500.000 	Homing seek, mm/min
$26=250 	Homing debounce, milliseconds
$27=1.000 	Homing pull-off, mm
$30=1000. 	Max spindle speed, RPM
$31=0. 	Min spindle speed, RPM
$32=0 	Laser mode, boolean
<b style="color:green">$100=250.000 	X steps/mm
$101=250.000 	Y steps/mm
$102=250.000 	Z steps/mm
$110=500.000 	X Max rate, mm/min
$111=500.000 	Y Max rate, mm/min
$112=500.000 	Z Max rate, mm/min
$120=10.000 	X Acceleration, mm/sec^2
$121=10.000 	Y Acceleration, mm/sec^2
$122=10.000 	Z Acceleration, mm/sec^2
$130=200.000 	X Max travel, mm
$131=200.000 	Y Max travel, mm
$132=200.000 	Z Max travel, mm</b>
</pre>
If not, then copy each line, and paste it into the Serial Monitor.  

## Try with the stepper motors {pagestep}
Now it is a good idea to plug all the stepper motors, then check if they run as expected. To do so, run the following commands into the serial monitor: 
* G91; This command set all axis to relative. I find it better to test the CNC shield, so you can run many times the same command.
* G0 X10 Y10 Z10; G0 means 'rapide move' and X10 means 'move 10 mm on X axis'.

### With 4 motors
* You can use the 4rth motor as a clone of another axis. In this case, just add 2 jumpers on X, Y or Z line as on the following picture.
* You can also use the 4rth motor independantly. In this case, add 2 jumpers on the last line after the X, Y and Z lines (as seen below).

### Inverse motor direction
There are 2 possibilities:
* Rotate the connector to 180°;
* OR connect a 5V pin to the DIR pin of interest.


