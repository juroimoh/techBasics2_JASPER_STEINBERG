**Disclaimer:** This `.md` contains my personal learning diary for Tech Basics II. I will try making the formatting easy to follow.

# Learning Log

## 13/10/26 Notes (week 1)

Today was the first day and introduction to the Arduino device and program.  
We only made a light flash using the following code:  
```
void setup() {
  // put your setup code here, to run once:
  pinMode(13, OUTPUT);
}

void loop() {
  // put your main code here, to run repeatedly:
  digitalWrite(13, LOW);
  delay(300);
  digitalWrite(13, HIGH);
  delay(300);
}
```
My Arduino box was missing a USB cable (15) so as of writing this I have swapped to a different box (35).  

Current ideas for the final project include joystick controls for my game Dangan, a simple screen LED-based movement game, or collaborating with Hendrik to make physical controls for his Tech Basics I game, which has more controls than Dangan.  

## 20/10/26 Notes (week 2)

Today we further explored lights using the breadboard and power supply.  

This code flashes three lights and then keeps them on:  
```
const int redLED = 2;
const int yellowLED = 3;
const int greenLED = 4;

void setup() {
  pinMode(redLED, OUTPUT);
  pinMode(yellowLED, OUTPUT);
  pinMode(greenLED, OUTPUT);
}

void loop() {
  digitalWrite(redLED, HIGH);
  digitalWrite(yellowLED, HIGH);
  digitalWrite(greenLED, HIGH);
  delay(500);
  digitalWrite(redLED, HIGH);
  digitalWrite(yellowLED, LOW);
  digitalWrite(greenLED, LOW);
  delay(50);
  digitalWrite(redLED, LOW);
  digitalWrite(yellowLED, HIGH);
  digitalWrite(greenLED, LOW);
  delay(50);
  digitalWrite(redLED, LOW);
  digitalWrite(yellowLED, LOW);
  digitalWrite(greenLED, HIGH);
  delay(50);
}
```
Resistor Color Codes:  
<img width="500" src="https://github.com/user-attachments/assets/c3d24317-307f-41e0-b0bd-95a99e57e4c3" />  

`digitalWrite` is used for LED lights.  
`analogWrite` is used for RGB lights.  

Using the RGB light, this code flashes new colors:  
```
const int redLED = 9;
const int greenLED = 10;
const int blueLED = 11;

void setup() {
  pinMode(redLED, OUTPUT);
  pinMode(greenLED, OUTPUT);
  pinMode(blueLED, OUTPUT);
}

void loop() {
  setRGB(255,255,0); // yellow
  delay(300);
  setRGB(0,255,255); // cyan
  delay(300);
  setRGB(255,0,255); // purple
  delay(300);
}

void setRGB(int r, int g, int b){
  analogWrite(redLED, r);
  analogWrite(greenLED, g);
  analogWrite(blueLED, b);
}
```

## 27/10/26 Notes (week 3)

`analogWrite()` uses PWN pins, allowing for analog signals.  
- 0 - 255 (0v to 5v)  

Analog signals are **continuous** and vary **smoothly** over time (PWN pin like ~3, not pin 13).  
Digital signals are **discrete** and are usually represented with binary (0 or 1).  

Demo of the oscilations:  
```
void setup() {
  pinMode(3, OUTPUT);
}

void loop() {
  analogWrite(3, 255); // always on (5v)
  analogWrite(3, 127); // half on, half off
  analogWrite(3, 50); // sometimes on, mostly off
  analogWrite(3, 0); // always off (0v)
}
```

Blinking LED:  
```
const int LED_PIN = 3; // ~ PWM pin (between power values 0-255)
void setup() {
  pinMode(LED_PIN, OUTPUT);
}

void loop() {
  analogWrite(LED_PIN, 200);
  delay(300);
  analogWrite(LED_PIN, 5);
  delay(300);
}
```

Fade in/out an LED:  
```
const int LED_PIN = 3;

void setup() {
  pinMode(LED_PIN, OUTPUT);
}

void loop() {
  for (int i = 0; i <= 255; i += 5) { // (START; END; STEP)
    analogWrite(LED_PIN, i);
    delay(15);
  }

  for (int i = 255; i >= 0; i -= 5) {
    analogWrite(LED_PIN, i);
    delay(15);
  }
}
```

