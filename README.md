lab6
====

##Prelab

###Things I may need to know

####For running the motor:

![alt text](https://raw.githubusercontent.com/JeremyGruszka/lab6/master/DrivingMotors.PNG "Running Motor")

MOTOR CONTROL SUGGESTIONS:
 
There are many ways to control your robot motors using the resources on the MSP430. I'm leaving this purposesly open-ended to see the creative approaches you come up with. Here are a few ideas to get your started:
 •Output four independent PWM signals - two for each motor ◦Use RESET mode on the signals you want to ground
 ◦Use RESET/SET mode or TOGGLE mode on the signals you want to be PWM ■NOTE: CCR0 cannot use RESET/SET mode because it dictates the Timer_A period
 

•Output three independent PWM signals - two for forward on each motor, one common for reverse ◦Forward signals would be independent - reverse would be the same
 ◦Limitation: you wouldn't be able to turn tank-style
 
•Use internal PWM to trigger interrupts, in which you use GPIO to create motor signals ◦In this approach, you'd only need two PWM signals
 ◦You could use global variables to control direction
 ◦You wouldn't have to worry about multiplexing and pin limitations on our packaging.

