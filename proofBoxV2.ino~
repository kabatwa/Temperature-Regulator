#include <LiquidCrystal.h>

//Setup LCD Display
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

//Pin setup
int sensorPin = 1;
int relayPin=9;

//Temperature control
int maxTemp=28;
int resetTemp=26;

//For a smooooth reading take an average!
const int numReadings=10;
int readings[numReadings];
int index=0;
int total=0;
int average=0;

//Have we reached target temp?
boolean resting=0;

void setup(){
  Serial.begin(9600);  //Start the serial connection with the computer
  lcd.begin(16, 2);

  //to view the result open the serial monitor 
  pinMode(relayPin,OUTPUT);
  for(int thisReading=0; thisReading<numReadings;thisReading++)
    readings[thisReading]=0;
}

void loop(){
  float temperatureC=setTemperature();
  total= total - readings[index];
  readings[index]=temperatureC;
  total=total+readings[index];
  index+=1;

  //Got to beginning of array if we have reached the end
  if(index>=numReadings){
    index=0;
  }

  //Our temperature is the all of the readings in the array averaged.
  average=total/numReadings;

  //Print our temperature to LCD
  lcd.home();
  lcd.print("Temperature=");
  lcd.print(average);
  lcd.print("C");

  //If we've not reached the target temperature yet...
  if(!resting){
    digitalWrite(relayPin,HIGH);

    //If we've reached our target temperature then go into resting mode on next loop
    if(average>=maxTemp){
      resting=1;
    }
    //If we have reached taret temp then go into resting mode...
  }
  else{
    digitalWrite(relayPin,LOW);

    lcd.setCursor(0,1);
    lcd.print("Resting");
    delay(5000);
    if(average<=resetTemp){
      resting=0;
    }
  }
  delay(100);
}

int setTemperature(){
  int reading = analogRead(sensorPin);
  // converting that reading to voltage, for 3.3v arduino use 3.3
  float voltage = reading * 5.0;
  voltage /= 1024.0;
  return(voltage - 0.5) * 100 ;  //converting from 10 mv per degree wit 500 mV offset
  //to degrees ((volatge - 500mV) times 100)
}
