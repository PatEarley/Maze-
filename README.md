# Maze-
Arduino Maze Robot Car
//include the servo library
#include <Servo.h>  

//create a servo object called servo1 
Servo servo1;  
const int trigPin = 11;           //connects to the echo pin on the distance sensor       
const int echoPin = 12;           //connects to the trigger pin on the distance sensor      

void setup()
{
  //attach servo1 to pin 9 on the Arduino 101
  servo1.attach(7);
   Serial.begin (9600);        //set up a serial connection with the computer

  pinMode(trigPin, OUTPUT);   //the trigger pin will output pulses of electricity 
  pinMode(echoPin, INPUT);    //the echo pin will measure the duration of pulses coming back from the distance sensor
}


void loop()
{   float duration, distance;
  duration = pulseIn(echoPin, HIGH);
  distance = (duration / 2) * 0.0344;
  int position;
  //create a local variable to store the servo's position.
distance = getDistance();   //variable to store the distance measured by the sensor

  Serial.print(distance);     //print the distance that was measured
  Serial.println(" in");      //print units after the distance

  if(distance <= 10){                         //if the object is close
servo1.write(90); 
   
  } else if(10 < distance && distance < 20){  //if the object is a medium distance
     servo1.write(180);

  } else{                                     //if the object is far away
 servo1.write(0);   
  }

  delay(50);      //delay 50ms between each reading
}
//RETURNS THE DISTANCE MEASURED BY THE HC-SR04 DISTANCE SENSOR
float getDistance()
{
  float echoTime;                   //variable to store the time it takes for a ping to bounce off an object
  float calcualtedDistance;         //variable to store the distance calculated from the echo time

  //send out an ultrasonic pulse that's 10ms long
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10); 
  digitalWrite(trigPin, LOW);

  echoTime = pulseIn(echoPin, HIGH);      //use the pulsein command to see how long it takes for the
                                          //pulse to bounce back to the sensor

  calcualtedDistance = echoTime / 148.0;  //calculate the distance of the object that reflected the pulse (half the bounce time multiplied by the speed of sound)

  return calcualtedDistance;              //send back the distance that was calculated
}

