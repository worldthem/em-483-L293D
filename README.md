# em-483-L293D
Connect Bipolar stepper motor(4 wires)  em 483 to L293D

First download and install library from here https://github.com/worldthem/28bYJ48

```


#define switchCW A2
#include <28BYJ48.h>


NonBlockingMotor motor(4, 7, 5, 6); 

int degreRotate = 900; 
int degreRotateBack = 330; 
 

long f1(long x){
   return x*motor.steps_per_rev/3600;
}

unsigned long currentTime = 0;
unsigned long newTime = 0; 

void on_reached(long steps){ } // to get number of steps use println

boolean dataBtn ;

void setup() {
  // put your setup code here, to run once:
  // initialise step motor 
  motor.init();
  motor.set_time_between_pulses(2000); // recommended
  motor.set_conversion_function(f1);
  motor.set_on_destination_reached(on_reached);
  pinMode(switchCW,INPUT_PULLUP);// CW push button pin as input
 Serial.begin(9600);
}

void loop() {
 dataBtn = !digitalRead(switchCW);

 
  // put your main code here, to run repeatedly:

  if(millis() % 20000 == 0){
        motor.set_destination(degreRotate);
        motor.status = CW;
  }
       

  if( dataBtn== 1) 
  {
        Serial.println("Button presed");
      // motor.set_destination(degreRotate);
        motor.status = STOP;
   }
  
 motor.update(); // commit rotation 

    if(millis() % 4000 == 0){   // this need because L293D Get realy hot so to prevent bourning the skheme :)  
           waitModeMotor();
      }
}


// motor get hot and for this we will need to make all the pins to LOW to avoid HOT
void waitModeMotor(){
  digitalWrite(4, LOW);
  digitalWrite(5, LOW);
  digitalWrite(6, LOW);
  digitalWrite(7, LOW);
  }
  
  
```
