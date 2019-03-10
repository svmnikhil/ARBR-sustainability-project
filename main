const int UpPin = 2;
const int SetPin =  3;
const int DownPin = 4;
const int GreenPin = 7;
const int RedPin =  6;
const int BuzzPin =  8;
const int DiagRepPin = 5;

//OBD2 Inputs
const int ConsumpPerInterv = 2; //This is liters of gas
const float OBD2fuelEff = 15;

#include <Wire.h>
#include "rgb_lcd.h"

rgb_lcd lcd;

// variables will change:
int fuelEff = 0;
bool toggleSet = false;
bool toggleDiagRep = false;
bool upbuttonState = LOW;
bool downbuttonState = LOW;
float cumCO2 = 0;
int numtrees = 0;
int prevtrees=0;
int ind = 0;

void setup() {
 Serial.begin(9600);
 lcd.begin(16, 2);
 lcd.setRGB(0, 0, 0);

 pinMode(UpPin, INPUT);
 pinMode(SetPin, INPUT);
 pinMode(DownPin, INPUT);
 pinMode(GreenPin, INPUT);
 pinMode(RedPin, INPUT);
 pinMode(BuzzPin, OUTPUT);
 pinMode(DiagRepPin, INPUT);
}

void loop() {

 if (digitalRead(SetPin)==HIGH){
   toggleSet = true;
 }

 if (digitalRead(DiagRepPin) == HIGH) {
   toggleDiagRep = true;
 }

 if (toggleSet == false){
   upbuttonState = digitalRead(UpPin);
   downbuttonState = digitalRead(DownPin);

   if (upbuttonState == HIGH) {
     fuelEff++;
   }
   if (downbuttonState == HIGH) {
     fuelEff--;
   }
   if (fuelEff <0){
     fuelEff = 0;
   }

   lcd.setRGB(fuelEff, fuelEff, fuelEff);
   digitalWrite(RedPin,LOW);
   digitalWrite(GreenPin,HIGH);

 } else if (toggleDiagRep == true){
   Serial.print("The difference between the fuel efficiency you entered and the fuel efficiency calculated by your car is: ");
   Serial.println(OBD2fuelEff - fuelEff);

   if (OBD2fuelEff - fuelEff >= 0){
     Serial.println("Your fuel efficiency is higher than what you expected, give your car more credit!");
   } else {
     Serial.println("Your fuel efficiency is lower than what you expected, maybe it is time to take your car in for a tune up!");
   }

   while(1){}

 } else{
     if (ind%5 ==0){ //In this model, 1 sec ~ 1 min IRL
       prevtrees=numtrees;
       cumCO2 += ConsumpPerInterv * 2.3; //Each litre of gas releases 2.3kg of CO2
       numtrees = floor(cumCO2*0.06); //A tree absorves 0.06kg of CO2 per day
       ind = 0;
       if (prevtrees != numtrees){
         tone(BuzzPin, 500);
         delay(100);
         noTone(BuzzPin);
         if (numtrees >100){
           digitalWrite(RedPin,HIGH);
           digitalWrite(GreenPin,LOW);
         }
       }
     }
     ind++;
 }

 Serial.println();

 Serial.print("Fuel Efficiency: ");
 Serial.println(fuelEff);

 Serial.print("Cumulative CO2: ");
 Serial.println(cumCO2);

 Serial.print("Number of Trees: ");
 Serial.println(numtrees);

 Serial.print("Index: ");
 Serial.println(ind);

 delay(195);


}
