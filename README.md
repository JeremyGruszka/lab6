lab6
====

##Prelab

###Things I may need to know

####For running the motor:

![alt text](https://raw.githubusercontent.com/JeremyGruszka/lab6/master/DrivingMotors.PNG "Running Motor")



#####MOTOR CONTROL SUGGESTIONS:
 
There are many ways to control your robot motors using the resources on the MSP430. I'm leaving this purposesly open-ended to see the creative approaches you come up with. Here are a few ideas to get your started:

 •Output four independent PWM signals - two for each motor ◦Use RESET mode on the signals you want to ground
 ◦Use RESET/SET mode or TOGGLE mode on the signals you want to be PWM 
 NOTE: CCR0 cannot use RESET/SET mode because it dictates the Timer_A period

•Output three independent PWM signals - two for forward on each motor, one common for reverse 
◦Forward signals would be independent - reverse would be the same
◦Limitation: you wouldn't be able to turn tank-style
 
•Use internal PWM to trigger interrupts, in which you use GPIO to create motor signals 
◦In this approach, you'd only need two PWM signals
◦You could use global variables to control direction
◦You wouldn't have to worry about multiplexing and pin limitations on our packaging.



#####MOTOR DRIVER CHIP:
 
The robot motors require ~12V and a high amount of current b both of which would immediately burn out your microcontroller if it were directly connected to the motors. The motor driver chip (SN754410) takes a 5V input and produces a ~12V output. Each chip has up to four channels of 5V inputs (1A, 2A, 3A, and 4A) and four corresponding 12V outputs (1Y, 2Y, 3Y, and 4Y).
 
Measure voltage across the 12V rail to determine what is actually being supplied by your battery.
 
The motor driver chip can only supply 1A per circuit! Do not exceed that!
 
You can test your 12V PWM motor driver chip output by connecting it to the oscilloscope. Do not use the logic analyzer for the 12V PWM signals!



#####MOTOR STALL CURRENT:
 
To ensure you never exceed 1A drawn from your motor driver chip, you have to determine the worst-case current draw from your motors. This is called the motor stall current and usually occurs when your robot is pushing against an object it can't move (i.e. a wall) or switching directions quickly.
 
To measure motor stall current, connect your robot to a power supply in series with an ammeter. Allow the wheel to run freely and apply a voltage you expect to use. Then, stop the wheel with your hand and monitor the current. This is your worst-case expected current draw at that voltage. If it exceeds 1A, you can't run your motor at that voltage or risk burning your motor driver chip on motor stall. Reduce the voltage until the stall current is below 1A to see a safe voltage you can drive your motor at.
 
On my robot, the stall current does not go below one amp until my motor is being driven at 8V or less - roughly 60% duty cycle. Exceed this at your own risk!



#####DECOUPLING CAPACITORS:
 
The robot motors have the potential to create voltage fluctuations due to sudden spikes in current draw. They can also induce noise on the 5V line. This can cause your microcontroller to reset. To mitigate these fluctuations and noise, you'll need to use some capacitors:

•One large capacitor (~100uF) across the 12V rail 
◦To supplement current when motor draw spikes
 
•One smaller capacitor (~0.1uF) across the 5V rail 
◦To smooth high frequency noise
 
•One small capactior (10pF) between the RST pin and ground 
◦To smooth noise to the RST pin
◦The RST pin is extremely sensitive to voltage fluctuations
◦If voltage drops, even briefly, it will reset your MCU



#####HOW TO DRIVE YOUR MOTORS:

•You never want to send voltage to both motor terminals simultaneously. This will create a short in your motor driver chip and cause it to burn out.
 
•Avoid sudden direction changes - these cause spikes in current draw by the motors. 
◦I often make both motor terminals low for ~10k clock cycles before switching direction.



#####BREAKING PARTS:
 
You'll probably break parts on this lab - you'll know by the burning smell!
 
This will sometimes result in a short, causing you to burn out one of the fuses on the battery management board that protect the batteries. When the light on the battery goes from green to yellow, that means there's something wrong. Either the battery is dead or you've burned a fuse.
 
To test whether a fuse is burned, use the ohmmeter - a working fuse should have very little resistance. You should turn off the power before you do this! You can also use a voltmeter - there shouldn't be much voltage drop across a working fuse. If you have burned a fuse, don't just replace it and try again - try to figure out what caused the fuse to burn in the first place! If you burned your motor driver chip, replace that - but also figure out why you burned it! If you don't fix the root cause, you'll just break more parts and cost yourself more time / frustration.



#####Schematic

![alt text](https://raw.githubusercontent.com/JeremyGruszka/lab6/master/schematic.jpg "Schematic")

##Lab 6 Work