Fading an LED using a potentiometer:  
```
const int POT = A0;
const int LED_PIN = 3;
int reading = 0;
int brightness = 0;

void setup() {
  pinMode(POT, INPUT);
  Serial.begin(9600);
}

void loop() {
  reading = analogRead(POT);
  Serial.print("Potentiometer: ");
  brightness = map(reading, 0, 1023, 0, 255);
  Serial.println(reading);
  analogWrite(LED_PIN, brightness);
}
```
<img width="300" src="https://github.com/user-attachments/assets/1346b10a-33dc-4174-b896-acbf7126ef4a" />  

Detecting light using a photoresistor using the same code as above (we detect using the same pins):  
<img width="300" src="https://github.com/user-attachments/assets/a943fa89-b5cd-414e-a7e7-057a0ba600c3" />  

## 03/11/26 Notes (week 4)

Introduction to sound!

**Active Buzzer:** Produces sound directly when connectefd to power (built in oscillator, defined pitch).  
**Passive Buzzer:** Requires a signal in order to make a sound.  

Basic sound on a passive buzzer:
```
const int buzzerPin = 8;

void setup() {
  pinMode(buzzerPin, OUTPUT);
  tone(buzzerPin, 200, 1000); // tone(pin, frequency, duration)
}

void loop() {
}
```

Input pitch on a passive buzzer:
```
const int buzzerPin = 8;
int input = 0;

void setup() {
  Serial.begin(9600);
  pinMode(buzzerPin, OUTPUT);
  delay(1000);
  Serial.println("Enter the pitch you want to play:");
}

void loop() {
  if (Serial.available() > 0) {
    input = Serial.parseInt();
    Serial.println(input);
    tone(buzzerPin, input, 500);
  }
}
```

Repo for ultrasonic sensor: https://github.com/cqx931/techBasics2_2026/blob/main/week4/3_ultrasonic/3_ultrasonic.ino  

Detecting a close object = play sound:
```
const int trigPin = 9;
const int echoPin = 10;
const int soundPin = 2;

long duration;
int distance;

void setup() {
  Serial.begin(9600);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(soundPin, OUTPUT);
}

void loop() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);

  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);

  Serial.print("Duration ");
  Serial.print(duration);
  Serial.print(" microseconds | ");

  distance = duration * 0.034 / 2; // Speed of sound wave divided by 2 (go and back)

  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  if (distance < 30) { // THIS PART PLAYS A SOUND IF THE DISTANCE IS LESS THAN 30
    digitalWrite(soundPin, HIGH);
  } else { (distance > 30); {
    digitalWrite(soundPin, LOW);
  }} // Not sure if this is correct, but this gets rid of any error messages.
  delay(500);
}
```
<img width="300" src="https://github.com/user-attachments/assets/6a141c4a-24a9-4259-8159-a2a2f2843607" />  

Detecting a button press:  
```
const int buttonPin = 2;
int state = 0;

void setup() {
  pinMode(buttonPin, INPUT);
  Serial.begin(9600);
}

void loop() {
  state = digitalRead(buttonPin);
  Serial.print("Button:");
  Serial.println(state);
}
```
<img width="500" src="https://github.com/user-attachments/assets/a567cee2-4651-4560-a8e4-3a036ff44d69" />  

To swap the 1 and 0 output, swap the breadboard positions of green and yellow (input and output nodes?).  

A mini instrument, with 3 buttons and 3 pitches:
```
const int buttonPin1 = 2;
const int buttonPin2 = 4;
const int buttonPin3 = 12;

const int buzzerPin = 8;

int state1 = 0;
int state2 = 0;
int state3 = 0;

void setup() {
  pinMode(buttonPin1, INPUT_PULLUP);
  pinMode(buttonPin2, INPUT_PULLUP);
  pinMode(buttonPin3, INPUT_PULLUP);
  
  Serial.begin(9600);
}

void loop() {
  state1 = digitalRead(buttonPin1);
  state2 = digitalRead(buttonPin2);
  state3 = digitalRead(buttonPin3);

  Serial.print("Button: ");
  Serial.print(state1);
  Serial.print(" ");
  Serial.print(state2);
  Serial.print(" ");
  Serial.println(state3);
  
  if (state1 == 1 && state2 == 1 && state3 == 1){ // IF NO BUTTON IS PRESSED, STOP PLAYING
    noTone(buzzerPin);
  }

  if (state1 == 0) {
    tone(buzzerPin, 200);
  }

  if (state2 == 0) {
    tone(buzzerPin, 300);
  }

  if (state3 == 0) {
    tone(buzzerPin, 400);
  }
}
```
<img width="500" src="https://github.com/user-attachments/assets/a887ccdd-e999-4269-9653-a523d55e1f26" />

