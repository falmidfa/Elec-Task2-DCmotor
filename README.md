# Elec-Task2-DCmotor
## Task1-Code


const int motor1Pin1 = 5;
const int motor1Pin2 = 6;
const int motor2Pin1 = 9;
const int motor2Pin2 = 10;

void setup() {
  pinMode(motor1Pin1, OUTPUT);
  pinMode(motor1Pin2, OUTPUT);
  pinMode(motor2Pin1, OUTPUT);
  pinMode(motor2Pin2, OUTPUT);
}

void loop() {
  forward();
  delay(30000);

  backward();
  delay(60000);

  alternateRightLeft(60000);

  stopMotors();
  while(true);{ // توقف نهائي (بديل آمن لـ while(true);)
}
}

// دوال الحركات
void forward() {
  digitalWrite(motor1Pin1, HIGH);
  digitalWrite(motor1Pin2, LOW);
  digitalWrite(motor2Pin1, HIGH);
  digitalWrite(motor2Pin2, LOW);
}

void backward() {
  digitalWrite(motor1Pin1, LOW);
  digitalWrite(motor1Pin2, HIGH);
  digitalWrite(motor2Pin1, LOW);
  digitalWrite(motor2Pin2, HIGH);
}

void turnRight() {
  digitalWrite(motor1Pin1, HIGH);
  digitalWrite(motor1Pin2, LOW);
  digitalWrite(motor2Pin1, LOW);
  digitalWrite(motor2Pin2, HIGH);
}

void turnLeft() {
  digitalWrite(motor1Pin1, LOW);
  digitalWrite(motor1Pin2, HIGH);
  digitalWrite(motor2Pin1, HIGH);
  digitalWrite(motor2Pin2, LOW);
}

void stopMotors() {
  digitalWrite(motor1Pin1, LOW);
  digitalWrite(motor1Pin2, LOW);
  digitalWrite(motor2Pin1, LOW);
  digitalWrite(motor2Pin2, LOW);
}

void alternateRightLeft(unsigned long totalMs) {
  unsigned long elapsed = 0;
  const unsigned long stepMs = 1000;
  while (elapsed < totalMs) {
    turnRight();
    delay(stepMs);
    elapsed += stepMs;
    if (elapsed >= totalMs) break;
    turnLeft();
    delay(stepMs);
    elapsed =+ stepMs;
  }
}
---
## Task2-Code with LCD

#include <Wire.h>
#include <LiquidCrystal_I2C.h>

// LCD: address 0x27 or 0x3F depending on your module
LiquidCrystal_I2C lcd(0x20, 16, 2);

// Motor pin definitions
const int motor1Pin1 = 5;
const int motor1Pin2 = 6;
const int motor2Pin1 = 9;
const int motor2Pin2 = 10;

void setup() {
  // Setup LCD
  lcd.init();
  lcd.backlight();
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Robot Starting...");
  delay(2000);

  // Setup motor pins
  pinMode(motor1Pin1, OUTPUT);
  pinMode(motor1Pin2, OUTPUT);
  pinMode(motor2Pin1, OUTPUT);
  pinMode(motor2Pin2, OUTPUT);

  lcd.clear();
}

void loop() {
  forward();
  delay(30000); // forward 30 sec

  backward();
  delay(60000); // backward 60 sec

  alternateRightLeft(60000); // alternate for 60 sec

  stopMotors();
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Motors Stopped");
  while (true); // stop loop permanently
}

// ===== Motor Control Functions =====
void forward() {
  digitalWrite(motor1Pin1, HIGH);
  digitalWrite(motor1Pin2, LOW);
  digitalWrite(motor2Pin1, HIGH);
  digitalWrite(motor2Pin2, LOW);

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Moving Forward");
  lcd.setCursor(0, 1);
  lcd.print("M1 & M2: CW");
}

void backward() {
  digitalWrite(motor1Pin1, LOW);
  digitalWrite(motor1Pin2, HIGH);
  digitalWrite(motor2Pin1, LOW);
  digitalWrite(motor2Pin2, HIGH);

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Moving Backward");
  lcd.setCursor(0, 1);
  lcd.print("M1 & M2: CCW");
}

void turnRight() {
  digitalWrite(motor1Pin1, HIGH);
  digitalWrite(motor1Pin2, LOW);
  digitalWrite(motor2Pin1, LOW);
  digitalWrite(motor2Pin2, HIGH);

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Turning Right");
  lcd.setCursor(0, 1);
  lcd.print("M1: CW, M2: CCW");
}

void turnLeft() {
  digitalWrite(motor1Pin1, LOW);
  digitalWrite(motor1Pin2, HIGH);
  digitalWrite(motor2Pin1, HIGH);
  digitalWrite(motor2Pin2, LOW);

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Turning Left");
  lcd.setCursor(0, 1);
  lcd.print("M1: CCW, M2: CW");
}

void stopMotors() {
  digitalWrite(motor1Pin1, LOW);
  digitalWrite(motor1Pin2, LOW);
  digitalWrite(motor2Pin1, LOW);
  digitalWrite(motor2Pin2, LOW);

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Motors OFF");
  lcd.setCursor(0, 1);
  lcd.print("Idle Mode");
}

void alternateRightLeft(unsigned long totalMs) {
  unsigned long elapsed = 0;
  const unsigned long stepMs = 1000;

  while (elapsed < totalMs) {
    turnRight();
    delay(stepMs);
    elapsed += stepMs;

    if (elapsed >= totalMs) break;

    turnLeft();
    delay(stepMs);
    elapsed += stepMs;
  }

  stopMotors();
}
