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
```
/*
  SparkFun Inventor’s Kit
  Circuit 5B - Remote Control Robot

  Control a two wheeled robot by sending direction commands through the serial monitor.
  This sketch was adapted from one of the activities in the SparkFun Guide to Arduino.
  Check out the rest of the book at
  https://www.sparkfun.com/products/14326

  This sketch was written by SparkFun Electronics, with lots of help from the Arduino community.
  This code is completely free for any use.

  View circuit diagram and instructions at: https://learn.sparkfun.com/tutorials/sparkfun-inventors-kit-experiment-guide---v40
  Download drawings and code at: https://github.com/sparkfun/SIK-Guide-Code

  Modified by:
  Carlos Jarro for University of Kentucky's BAE305 Lab 6
  02/28/2024
*/

#include <PID_v2.h>

//the right motor will be controlled by the motor A pins on the motor driver
const int AIN1 = 13;           //control pin 1 on the motor driver for the right motor
const int AIN2 = 12;            //control pin 2 on the motor driver for the right motor
const int PWMA = 11;            //speed control pin on the motor driver for the right motor

//the left motor will be controlled by the motor B pins on the motor driver
const int PWMB = 10;           //speed control pin on the motor driver for the left motor
const int BIN2 = 9;           //control pin 2 on the motor driver for the left motor
const int BIN1 = 8;           //control pin 1 on the motor driver for the left motor

const int trigPin = 6;        //trigger pin for distance snesor
const int echoPin = 7;        //echo pin for distance sensor
const int RED = 3;
const int GREEN = 2;

String botDirection;           //the direction that the robot will drive in (this change which direction the two motors spin in)
String motorSpeedStr;

int motorSpeed;               //speed integer for the motors
float duration, distance;     //duration and distance for the distance sensor

double setpoint = 15; 
double measurement = 0; 
double output = 0; 
double Kp = 8; 
double Ki = 1; 
double Kd = 1; 
int speed = 0; 

PID myPID(&measurement, &output, &setpoint, Kp, Ki, Kd, DIRECT);

/********************************************************************************/
void setup()
{
  myPID.SetTunings(Kp, Ki, Kd);
  myPID.SetMode(AUTOMATIC);

  //set the motor control pins as outputs
  pinMode(AIN1, OUTPUT);
  pinMode(AIN2, OUTPUT);
  pinMode(PWMA, OUTPUT);

  pinMode(BIN1, OUTPUT);
  pinMode(BIN2, OUTPUT);
  pinMode(PWMB, OUTPUT);
  
  //set the distance sensor trigger pin as output and the echo pin as input
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);

  pinMode(RED,OUTPUT);
  pinMode(GREEN,OUTPUT);

  Serial.begin(9600);           //begin serial communication with the computer

  //prompt the user to enter a command
  Serial.println("Enter a direction followed by speed.");
  Serial.println("f = forward, b = backward, r = turn right, l = turn left, s = stop");
  Serial.println("Example command: f 50 or s 0");
}

/********************************************************************************/
void loop()
{
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);
  distance = (duration*340/10000)/2; // Units are cm
  measurement = distance; 
  myPID.Compute();

  speed = (map(output, 0, 255, 150, 255));
  Serial.print(distance);
  Serial.print(" ,");
  Serial.print(setpoint);
  Serial.print(" ,");
  Serial.println(speed);
  
//  Serial.println(distance);
    delayMicroseconds(50);

  //if (Serial.available() > 0)                         //if the user has sent a command to the RedBoard
  //{
  //  botDirection = Serial.readStringUntil(' ');       //read the characters in the command until you reach the first space
  //  motorSpeedStr = Serial.readStringUntil(' ');           //read the characters in the command until you reach the second space
    //motorSpeed = motorSpeedStr.toInt();
  //}
  if (true)
  {                                                     //if the switch is in the ON position
  motorSpeed = speed; 
    if (measurement > setpoint)
    {
      botDirection = "b"; 
    }
      else {
      botDirection = "f";
    }

    if (botDirection == "f")                           //if the entered direction is forward
    {
      rightMotor(-motorSpeed);                                //drive the right wheel forward
      leftMotor(motorSpeed);                                 //drive the left wheel forward
      digitalWrite(GREEN,HIGH);
      digitalWrite(RED,LOW);
    }
    else if (botDirection == "b")                    //if the entered direction is backward
    {
      rightMotor(motorSpeed);                               //drive the right wheel forward
      leftMotor(-motorSpeed);                                //drive the left wheel forward
      digitalWrite(GREEN,HIGH);
      digitalWrite(RED,LOW);
    }
    else if (botDirection == "r")                     //if the entered direction is right
    {
      rightMotor(motorSpeed);                               //drive the right wheel forward
      leftMotor(motorSpeed);                                 //drive the left wheel forward
      digitalWrite(GREEN,HIGH);
      digitalWrite(RED,LOW);
    }
    else if (botDirection == "l")                   //if the entered direction is left
    {
      rightMotor(-motorSpeed);                                //drive the right wheel forward
      leftMotor(-motorSpeed);                                //drive the left wheel forward
      digitalWrite(GREEN,HIGH);
      digitalWrite(RED,LOW);
    }
    else if (botDirection == "s")
    {
      rightMotor(0);
      leftMotor(0);
      digitalWrite(GREEN,LOW);
      digitalWrite(RED,HIGH);
    }
  }
  else if (distance < 20)
  {
    if (botDirection == "b")
    {
      rightMotor(motorSpeed);                               //drive the right wheel forward
      leftMotor(-motorSpeed);                                //drive the left wheel forward
    }
    else
    {
      //Serial.print("Object Detected at ");
      //Serial.print(distance);
      //Serial.println(" cm");
      rightMotor(0);                                  //turn the right motor off
      leftMotor(0);                                   //turn the left motor off
    }
  }
}
/********************************************************************************/
void rightMotor(int motorSpeed)                       //function for driving the right motor
{
  if (motorSpeed > 0)                                 //if the motor should drive forward (positive speed)
  {
    digitalWrite(AIN1, HIGH);                         //set pin 1 to high
    digitalWrite(AIN2, LOW);                          //set pin 2 to low
  }
  else if (motorSpeed < 0)                            //if the motor should drive backward (negative speed)
  {
    digitalWrite(AIN1, LOW);                          //set pin 1 to low
    digitalWrite(AIN2, HIGH);                         //set pin 2 to high
  }
  else                                                //if the motor should stop
  {
    digitalWrite(AIN1, LOW);                          //set pin 1 to low
    digitalWrite(AIN2, LOW);                          //set pin 2 to low
  }
  analogWrite(PWMA, abs(motorSpeed));                 //now that the motor direction is set, drive it at the entered speed
}

/********************************************************************************/
void leftMotor(int motorSpeed)                        //function for driving the left motor
{
  if (motorSpeed > 0)                                 //if the motor should drive forward (positive speed)
  {
    digitalWrite(BIN1, HIGH);                         //set pin 1 to high
    digitalWrite(BIN2, LOW);                          //set pin 2 to low
  }
  else if (motorSpeed < 0)                            //if the motor should drive backward (negative speed)
  {
    digitalWrite(BIN1, LOW);                          //set pin 1 to low
    digitalWrite(BIN2, HIGH);                         //set pin 2 to high
  }
  else                                                //if the motor should stop
  {
    digitalWrite(BIN1, LOW);                          //set pin 1 to low
    digitalWrite(BIN2, LOW);                          //set pin 2 to low
  }
  analogWrite(PWMB, abs(motorSpeed));                 //now that the motor direction is set, drive it at the entered speed
}
```

Part 2 - Keep your Distance


# Test Results
Part 1 - PID Use 
1. The result of part 1 modified the Arduino sketch to implement the PID controller and detect a distance through the ultrasonic sensor.    
Part 2 - Keep your Distance   
1. The result of 

# Discussion
Did you make any design decisions that had an impact on the results? How did they impact the results? What do the results mean?
# Conclusion

# References
**Figure 1**  
JOEL_E_B. “Sparkfun Inventor’s Kit Experiment Guide - v4.0.” SparkFun Inventor’s Kit Experiment Guide - v4.0 - SparkFun Learn, learn.sparkfun.com/tutorials/sparkfun-inventors-kit-experiment-guide---v40/circuit-5c-autonomous-robot. Accessed 5 Mar. 2024.
