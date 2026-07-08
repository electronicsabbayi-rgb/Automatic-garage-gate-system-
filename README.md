# Automatic-garage-gate-system-



#include <Servo.h>

Servo myServo;

const int trigPin = 7;
const int echoPin = 6;
const int servoPin = 9;

long duration;
int distance;

bool isOpen = false;   // To prevent shaking

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  myServo.attach(servoPin);
  myServo.write(0);
  Serial.begin(9600);
}

void loop() {

  // Trigger ultrasonic
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);

  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);
  distance = duration * 0.034 / 2;

  Serial.print("Distance: ");
  Serial.println(distance);

  // If object detected and servo is closed
  if (distance > 0 && distance < 15 && !isOpen) {

    for (int pos = 0; pos <= 90; pos++) {
      myServo.write(pos);
      delay(8);
    }

    isOpen = true;
  }

  // If object removed and servo is open
  if ((distance >= 20 || distance == 0) && isOpen) {

    for (int pos = 90; pos >= 0; pos--) {
      myServo.write(pos);
      delay(8);
    }

    isOpen = false;
  }

  delay(100);
}