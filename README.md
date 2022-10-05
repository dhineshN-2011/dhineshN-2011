#include <Adafruit_MPU6050.h>
#include <Adafruit_Sensor.h>
#include <Wire.h>

Adafruit_MPU6050 mpu;
int pot=A0;
int red = 6; 
int blue = 11;
int flex = A1;
int buz =A2;
int wat_lvl = 3;
int a=0;
int beep = 2;

void setup()                                                          
{
pinMode(red, OUTPUT);
pinMode(blue, OUTPUT);
pinMode(beep, OUTPUT);
pinMode(buz,INPUT);
pinMode(wat_lvl,INPUT);
pinMode(pot,INPUT);
pinMode(flex,INPUT);
Serial.begin(9600);  
 while (!mpu.begin()) {
    Serial.println("MPU6050 not connected!");
    delay(1000);
  }
  Serial.println("MPU6050 ready!");
}                                         
sensors_event_t event;
void loop()
{ 
  int buzz = analogRead(buz);
int water = digitalRead(wat_lvl);
int cap = analogRead(flex);
 mpu.getAccelerometerSensor()->getEvent(&event);
Serial.println(buzz);
Serial.println(cap);
Serial.println(event.acceleration.z);
Serial.println("NONE");
if (event.acceleration.z <= 0 && buzz > 700 )// need to add gyro condition
{
  Serial.println("Water Drinking.........................");
  drinking();
  a=1;
  
}
if (a == 1)
{
  Serial.println("choose timimng>>>>>>>>>>>>>>>>>>>>>>>");
  delay(2000);
   int pote = analogRead(pot);
   int dely = map(pote,0,1023,5000,10000);
digitalWrite(beep,LOW);
digitalWrite(blue, HIGH); 
delay(dely);
check_water();
digitalWrite(red,HIGH);
digitalWrite(blue, LOW);
digitalWrite(beep,HIGH);
a=0;
delay(10000);

}
 
check_water();
delay(1000);
}

int drinking()
{
  int buzz = digitalRead(buz);
  int pote = analogRead(pot); 
  int cap = analogRead(flex);
  mpu.getAccelerometerSensor()->getEvent(&event);
  Serial.println(cap);
  //int dely = map(pote,0,1023,10000,30000);
//Serial.println(dely);
Serial.println(event.acceleration.z);
delay(5000); // drinking time


}

int check_water()
{
  int water = digitalRead(wat_lvl);
  if (water == 0)
{
  Serial.println("WATER AVAILABLE");
  digitalWrite(red,HIGH);
  delay(200);
}
if(water ==1)
{
  Serial.println("WATER NOT AVAILABLE");
  digitalWrite(beep,LOW);
  digitalWrite(blue,HIGH);
  digitalWrite(red,LOW);
  delay(20);
  
}
}
