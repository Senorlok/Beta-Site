---
title: HC-05 BT and Motor
---
/*
Using the HC-05 BT module, an Arduino Nano (see schematic in Arduino folder), and an L298N DC motor driver board.
Also using the 'Bluetooth RC Controller' app.
*/

#define IN1 4                 //Defining the pins.
#define IN2 5
int state = 0;
void setup() 
{
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  Serial.begin(9600);         //Default communication rate of the Bluetooth module.
}
void loop() 
{
 if(Serial.available() > 0)
 { //Checks whether data is comming from the serial port.
    state = Serial.read();    //Reads the data from the serial port.
 }
 if (state == 'H') 
 {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  Serial.println("LEFT");     //Send back to the phone the turn direction.
  delay (45);                 //Making sure the motor is moving on that direction for the increment of 45ms.
  state = 0;                  //Resetting the state back to zero.
 }
 else if (state == 'J') 
 {
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  Serial.println("RIGHT");;
  delay (45);
  state = 0;
 } 
 else
 {
  digitalWrite(IN1, LOW);     //Turn off the motors when the default condition (which is state = 0) is met.
  digitalWrite(IN2, LOW);
 }
}