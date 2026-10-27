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
  for (int i=0; i <= 255; i += 5) { // (START; END; STEP)
    analogWrite(LED_PIN, i);
    delay(15);
  }

  for (int i=255; i >= 0; i -= 5) {
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
<img width="300" src="https://github.com/user-attachments/assets/a2d7e70f-a9ec-4109-b542-6ef89a582688" />



## XX/10/26 Notes (week X)

scientists reveal the hi
