include <SoftwareSerial.h>

//#ifndef debug
//#define debug
//#endif 

#ifndef motorpinconfig
#define motorpinconfig
#define motor1a 8
#define motor1b 9
#define motor2a 10
#define motor2b 11
#endif

#ifndef motionpinconfig
#define motionpinconfig
#define motion 7
#endif

#ifndef alarmpinconfig
#define alarmpinconfig
#define alarm 6
#endif 

#ifndef btpinconfig
#define btpinconfig
#define bttransmitter NULL
#define btreceiver    5
#define btbaudrate    9600
#endif 

SoftwareSerial bt(btreceiver, bttransmitter);
int8_t btdata;

void setup()
{
  pinMode(motion, INPUT);
  digitalWrite(motion, HIGH);

  pinMode(alarm, OUTPUT);
  digitalWrite(alarm, LOW);

  pinMode(motor1a, OUTPUT);
  pinMode(motor1b, OUTPUT);
  pinMode(motor2a, OUTPUT);
  pinMode(motor2b, OUTPUT);

  digitalWrite(motor1a, LOW);
  digitalWrite(motor1b, LOW);
  digitalWrite(motor2a, LOW);
  digitalWrite(motor2b, LOW);

  bt.begin(btbaudrate);
  bt.flush();

  #ifdef debug
  Serial.begin(9600);
  Serial.println("Alive human detection robot...\r\n");
  #endif 
}

void loop()
{
  if(bt.available()) 
  {
    btdata = bt.read();

    #ifdef debug
    Serial.print("Received char: ");
    Serial.write(btdata);
    Serial.println("\r\n");
    #endif 
  }

  if(digitalRead(motion))
  {
    btdata = 'H';
    digitalWrite(alarm, HIGH);
  }
  else digitalWrite(alarm, LOW);

  switch(btdata)
  {
    case '0':

    #ifdef debug
    Serial.println("stop\r\n");
    #endif 
    
    digitalWrite(motor1a, LOW);
    digitalWrite(motor1b, LOW);
    digitalWrite(motor2a, LOW);
    digitalWrite(motor2b, LOW);
    btdata = false; break;

    case 'F':

    #ifdef debug
    Serial.println("Forwarad\r\n");
    #endif 
    
    digitalWrite(motor1a, HIGH);
    digitalWrite(motor1b, LOW);
    digitalWrite(motor2a, HIGH);
    digitalWrite(motor2b, LOW);
    btdata = false; break;

    case 'B':

    #ifdef debug
    Serial.println("Backward\r\n");
    #endif
    
    digitalWrite(motor1a, LOW);
    digitalWrite(motor1b, HIGH);
    digitalWrite(motor2a, LOW);
    digitalWrite(motor2b, HIGH);
    btdata = false; break;

    case 'R':

    #ifdef debug
    Serial.println("Right\r\n");
    #endif 
    
    digitalWrite(motor1a, LOW);
    digitalWrite(motor1b, HIGH);
    digitalWrite(motor2a, HIGH);
    digitalWrite(motor2b, LOW);
    btdata = false; break;

    case 'L':

    #ifdef debug
    Serial.println("Left\r\n");
    #endif 
    
    digitalWrite(motor1a, HIGH);
    digitalWrite(motor1b, LOW);
    digitalWrite(motor2a, LOW);
    digitalWrite(motor2b, HIGH);
    btdata = false; break;

    case 'H':

    #ifdef debug
    Serial.println("Motion detected\r\n");
    #endif 
    
    digitalWrite(motor1a, LOW);
    digitalWrite(motor1b, LOW);
    digitalWrite(motor2a, LOW);
    digitalWrite(motor2b, LOW);
    digitalWrite(alarm, HIGH);
    btdata = false; break;

    default: break;
  }
}
