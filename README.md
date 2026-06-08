# Simple---security---password-v2
new added component lcd 16 x 2 I2C

```cpp
// LIBRARY
#include <Adafruit_LiquidCrystal.h>
#include <Keypad.h>

// INTIALIZATION

// LCD
Adafruit_LiquidCrystal lcd(0);

// ULTRASONIC SENSOR
int trig = 10;
int echo = 11;
long duration;
float distance;

// SWITCH BUTTON
int button = 12;

// PASSWORD
String password = "1234";

//currentpassword
String currentpassword = "";

// KEYPAD
const byte ROW = 4;
const byte COL = 4;

char keys [ROW][COL] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};

byte rowpin [ROW] = {2,3,4,5};
byte colpin [COL] = {6,7,8,9};

Keypad keypad = Keypad( makeKeymap(keys), rowpin, colpin, ROW, COL);

void setup() {
  lcd.begin(16,2);
  lcd.print("PASSWORD:");
  pinMode(LED_BUILTIN, OUTPUT);
  pinMode(trig, OUTPUT);
  pinMode(echo, INPUT);
  pinMode(button, INPUT_PULLUP);
}

void loop() {
  digitalWrite(trig, LOW);
  delayMicroseconds(2);
  digitalWrite(trig, HIGH);
  delayMicroseconds(10);
  digitalWrite(trig, LOW);

  duration = pulseIn(echo, HIGH);

  distance = duration * 0.0343 / 2 ;

  if (distance <= 150){
    char click = keypad.getKey();
    
    if (click){
      currentpassword += click;
      lcd.setCursor(0,1);
      lcd.print(currentpassword);
    }
      
    if (digitalRead(button) == LOW){
      if (currentpassword == password){
        lcd.setCursor(0,1);
        lcd.print("CORRECT");
        digitalWrite(LED_BUILTIN, HIGH);
        delay(500);
        digitalWrite(LED_BUILTIN, LOW);
        delay(500);  
      }else{
        lcd.setCursor(0,1);
        lcd.print("INCORRECT");       
      }
      lcd.setCursor(0,1);
      lcd.print("                ");
      currentpassword = "";
      
    }
    
  }
}
