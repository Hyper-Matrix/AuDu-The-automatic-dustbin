#define REMOTEXY_MODE__SOFTWARESERIAL  
#include <SoftwareSerial.h>             //Including the software serial library
#include <RemoteXY.h>                   //Including the remotexy library
#include <Servo.h>                      //servo library
/* RemoteXY connection settings */ 
#define REMOTEXY_SERIAL_RX 2            //defining the pin 2 as RX pin
#define REMOTEXY_SERIAL_TX 3            //defining the pin 3 as TX pin
#define REMOTEXY_SERIAL_SPEED 9600      //setting baudrate at 9600

unsigned char RemoteXY_CONF[] ={ 3,0,23,0,1,5,5,15,41,11,43,43,1,2,0,6,5,27,11,5,79,78,0,79,70,70,0 };  
    
struct {                            //Function for declaring the variables
  signed char joystick_1_x;         //joystick x-axis 
  signed char joystick_1_y;         //joystick y-axis
  unsigned char switch_1;           //variables for switch
  unsigned char connect_flag; 

} RemoteXY;  

//defining the pins for first motor
#define IN1 10 
#define IN2 9 
#define ENA 12

Servo servo;     
int trigPin = 6;    
int echoPin = 5;   
int servoPin = 4;

long duration, dist, average;   
long aver[3];

//defining the pins for second motor
#define IN3 8 
#define IN4 7 
#define ENB 11 

//defining the LED pin  
#define ledpin 13 

unsigned char first_motor[3] =  
  {IN1, IN2, ENA}; 
unsigned char second_motor[3] =  
  {IN3, IN4, ENB}; 

void Speed (unsigned char * pointer, int motor_speed) 
{ 
  if (motor_speed>100) motor_speed=100; 
  if (motor_speed<-100) motor_speed=-100; 
  if (motor_speed>0) { 
    digitalWrite(pointer[0], HIGH); 
    digitalWrite(pointer[1], LOW); 
    analogWrite(pointer[2], motor_speed*2.55); 
  } 
  else if (motor_speed<0) { 
    digitalWrite(pointer[0], LOW); 
    digitalWrite(pointer[1], HIGH); 
    analogWrite(pointer[2], (-motor_speed)*2.55); 
  } 
  else { 
    digitalWrite(pointer[0], LOW); 
    digitalWrite(pointer[1], LOW); 
    analogWrite(pointer[2], 0); 
  } 
} 

void setup() 
{ 
  //defining the motor pins as the output pins
  servo.attach(servoPin);  
  pinMode(trigPin, OUTPUT);  
  pinMode(echoPin, INPUT);  
  servo.write(0);         //close dustbin cap on power on
  delay(1000);
  servo.detach();
  pinMode (IN1, OUTPUT); 
  pinMode (IN2, OUTPUT); 
  pinMode (IN3, OUTPUT); 
  pinMode (IN4, OUTPUT); 
  pinMode (ledpin, OUTPUT); 
  RemoteXY_Init (); 
} 

void loop() 
{ 
  RemoteXY_Handler (); 
  digitalWrite (ledpin, (RemoteXY.switch_1==0)?LOW:HIGH);  
  Speed (first_motor, RemoteXY.joystick_1_y - RemoteXY.joystick_1_x); 
  Speed (second_motor, RemoteXY.joystick_1_y + RemoteXY.joystick_1_x); 

   for (int i=0;i<=2;i++) {   //average distance
    measure();               
   aver[i]=dist;            
    delay(50);              //delay between measurements
  }
  dist=(aver[0]+aver[1]+aver[2])/3;    //average distance by 3 measurements

  if ( dist<40 ) {
   //if hand on the distance 10...50 cm
   servo.attach(servoPin);
    delay(1);
   servo.write(90);  
   delay(5000);       //wait 5 seconds
   servo.write(0);    
   delay(1000);
   servo.detach();      
  }
} 
void measure() {  
digitalWrite(trigPin, LOW);
delayMicroseconds(5);
digitalWrite(trigPin, HIGH);
delayMicroseconds(15);
digitalWrite(trigPin, LOW);
pinMode(echoPin, INPUT);
duration = pulseIn(echoPin, HIGH);
dist = (duration/2) / 29.1;    //obtain distance
}
