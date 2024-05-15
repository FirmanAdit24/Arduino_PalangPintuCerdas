#include <Servo.h>

int pinButton3 = 4;
int pinButton2 = 3;
int pinButton1 = 2;
int buttonState1 = LOW;
int buttonState2 = LOW;
int buttonState3 = LOW;
int lastButtonState1 = LOW;
int lastButtonState2 = LOW;
int lastButtonState3 = LOW;
int servoPosition = 90;
int isSensorActive = 0;
int pinLed1 = 7;
int pinLed2 = 6;
int pinLed3 = 5;
const int pinEcho = 8;
const int pinTrig = 9;
Servo servo;
long duration;
int distance;
bool button1Pressed = false;
bool servoActive = false;

void setup() {
  servo.attach(10);
  pinMode(pinButton1, INPUT);
  pinMode(pinButton2, INPUT);
  pinMode(pinButton3, INPUT);
  pinMode(pinLed1, OUTPUT);
  pinMode(pinLed2, OUTPUT);
  pinMode(pinLed3, OUTPUT);
  pinMode(pinEcho, INPUT);
  pinMode(pinTrig, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  buttonState1 = digitalRead(pinButton1);
  buttonState2 = digitalRead(pinButton2);
  buttonState3 = digitalRead(pinButton3);

  if (!button1Pressed) {
    if (buttonState1 != lastButtonState1 && buttonState1 == HIGH) {
      digitalWrite(pinLed1, HIGH);
      digitalWrite(pinLed2, LOW);
      digitalWrite(pinLed3, LOW);
      isSensorActive = 0;

      // Mengatur servo ke posisi awal (90 derajat)
      servo.write(90);
      delay(500);

      // Menonaktifkan fungsionalitas tombol 2 dan 3
      button1Pressed = true;
    }
  } else {
    if (buttonState1 != lastButtonState1 && buttonState1 == HIGH) {
      // Jika tombol 1 ditekan lagi, aktifkan kembali fungsionalitas tombol 2 dan 3
      button1Pressed = false;
      
      // Matikan LED1
      digitalWrite(pinLed1, LOW);
    }
    // Jika tombol 1 ditekan, nonaktifkan fungsionalitas tombol 2 dan 3
    buttonState2 = LOW;
    buttonState3 = LOW;
  }
  
  if (buttonState2 != lastButtonState2 && buttonState2 == HIGH) {
    digitalWrite(pinLed1, LOW);
    digitalWrite(pinLed2, HIGH);
    digitalWrite(pinLed3, LOW);
    isSensorActive = 0;

    if (!servoActive) {
      // Jika servo tidak aktif, aktifkan servo dan set flag servoActive
      servo.write(0);
      servoActive = true;
    } else {
      // Jika servo aktif, matikan servo dan reset flag servoActive
      servo.write(90);
      servoActive = false;
    }

    delay(300);  // Delay untuk debouncing
  }

  if (buttonState3 != lastButtonState3 && buttonState3 == HIGH) {
    digitalWrite(pinLed1, LOW);  
    digitalWrite(pinLed2, LOW);
    digitalWrite(pinLed3, HIGH);
    isSensorActive = 1;
    delay(500);
  }

  if (isSensorActive == 1 && getDistance() < 20) {
    servo.write(0);
    delay(5000);
  } else if (!servoActive) {
    // Jika servo tidak aktif, kembalikan servo ke posisi awal (90 derajat)
    servo.write(90);
  }
  

  lastButtonState1 = buttonState1;
  lastButtonState2 = buttonState2;
  lastButtonState3 = buttonState3;
}

int getDistance() {
  digitalWrite(pinTrig, LOW);
  delayMicroseconds(2);
  digitalWrite(pinTrig, HIGH);
  delayMicroseconds(10);
  digitalWrite(pinTrig, LOW);
  duration = pulseIn(pinEcho, HIGH);
  distance = duration * 0.034 / 2;
  Serial.print("Jarak: ");
  Serial.println(distance);
  delay(100);

  return distance;
}
