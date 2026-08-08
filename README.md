[README (1).md](https://github.com/user-attachments/files/30584691/README.1.md)
# Line Follower Robot (LFR) Using IR Sensor and Arduino

## 📌 Overview
This project implements a **Line Follower Robot (LFR)** that autonomously detects and follows a black line on a white surface (or vice versa) using **IR (Infrared) sensors** and an **Arduino microcontroller**. The robot continuously reads sensor data and adjusts motor speed/direction in real time to stay on the track, making it a great introductory project in embedded systems, robotics, and control logic.

## 🎯 Objective
- Detect a line path using IR sensors.
- Process sensor data on Arduino to determine the robot's position relative to the line.
- Drive two DC motors via a motor driver to keep the robot centered on the line.

## 🛠️ Components Required

| Component | Quantity | Purpose |
|---|---|---|
| Arduino UNO | 1 | Main controller |
| IR Sensor Modules (TCRT5000 or similar) | 2–5 | Line detection |
| L298N / L293D Motor Driver | 1 | Controls motor direction & speed |
| DC Geared Motors (hobby motors) | 2 | Wheel drive |
| Robot Chassis with Wheels + Caster | 1 | Body/frame |
| Li-ion / 9V Battery Pack | 1 | Power supply |
| Jumper Wires | As needed | Connections |
| Breadboard (optional) | 1 | Prototyping |

## ⚙️ Working Principle
1. IR sensors emit infrared light and measure the reflection.
2. **White surface** reflects more IR light → sensor reads LOW (or HIGH, depending on module).
3. **Black line** absorbs IR light → sensor reads the opposite state.
4. Arduino reads the digital output of each IR sensor.
5. Based on which sensor(s) detect the line, Arduino decides:
   - **Move forward** – center sensor(s) on line.
   - **Turn left** – right sensor detects line (robot has drifted left of line).
   - **Turn right** – left sensor detects line (robot has drifted right of line).
   - **Stop / search** – no sensor detects line (lost track).

## 🔌 Circuit Connections (2-Sensor Basic Version)

| IR Sensor | Arduino Pin |
|---|---|
| Left Sensor OUT | D2 |
| Right Sensor OUT | D3 |
| Left Sensor VCC | 5V |
| Right Sensor VCC | 5V |
| GND | GND |

| Motor Driver (L298N) | Arduino Pin |
|---|---|
| IN1 | D5 |
| IN2 | D6 |
| IN3 | D9 |
| IN4 | D10 |
| ENA | D11 (PWM) |
| ENB | D12 (PWM) |

## Arduino Code
---
// Line Follower Robot using 2 IR Sensors and Arduino

// IR Sensor Pins
#define LEFT_SENSOR 2
#define RIGHT_SENSOR 3

// Motor Driver Pins
#define IN1 5
#define IN2 6
#define IN3 9
#define IN4 10
#define ENA 11
#define ENB 12

int motorSpeed = 150; // 0-255

void setup() {
  pinMode(LEFT_SENSOR, INPUT);
  pinMode(RIGHT_SENSOR, INPUT);

  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
  pinMode(ENA, OUTPUT);
  pinMode(ENB, OUTPUT);

  Serial.begin(9600);
}

void loop() {
  int leftValue = digitalRead(LEFT_SENSOR);
  int rightValue = digitalRead(RIGHT_SENSOR);

  if (leftValue == LOW && rightValue == LOW) {
    moveForward();
  }
  else if (leftValue == HIGH && rightValue == LOW) {
    turnLeft();
  }
  else if (leftValue == LOW && rightValue == HIGH) {
    turnRight();
  }
  else {
    stopMotors();
  }
}

void moveForward() {
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
  analogWrite(ENA, motorSpeed);
  analogWrite(ENB, motorSpeed);
}

void turnLeft() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
  analogWrite(ENA, motorSpeed);
  analogWrite(ENB, motorSpeed);
}

void turnRight() {
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
  analogWrite(ENA, motorSpeed);
  analogWrite(ENB, motorSpeed);
}

void stopMotors() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}
---

## 🚀 Future Improvements
- Add **PID control** for smoother, faster line following.
- Use **5+ IR sensor array** for better precision on curves.
- Add **Bluetooth/Wi-Fi module** for remote monitoring or mode switching.
- Implement **obstacle avoidance** using an ultrasonic sensor.


## 📜 License
This project is open-source and free to use for educational purposes.
