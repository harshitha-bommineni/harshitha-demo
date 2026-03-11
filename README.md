# Smart Blind Stick 
This is my first GitHub repository
This includes the code which I used for the blind stick project in my college for Engineers Day exhibition 

/* * Project: Smart Blind Stick (Obstacle Sensing System)
 * Hardware: Arduino Uno / ATmega328P
 * Language: Embedded C / C++
 */

#include <stdio.h>

// Defining pins as constants for better readability
const int TRIG_PIN = 9;   // Ultrasonic Trigger
const int ECHO_PIN = 10;  // Ultrasonic Echo
const int BUZZER = 11;    // Alarm Output

// Global variables for sensor data
long pulse_duration;
int distance_cm;

void setup() {
  // Initialize pin modes
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  pinMode(BUZZER, OUTPUT);
  
  // Serial communication for debugging and validation
  Serial.begin(9600);
  Serial.println("System Initialized: Ultrasonic Distance Monitor");
}

void loop() {
  // Step 1: Trigger the sensor with a 10us HIGH pulse
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  // Step 2: Measure the pulse duration on Echo pin
  pulse_duration = pulseIn(ECHO_PIN, HIGH);

  // Step 3: Convert time to distance (Speed of sound = 0.034 cm/us)
  // Logic: Distance = (Time * Speed) / 2
  distance_cm = pulse_duration * 0.034 / 2;

  // Step 4: Logic for Buzzer-based feedback
  // Predefined threshold: 30cm
  if (distance_cm > 0 && distance_cm <= 30) {
    // Alert logic: Frequent beeping for closer objects
    digitalWrite(BUZZER, HIGH);
    delay(100); 
    digitalWrite(BUZZER, LOW);
    
    Serial.print("CRITICAL: Obstacle detected at ");
    Serial.print(distance_cm);
    Serial.println(" cm");
  } else {
    digitalWrite(BUZZER, LOW);
  }

  // Small delay to stabilize the sensor
  delay(250);
}