This week was very fun, learning to control the output of the program with an input, and learning to play music. I still struggle with adapting the code to fit the task without help, and setting up my own circuit without heavily relying on my own pictured examples and the picture from the slides.  

## 10/11/26 Notes (week 5)

Today we begin learning about movement.  

Servo Motor:  
<img height="300" src="https://github.com/user-attachments/assets/2c255f8b-b6a7-4aa1-b79d-10a10282dcb8" />

Basic servo motor control:
```
`#include <Servo.h>
Servo myservo;

void setup() {
  myservo.attach(9);
}

void loop () {
  myservo.write(0);
  delay(2000);
  myservo.write(180);
  delay(2000);
}
```

Beginning the void loop code with `myservo.write(0);` ensures the motor always begins at the correct position.  

Changing speeds:  
```
#include <Servo.h>
Servo myservo;

void setup() {
  myservo.attach(9);
}

void loop () {
  for (int i=0; i <= 180; i += 1) {
      myservo.write(i);
      delay(10);
  }
  myservo.write(0);
}
```

Control Servo using a potentiometer:
```
#include <Servo.h>
Servo myservo;
const int POT = A0;
int position = 0;
int reading = 0;


void setup() {
  pinMode(POT, INPUT);
  myservo.attach(9);
  Serial.begin(9600);
}

void loop() {
  reading = analogRead(POT);
  Serial.print("Potentiometer: ");
  Serial.println(reading);
  position = map(reading, 0, 1023, 0, 180);
  myservo.write(position);
}
```
<img width="300" src="https://github.com/user-attachments/assets/bb2e3a1b-a982-4b83-bea3-f1eb24c45123" />

Read a Joystick output:
```
const int X = A0;
const int Y = A1;
const int pin2 = 2;

int state = 0;
int readingX = 0;
int readingY = 0;

void setup() {
  pinMode(X, INPUT);
  pinMode(Y, INPUT);
  pinMode(pin2, INPUT_PULLUP);
  Serial.begin(9600);
}

void loop() {
  state = digitalRead(pin2);
  readingX = analogRead(X);
  readingY = analogRead(Y);
  Serial.print("Reading: ");
  Serial.print(readingX);
  Serial.print(" | ");
  Serial.print(readingY);
  Serial.print(", State: ");
  Serial.println(state);
}
```
<img height="200" src="https://github.com/user-attachments/assets/6339c8f5-ace7-4a76-8eec-befa95e0c6e2" /> <img height="200" src="https://github.com/user-attachments/assets/c02259eb-3679-4d70-a4e2-9559ae9508b5" />  
^ Setup and expected output (state `1` = joystick button not pressed)  

‼️ A **double-servo** controller using the Joystick ‼️
```
#include <Servo.h>
Servo myservo;
Servo myservo2;

const int X = A0;
const int Y = A1;
const int pin2 = 2;

int state = 0;
int readingX = 0;
int readingY = 0;
int positionX = 0;
int positionY = 0;

void setup() {
  pinMode(X, INPUT);
  pinMode(Y, INPUT);
  myservo.attach(9);
  myservo2.attach(11);
  Serial.begin(9600);
}

void loop() {
  state = digitalRead(pin2);
  readingX = analogRead(X);
  readingY = analogRead(Y);
  Serial.print("Reading: ");
  Serial.print(readingX);
  Serial.print(" | ");
  Serial.println(readingY);

  positionX = map(readingX, 0, 1023, 0, 180);
  positionY = map(readingY, 0, 1023, 0, 180);
  myservo.write(positionX);
  myservo2.write(positionY);
}
```
<img width="500" src="https://github.com/user-attachments/assets/3550d14a-edf6-4e24-9319-f083478f05b9" />  <img height="300" src="https://github.com/user-attachments/assets/9a09fbc1-5477-4805-a365-c4333ff386fe" />






## XX/11/26 Notes (week X)

scientists reveal the hi
