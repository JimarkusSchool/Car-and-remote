# Car-and-remote
How to make a RC drone
A tutorial on how to create a remote controlled car using arduinos.
This guide outlines how to setup the controller and motorised components of an RC vehicle, this guide can be used as part of an outline for how to create a DIY remote control car. Now if you haven’t checked out the tutorial on how to create wireless communications I reckon you should check out Finn McClusky’s tutorial on bluetooth connections to extend a project like this. 
Components:
2 arduinos (one for the controller and one for receiver

H-bridge motor shield and Joystick shield

Male to Female Jump wires and Male to Male Jumper wires  (The more the merrier)

1 Bread Board

2 motors(with wheels)

Chassis base

H-bridge (using a shield is easier but we will cover both in this tutorial)

1 swivel caster

2 AA battery packs

Step 1- Chassis building:
We need to connect  both the swivel caster and the motors/wheels to the car, lets start with the motors. Place the motor against the chassis, aligning it with each of the screw holes, Using a screw driver, tightly turn each of the screws until each of the motors are firmly attached to the board, The final result should resemble something like this.
![jimmy](https://user-images.githubusercontent.com/57181085/67947772-80283e80-fc38-11e9-93a3-b2faafac91d6.jpg)
Follow the same process with the swivel wheel to get a result resembling this.
![pls](https://user-images.githubusercontent.com/57181085/67947692-51aa6380-fc38-11e9-84e4-ad47e73bc857.jpg)
Step 2- Circuitry
To connect the controller to the board we will need 3 cables of at least 2 metres, do this by connecting the male and female cords together, the length of the cable determines the length of how far the drones range will be so adjust to whatever you require
On top of the chassis, place 2 battery packs and an arduino
	Controller
	Connect the joystick shield to the arduino
Car
Connect the H-bridge shield to the arduino
![Screenshot_20191031-234933_Gallery](https://user-images.githubusercontent.com/57181085/67948396-b7e3b600-fc39-11e9-9532-a44a1323c8df.jpg)
	Connect each of the wires according to this outline:

Left (image above) motor positive(RED):
H-board screw connection 4

Left motor negative:
H-board screw connection 3

Right motor positive:
H-board screw connection 2

Right motor negative:
H-board screw connection 1

Tightly screw in each of the wires with a small flathead screwdriver
Connect each battery pack to each of the arduinos for power
Connection
To connect the controller to the board we will need 3 cables, each of at least 2 metres, do this by connecting the male and female cords together, the length of the cable determines the length of how far the drones range will be so adjust to whatever you require, use tape to bundle the 3 cables together and to hold the individual cables between each other if necessary. The cables need to connect to each of these wires

Connect Ground one board too ground on another,
Connect 1 on the Controller board to 0 on the Car board
Connect 0 on the Controller board to 1 on the Car board

Step 3- Code
We will run two different pages of code, one for each arduino, the controller segment will use this code.
	
Controller code
	
	#include <stdio.h>
	#include <stdlib.h>
	void setup()
	{
	  Serial.begin(9600);
	}
	void loop()
	{
	  char str[10];
	  itoa(map(analogRead(A0), 0, 1023, -255, 254), str, 10);
	  itoa(map(analogRead(A1), 0, 1023, -255, 254), &str[5], 10);
	  Serial.write(str, 10);
	}



Motor control code

	const int channel_a_enable  = 6;
	const int channel_a_input_1 = 4;
	const int channel_a_input_2 = 7;
	const int channel_b_enable  = 5;
	const int channel_b_input_3 = 3;
	const int channel_b_input_4 = 2;

	void setup()
	{
	  Serial.begin(9600);

	  pinMode( channel_a_enable, OUTPUT );  // Channel A enable
	  pinMode( channel_a_input_1, OUTPUT ); // Channel A input 1
	  pinMode( channel_a_input_2, OUTPUT ); // Channel A input 2

	  pinMode( channel_b_enable, OUTPUT );  // Channel B enable
	  pinMode( channel_b_input_3, OUTPUT ); // Channel B input 3
	  pinMode( channel_b_input_4, OUTPUT ); // Channel B input 4

	}
	int forward=0;
	int side=0;
	void loop()
	{
	  if(Serial.available()>=10)
	  {
	    char something[10];
	    Serial.readBytes(something, 10);
	    forward=atoi(something);
	    side=atoi(&something[5]);
	    if(forward<0)
	    {
	      float power=-forward*1/float(255);
	      if(side<0)
	      {
		float left=(side+255)*1/float(255);
		analogWrite(channel_a_enable, 255*power);
		digitalWrite( channel_a_input_1, LOW);
		digitalWrite( channel_a_input_2, HIGH);

        analogWrite(channel_b_enable, 255*power*left);
        digitalWrite( channel_b_input_3, HIGH);
        digitalWrite( channel_b_input_4, LOW);
      }
      else if(side>=0)
      {
        float left=-(side-255)*1/float(255);
        analogWrite(channel_a_enable, 255*power*left);
        digitalWrite( channel_a_input_1, LOW);
        digitalWrite( channel_a_input_2, HIGH);

        analogWrite(channel_b_enable, 255*power);
        digitalWrite( channel_b_input_3, HIGH);
        digitalWrite( channel_b_input_4, LOW);
      }
    }
    else if(forward>=0)
    {
      float power=forward*1/float(255);
      if(side<0)
      {
        float left=(side+255)*1/float(255);
        analogWrite(channel_a_enable, 255*power);
        digitalWrite( channel_a_input_1, HIGH);
        digitalWrite( channel_a_input_2, LOW);

        analogWrite(channel_b_enable, 255*power*left);
        digitalWrite( channel_b_input_3, LOW);
        digitalWrite( channel_b_input_4, HIGH);        
      }
      else if(side>=0)
      {
        float left=-(side-255)*1/float(255);
        analogWrite(channel_a_enable, 255*power*left);
        digitalWrite( channel_a_input_1, HIGH);
        digitalWrite( channel_a_input_2, LOW);

        analogWrite(channel_b_enable, 255*power);
        digitalWrite( channel_b_input_3, LOW);
        digitalWrite( channel_b_input_4, HIGH);        
       }
      }
     }
    }
I hope you found this tutorial helpful. Thank you for reading.
