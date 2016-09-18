# AntiDecepticon
my sketchbooks
// 4-pin PWM Fan Controller
//
// Author: Sky
//
// Created: 2015-1-1
//
// Updated: 2015-1-3
//
//////////////////////////////////////////////////////////////////

int fanMode = 0;
int switchmode = 0;

// Initial Fan Speeds
int fanSpeed = 0;

// Button Input Pin
int buttonPin = 2;

// Led Ring Pin (PWM)
int ringPin = 11;
// Default Brightness for LED Ring
int ringBrightness = 225;

// PWM Pins
int fan = 3;
int switch_1 = 4;
int switch_2 = 5;
int switch_3 = 6;
int switch_4 = 7;


// Speaker Pin
int speakerPin = A5;

void setup() {
  // led ring output
  pinMode(ringPin, OUTPUT);
  //turn led ring on
  analogWrite(ringPin, ringBrightness);

  //button input
  pinMode(buttonPin, INPUT);

  //pwm pins
  pinMode(fan, OUTPUT);   //980ti Fan
  pinMode(switch_1, OUTPUT);   //switch1
  pinMode(switch_2, OUTPUT);   //switch2
  pinMode(switch_3, OUTPUT);   //switch3
  pinMode(switch_4, OUTPUT);   //switch4


  Serial.begin(9600);
}

void loop() {


  if ( digitalRead(buttonPin) == HIGH ) {

    // Increment the fan mode
    if ( fanMode < 11 ) {
      fanMode++;
    } else {
      fanMode = 0;
    }
  //switching the relays
 { if ( digitalRead(buttonPin) == HIGH ) {

    // relay control
    if ( fanMode > 0  ) {
      switchmode = 1;
    } else {
      switchmode = 0;
    }
    // Set the speed depending on the mode
    switch ( fanMode ) {
      case 0:
        // Fans off Switches in should be in NC positoion
        fanSpeed = 0;
        break;
        
      case 1: // relay switches fan on hig,h led turn on 
        // 100%
        fanSpeed = 255; //I think 255 is full speed 1%=25.5
        break;
      case 2:
        // 90%
        fanSpeed = 230; 
        break;
      case 3:
        // 80%
        fanSpeed = 204; 
        break;
      case 4:
        // 70%
        fanSpeed = 179; 
        break;
      case 5:
        // 60%
        fanSpeed = 153; 
        break;
      case 6:
        // 50%
        fanSpeed = 128; 
        break;
      case 7:
        // 40%
        fanSpeed = 102; 
        break;
      case 8:
        // 30%
        fanSpeed = 77; 
        break;
      case 9:
        // 20%
        fanSpeed = 51; 
        break;  
        case 10:
        // 10%
        fanSpeed = 26; 
        break;
        
        
     // Default
        
        default:
        fanSpeed = 0;
        switchmode = 0;
    }

    // Serial output for debugging
    Serial.print("fan: "); Serial.println(fanSpeed);
   Serial.print("switch: "); Serial.println(switchmode);
    Serial.print("Mode: "); Serial.println(fanMode+1);
    Serial.println();

    // Write the changes to the PWM pins
    analogWrite(fan, fanSpeed);
    analogWrite(switch_1, switchmode);
    analogWrite(switch_2, switchmode);
    analogWrite(switch_3, switchmode);
    analogWrite(switch_4, switchmode);
    
    // Blink led ring for visual output (also beeps)
    for ( int i = 0; i <= fanMode; i++ ) {
      analogWrite(ringPin, 0);
      for ( int ii = 0; ii < 60; ii++ ) {
        digitalWrite(speakerPin, HIGH);
        delayMicroseconds(400);
        digitalWrite(speakerPin, LOW);
        delayMicroseconds(400);
      }
      delay(18);
      analogWrite(ringPin, 255);
      delay(100);
    }

    // turn led ring back to original state so it's not too bright
    analogWrite(ringPin, ringBrightness);

    // Catch until button released for software debounce
    while ( digitalRead(buttonPin) == HIGH ) {}
    delay(20);
  
  }
 }


