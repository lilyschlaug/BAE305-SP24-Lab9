# Lab 9 – Take Control: The PID Feedback Controller 

# By: Lily Schlaug and Noah Lane
# Summary
This lab implemented a feedback controller to the robot that was built in a previous lab. The goal of this lab was to create a system that would detect objects at a specified distance and stop the robot from moving forward into the object. The robot continuously checked for an object, so that if the obstacle was removed the robot would begin to move forward again. This was achieved through detection with the ultrasonic sensor. A proportional-integral-derivative (PID) controller was implemented and required tuning to act as the feedback controller of the system. The outcome of this lab was that the robot automatically moved backwards or forwards, depending on the feedback from the controller about how far away the object was detected. 

# Materials
1. Sparkfun Inventors Kit 
- RedBoard
- Ultrasonic sensor
- Two Motors
- Motor Driver 
- Battery pack
2. Computer running Arduino IDE  
3. Smart phone running an Android OS with the MIT AI2 Companion app installed
4. Cable/adaptor to connect the smart phone to the RedBoard.
5. HC-05 Bluetooth UART Module

# Assembly Procedures
1. Build __Circuit 5C: Autonomous Robot__ from the Sparkfun Inventors Guidebook without the switch  
![image](https://github.com/npla225/BAE305-SP24-Lab9/assets/156371043/d6559943-b997-4331-a037-47834da85006)  
**Figure 1: Circuit 5C schematic**

# Test Equipment
1. Ruler
2. Wooden Plank  

# Test Procedures
Part 1 - PID Use 
1. Open Arduino IDE and install the PID_V2 Library.
2. Open the previous robot sketch, and make the following modifications: 
- include PID library
- create variables setpoint, measurement, output, Kp, Ki, Kd as double. 
- initialize the PID within the setup function
- within the loop, run the PID after obtaining the distance 
- write the setpoint, measurement and output values to the serial monitor to help verify operation 

Part 2 - Keep your Distance
1. write a function to instruct the robot to move forward or backward depending on the measured distance. 
2. tune the system by modifiying Kp, Ki, and Kd. Use the ruler to measure the actual distance with the reported distance and adjust the values. 

# Test Results
Part 1 - PID Use 
1. The result of part 1 modified the Arduino sketch to implement the PID controller and detect a distance through the ultrasonic sensor.    

Part 2 - Keep your Distance   
1. The result of part two is to implement the PID controller into the robot movement so that if an object is in front, the robot will move forward or backwards to maintain the specified distance. 

# Conclusion
When using a PID controller in a system such as the ultrasonic robot, the tuning of the KP,KI, and KD errors determine how consistent the output result, in this case staying the set distance away from the wall, is. Before tuning, the robot would oscillate over a much larger distance from the setpoint, +/- 2 inches from the 8 inch setpoint. But after tuning the robot was nearly stationary, oscillating over +/- 2 centimeters. If you were to apply this principle to a large factory setting, not properly tuning the PID controller can result in substantial losses of resources, energy, quality of the product, etc., demonstrating how important it is to adjust the controller to generate as minimal of an error as possible in order to obtain accurate results/outputs.  
# References
**Figure 1**  
JOEL_E_B. “Sparkfun Inventor’s Kit Experiment Guide - v4.0.” SparkFun Inventor’s Kit Experiment Guide - v4.0 - SparkFun Learn, learn.sparkfun.com/tutorials/sparkfun-inventors-kit-experiment-guide---v40/circuit-5c-autonomous-robot. Accessed 5 Mar. 2024.
