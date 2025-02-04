# IoT-Automated-water-plant
#include<LiquidCrystal.h>
const int rs=13,en=12,d4=11,d5=10,d6=9,d7=8;
LiquidCrystal lcd(rs,en,d4,d5,d6,d7);

int pump=3;
int fan=4;

#include<dht.h>  
#define dht_dpin 2 
dht DHT;


void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600);
  lcd.begin(16,2);
  pinMode(pump,OUTPUT);
  pinMode(fan,OUTPUT);
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("AUTOMATED WATER");
  lcd.setCursor(0,1);
  lcd.print("PLANT MONITORING");
  Serial.println("Autmoated Water Plant Monitoring System");
  delay(1000);
  

}

void loop() 
{
  // put your main code here, to run repeatedly:
  MOISTURE();
  DHT_CHECK();

}

void MOISTURE()
{
  int moistval=analogRead(A0);
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("MOIST:");
  lcd.setCursor(0,1);
  lcd.print(moistval);
  Serial.print("MOIST:");
  Serial.println(moistval);
  delay(1000);
  if(moistval>700)
  {
    Serial.println("$Moisture Low, Pump on#");
    lcd.clear();
    lcd.setCursor(0,0);
    lcd.print("MOISTURE LOW");
    lcd.setCursor(0,1);
    lcd.print("PUMP ON");
    digitalWrite(pump,HIGH);
//   digitalWrite(buzzer,HIGH);
    delay(1000);
//    digitalWrite(buzzer,LOW);
    delay(500);
    
  }
  else
  {
    digitalWrite(pump,LOW);
    delay(1000);
  }
}

void DHT_CHECK()
{
    DHT.read11(dht_dpin);
    Serial.print("Humidity:");
    Serial.print(DHT.humidity);   // Printing temperature on LCD
    Serial.print("%");
    Serial.println("  ");
    Serial.print("Temperature:");
    Serial.print(DHT.temperature);   // Printing temperature on LCD
    Serial.print("C");
    Serial.println("  ");
    lcd.clear();
    lcd.setCursor(0,0);
    lcd.print("HUMIDITY:");
    lcd.setCursor(9,0);
    lcd.print(DHT.humidity);
    lcd.setCursor(14,0);
    lcd.print("%");
    lcd.setCursor(0,1);
    lcd.print("TEMP:");
    lcd.setCursor(5,1);
    lcd.print(DHT.temperature);
    lcd.setCursor(10,1);
    lcd.print("C");
    delay(1500);
    if(DHT.temperature>32)
    {
      Serial.println("$More Temperature#");
      lcd.clear();
      lcd.setCursor(0,0);
      lcd.print("MORE TEMPERATURE");
      lcd.setCursor(0,1);
      lcd.print("  FAN ON  ");
      digitalWrite(fan,HIGH);
      delay(1000);
}
else
{
  digitalWrite(fan,LOW);
  delay(1000);
}
}
