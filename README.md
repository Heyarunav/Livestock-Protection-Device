# Livestock-Protection-Device
// Team Hack4Health - Livestock Protection Device Code (Enhanced)
// Includes low power mode & error handling
#include <avr/sleep.h>

// Define sensors and output components
const int pirSensor = 2;       // PIR Motion Sensor connected to pin 2
const int vibrationSensor = 3;  // Vibration Sensor connected to pin 3
const int siren = 4;             // Siren connected to pin 4
const int strobe = 5;            // LED Strobe connected to pin 5

// Error handling setup
int sensorErrorCount = 0;  

void setup() {
  pinMode(pirSensor, INPUT);
  pinMode(vibrationSensor, INPUT);
  pinMode(siren, OUTPUT);
  pinMode(strobe, OUTPUT);

  Serial.begin(9600);
  Serial.println("System Initialized: Hack4Health Livestock Protection Device");
}

void loop() {
  int motionDetected = digitalRead(pirSensor);
  int vibrationDetected = digitalRead(vibrationSensor);

  // Error Handling: Check sensor status
  if (motionDetected == -1 || vibrationDetected == -1) {
    sensorErrorCount++;
    Serial.println("Error: Sensor failed to read.");
    if (sensorErrorCount >= 3) {
      Serial.println("Multiple sensor errors. System halting...");
      while (1);  // Halt system on repeated failure
    }
  } else {
    sensorErrorCount = 0;  // Reset error count on successful reading
  }

  // Trigger alarm if motion or vibration is detected
  if (motionDetected == HIGH || vibrationDetected == HIGH) {
    digitalWrite(siren, HIGH);
    digitalWrite(strobe, HIGH);
    Serial.println("Threat detected! Alarm activated.");
    delay(5000);  // Keep the alarm on for 5 seconds
  } else {
    digitalWrite(siren, LOW);
    digitalWrite(strobe, LOW);

    // Low Power Mode: Enter sleep if no activity detected
    Serial.println("No activity detected. Entering low power mode...");
    goToSleep();
  }

  delay(100);  // Small delay to avoid rapid false triggers
}

// Function to activate low power mode
void goToSleep() {
  set_sleep_mode(SLEEP_MODE_PWR_DOWN);
  sleep_enable();
  sleep_mode();

  // System resumes from sleep when a sensor triggers
  sleep_disable();
  Serial.println("Waking up from low power mode...");
}
