# ⚡ SmartSine-Stepper ⚡
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Commits since latest](https://img.shields.io/github/commits-since/yasir-shahzad/SmartSine-Stepper/latest)](https://github.com/yasir-shahzad/SmartSine-Stepper/commits/master) 
[![GitHub issues](https://img.shields.io/github/issues/yasir-shahzad/SmartSine-Stepper.svg)](https://github.com/yasir-shahzad/SmartSine-Stepper/issues) 
[![Downloads](https://img.shields.io/github/downloads/yasir-shahzad/SmartSine-Stepper/total.svg?maxAge=3600)](https://github.com/yasir-shahzad/SmartSine-Stepper/releases/latest) 
![Hit Counter](https://visitor-badge.laobi.icu/badge?page_id=yasir-shahzad_SmartSine-Stepper)

If you like **SmartSine-Stepper** - give it a star, or fork it and contribute!  
[![GitHub stars](https://img.shields.io/github/stars/yasir-shahzad/SmartSine-Stepper.svg?style=social&label=Star)](https://github.com/yasir-shahzad/SmartSine-Stepper/stargazers)  
[![GitHub forks](https://img.shields.io/github/forks/yasir-shahzad/SmartSine-Stepper.svg?style=social&label=Fork)](https://github.com/yasir-shahzad/SmartSine-Stepper/network)

Firmware to turn a stepper motor into servo motor: see http://xyz.com for hardware! 

If you have a MKS Servo42D or MKS Servo57D, this firmware is not fully tested. Test submissions are appreciated. __Test at you own RISK!__

If you want to support the work on the firmware and hardware consider buying hardware from www.xyz.com or buying me a beer using the donation button. 


**Support Our Work and Future Projects**:  🚀✨  
If you'd like to contribute and help bring more exciting projects to life, consider sponsoring our work. Every bit of support is greatly appreciated and helps me continue creating valuable tools and solutions.

<p align='left'>
 <a href="https://github.com/sponsors/yasir-shahzad">
    <img src="https://img.shields.io/badge/sponsor-30363D?style=for-the-badge&logo=GitHub-Sponsors&logoColor=#white" />
  </a>&nbsp;&nbsp;
</p>


# How to Install 

# Google groups forum
[Google groups forum]



# Command List

## SmartSine Stepper Command Line Interface
The smart stepper uses a command line interface where the prompt is “:>” 

### help 
The help command will return a list of commands that the smart stepper supports. 

### getcal
This command will print out the 200 point calibration table.  This is useful if you are doing firmware development and do not want to calibrate each time you update firmware.  You can take this table and copy it into the nonvolatile.cpp file as shown below

### calibrate
This will run a 200 point calibration of the encoder. 

### testcal 
This will test the calibration and report the maximum error in degrees. 

### microsteps 
This command gets/sets the number of microsteps the smart stepper will use for the step command and the step pin.  The number of microsteps does not affect the resolution of the controller but rather how fine you can set the position. 

### step
This will move the motor one step clockwise, the step size is based on the current microstep setting.  To move the motor counterclockwise use “step 1”. To move the motor clockwise 16 steps used “step 0 16” to move motor counterclockwise 16 steps use “step 1 16”

### feedback 
This commands disable/enables feedback control loop. 
The plan is to discountinue this command in the future and use the “controlmode” command to put controller in open or one of the many closed loop operational modes. 

### readpos
Reads the current motor position and reports it as degrees.

### encoderdiag
This command will read and report the AS5047D internal registers for diagnostic purposes. 

### spid
This command sets the Kp, Ki, and Kd terms for the simple positional PID controller. 

### ppid
This command sets the Kp, Ki, and Kd terms for the positional PID controller. 

### vpid
This command sets the Kp, Ki, and Kd terms for the velocity PID controller. 

### velocity 
This sets the velocity to rotate motor when unit is configured for velocity PID mode of operation. 

### boot
This command will put the microprocessor in the boot loader mode.  Alternatively this can be done by double pressing the reset button. 

### factoryreset 
This erases the calibration and other system and motor parameters such that unit is reset to the factory ship state.  After this command the unit will need to be calibrated to the motor again. 

### dirpin
This command sets which direction the motor will rotate when direction pin is pulled high. The direction pin is only sampled when the step pin has a rising edge. 
‘dirpin 0’ will set the motor to rotate clockwise when dir pin is high
‘dirpin 1’ will set the motor to rotate counter-clockwise when dir pin is high

### errorlimit
Gets set the maximum number of degrees of error that is acceptable, any posistioning error about the error limit will assert the error pin, when error pin is set as error output. 
For example:
~~~~
:>errorlimit 1.8 
~~~~
Will set the error limit to 1.8 degrees. 

### ctrlmode
Gets/Sets the feedback controller mode of operation. The command takes an integer from 0 through 4 to set the control mode per table below:
* Controller off - 	0  -- this is not currently used
* Open-Loop - 	1  -- this is open loop with no feedback
* Simple PID -   2  -- simple positional PID, which is factory default 
* Positional PID - 3 -- current based PID mode, requires tuning for your machine
* Velocity PID - 4 -- velocity based PID, requires tuning for your machine and speed range

If you are unsure what you are doing leave unit in the Simple PID mode of operation. 

### maxcurrent
This sets the maximum current that will be pushed into the motor. To set the current for maximum of 2.0A you would use command “maxcurrent 2000” as the argument is in milliAmps. 

### holdcurrent
For the Simple Positional PID mode the minimal current (ie current with no positional error) is the hold current. You set this current based on the required holding torque you need for your application. The higher the hold current, the hotter and noisier the motor will be but also the larger the holding torque. 
For the Positional PID mode the PID tuning params have to be set correctly such that the control loop will dynamically determine the holding torque. This tuning of the PID can be difficult, hence the simple PID mode will work most of the time out of the box by setting maximum current and holding current. 

### motorwiring
The firmware always uses a positive angle as a clockwise rotation. A stepper motor however could have wiring done with one coil reversed wired, which will cause motor to normally operate in opposite direction. The Smart Stepper firmware will detect the motor wiring direction, using the encoder, and the firmware will compensate for a reverse wired motor.  The reverse or forward wiring of a motor is detected on first power up after factory reset. If the wiring changes after that you can compensate using this command. 
HOWEVER  it is better to do a factory reset and recalibrate motor if wiring changes. 

### stepsperrotation
The Smart Stepper firmware will with first power on after factory reset detect the number of full steps per rotation for the stepper motor and store in flash memory. This command will read this parameter from flash and allow user to change this parameter if motor is changed.
HOWEVER  it is better to do a factory reset and recalibrate motor if motorchanges. 

### move
The move command will request the motor to move to an absolute angle position.  Optionally the user can specify the rotational speed (RPMs) by which the move should happen. For example if the current motor position is at angle 0 and you issue ‘move 3600” the motor will turn 10 rotations clockwise to angle of 3600 degrees. If issue the ‘move 3600’ again nothing will happen as motor is already at angle 3600. 
If motor is at angle 0 and user issues the command ‘move 3600 20’ then motor will move to 10 rotations clockwise to angle of 3600 at a rate of 20 RPMs. 

### stop
If user issues a move command that takes a long time and wants to stop the move before completion then user can issue the stop command command which will stop a move operation. 

### setzero
This command will take the current motor position and set it to absolute angle of  zero. Note that if you are in the middle move it will take the position at the time of the command and use it, thus it is recommend a move be stopped or wait for completion before issuing the setzero. 

### data
This command will toggle output of binary data.

### looptime
This command will display the time it takes for a single processing loop to execute.

### eepromerror
This command displays the motor error in degrees difference from the stored eeprom value at motor power up.

### eepromloc
Displays the location of the shaft angle in degrees at motor power on.

### eepromwrite
Forces the eeprom to store all current values in ram to eeprom.

### eepromsetloc
Forces the eeprom to write the current shaft angle overwriting the stored location from powerup.

### setpos
Overwrites the current shaft angle in the motion planner.

### reboot
Forces the smart stepper to reboot

### homecurrent
If using built in homing routine (command "home") this will specify the amount of current applied when motor is moving during homing operation when homepin is logic active.__EXPERIMENTAL USE WITH CAUTION__

### homepin
Allows setting of pin for current limited enable for homing. This triggers a current drop during homing movements. Current set using command "homecurrent". This pin is pulled low to activate. __EXPERIMENTAL USE WITH CAUTION__

### homeangledelay
Currently unused.

### home
Tells the motion controller to move motor until the home switch (enable pin) is pulled low. (Only on boards 3/21/2017 or newer) (Must be enabled in firmware).
For example:
~~~~
:>home 360 0.5
~~~~
Will move up to 360 degrees at 0.5 RPM. __EXPERIMENTAL USE WITH CAUTION__

### pinread
Displays the binary states of all pins (Step, Dir, Enable, Error, A3, TX, RX)

### errorpinmode
Sets or displays the error pin mode. Allows someone to swap usage of the error pin as an enable pin on older boards. (Not compiled for use on boards 3/21/2017 or newer since they have separate enable and error pins) (Must be enabled in firmware)
Modes are: 
* "0" - Enable mode, active high (digital input).
* "1" - Enable mode, active low (digital input).
* "2" - Error mode, active low (digital output). Active level is reached when there is an angle error.
* "3" - Error mode, bi-directional, (digital input/output open collector). (Not currently used).

### errorpin
Sets or displays the binary state of the enable pin. Acceptable values are 0 or 1
For example:
~~~~
:>errorpin 1
~~~~
Will set the error pin on the terminal block to output a logic high when the error level is reached

### enablepinmode
Sets or displays the enable pin mode. Allows someone to swap usage of the enable pin as an error pin on older boards. (Only on boards 3/21/2017 or newer since they have separate enable and error pins) (Must be enabled in firmware)
Modes are: 
* "0" - Enable mode, active high (digital input).
* "1" - Enable mode, active low (digital input).
* "2" - Error mode, active low (digital output). Active level is reached when there is an angle error.
* "3" - Error mode, bi-directional, (digital input/output open collector). (Not currently used).

### geterror
Displays the current motor shaft error in degrees.

### getsteps
Displays the number of steps that have been seen from the DIR pin.

### debug
Sets if syslog debugging will be output on USB serial. Allowed values are 0 for disable, 1 for enable.



### description 
20210712 Motor hardware and software update: 57 motor driver chip is changed to the latest research and development of large current SS6952A, the two channels are merged into one channel, driving current is larger, driving capacity is stronger, so that 1 drive 57 motor when the margin is larger, can take a larger motor, is not easy to overheat. The program remains compatible with the previous version, 42 motor circuit remains unchanged, still using SS6951A driver.

 

20210517 Motor hardware and software update: modify and optimize the circuit diagram of SS6951A driver chip. 1. Improve the current sampling accuracy of driver chip, so as to reduce vibration and noise; In deep cooperation with McGwan Company, a low cost encoder chip was independently tested for the driver chip of our company, which further improved the vibration and noise of the motor at low speed!

 

20210230 Motor hardware and software update: in view of the driver chip price skyrocketing and serious shortage of hardware, the driver chip will be changed to the owner company independent self-developed chip, model SS6951A. The internal resistance is only 0.3 euro, half of the original Toshiba 450 chip, the heat is reduced by half, and the maximum driving current reaches 4A; In addition, the internal structure circuit of the chip is optimized for the depth of closed-loop stepping application. The noise and vibration are reduced by about 1/3 compared with before, and the torque and response are improved by about 1/3. Adopt better heat dissipation large package, chip thickness is only 1mm, more conducive to heat sink; If you need a free sample, contact the store owner. In order to alleviate the impact of the shortage of single chip microcomputer, the software has been modified to be compatible with The "N к32" and THE "STM32" models.

 

20190917 update of motor hardware and software: the software has changed only a few parameters to improve the stability, and added Chinese annotations to facilitate learning and understanding. The hardware added anti-reverse diode, model SL54, the original row pin seat changed to a more beautiful and lower height of XH2.54 seat, in addition to the serial port and the burning port into one, convenient to use STLINKV2.1 direct burning and serial port communication.

 
Hardware and software update: the hardware circuit board will be optocoupler input side copper hollow to prevent interference, the sensor is changed to the bottom welding, the circuit board design needs to use nylon washer and motor separated by about 2mm to prevent interference; In terms of software, all floating point number variables are changed to fixed-point variables, and the program is rewritten to adapt to fixed-point variables, the amount of calculation of SCM is greatly reduced, CPU still has about 3/5 free time can be used for the secondary development of other functions. The update frequency of the position ring is increased from 5KHZ to 10KHZ, and the control performance is strengthened. The 42 maximum speed is raised to about 1200RPM, while the 57 maximum speed is raised to about 1000RPM. Part of the timer processing STEP/DIR signal is optimized so that even high speed pulses of several hundred K or even M can be easily handled without losing the pulse. Overloading the fputc () and fgetc () functions in stdio.h header file, the program can now use printf () and scanf () and other standard input and output functions to write RS232 and RS485 protocols, in addition to a simple ASCII protocol as a reference.

 

Latest update: the hardware circuit board has been replaced with a new LDO model RS3005, which completely solves the problem of overheat shutdown of the regulator chip. In addition, the input signal CLK is changed from the original external interrupt pin to the external counting input pin of timer TIM1, and the circuit board is changed to support both STM32F103 and STM32F030 microcontroller. The processing of the original CLK signal on the software is changed from external interrupts to timer counting, so it does not need to consume a lot of processor resources. The original use of interrupts to process the CLK signal will consume a lot of processing time, which will also cause the program to measure the speed of the motor incorrectly. In the case of 32 subdivision, as long as the motor speed runs to 1000RPM, the program may crash, and it is almost impossible to use a high subdivision such as 128/256

The integration of the closed loop stepper motor is the owner of new research and development, not take somebody else's foreign open source project shanzhai copying directly to cheat the little white, buy motor comes with a full range of hardware and software, including source code program, 42 step and 57 stepper schematic and PCB, can buy to return their sheet copy, can also be secondary development, the shop owner will not be updated regularly and fix the BUG of software and hardware, and can provide the code level technical support.

 
The hardware and software performance of the motor has been greatly improved. The hardware cost is less than half of that of MECHADUINO and its counterfeit products in foreign countries. The maximum driving current can reach 3.5a, which is twice as high as that of foreign versions. According to the industrial standard design, high speed isolation optical coupling, software watchdog, reliability optimization. Open loop closed-loop mode with optional switch, subdivision selection, encoder correction all dial switch one key to complete, no need to connect the upper computer secondary programming!

 

 

Closed-loop stepping parameters:
 
Main control chip: hangshun HK32F030C8T6
 
Drive chip: two Toshiba TB67H450 (maximum current 3.5a)
 
Encoder chip: 15 infineon auto industry grade TLE5012B
 
High speed optocoupler: Toshiba dual channel TLP2168
 
Working voltage: 12-30v (recommended 24V)
 
Working current: rated 1.3a (42 steps) 2.5a (57 steps) maximum 3.5a
 
Control accuracy: less than 0.08 degrees





## Contributing 🤝

We encourage contributions to this project! Feel free to submit pull requests with improvements, bug fixes, or additional features.

## Thanks to all contributors ❤️

 <a href="https://github.com/yasir-shahzad/SmartSine-Stepper/graphs/contributors">
   <img src="https://contrib.rocks/image?repo=yasir-shahzad/SmartSine-Stepper" />
 </a>

