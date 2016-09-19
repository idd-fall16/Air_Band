# Air_Band
A combination of drumsticks, guitar, and box MIDI controller which controls whether notes are on or off, pitch, and tambre.

Guitar Code

/******************
 * MIDI over Serial Example
 * for RedBear Duo using a modified ardumidi library (included)
 * Sends MIDI messages over the Serial (USB) connection to a laptop
 * Demonstrates note on, note off, and a controller change
 * Bjoern Hartmann for IDD, Fall 2016
 */
#include "ardumidi.h"

// turn off cloud functionality
#if defined(ARDUINO) 
SYSTEM_MODE(MANUAL); 
#endif

int strum1 = 0, strum2 = 0;

// Analog Sensor variables
int softpot;

// Digital Sensor variables
int switch1, switch2;

void setup() {
  Serial.begin(115200);

  // Pin configurations
  pinMode(A0, INPUT);
  pinMode(D0, INPUT);
  pinMode(D1, INPUT);
}

void loop() {
  // Read the sensors
  softpot = analogRead(A0);
  switch1 = digitalRead(D0);
  switch2 = digitalRead(D1);

 
  // Map the sensor value (MIGHT BE DIFFERENT HERE!!!!)
  softpot = map(softpot, 1900, 4100, 0, 120);

//  Serial.println(switch1);
//  Serial.println(switch2);
//  Serial.println("\n");
 
  // Send MIDI controls via Serial USB
  if ((switch1 == 0) && (strum1 != 1)) {
    midi_note_on(1, scale1(softpot), 127);
//    Serial.println("Strum1");
    strum1 = 1;
    strum2 = 0;
  } else if ((switch2 == 0) && (strum2 != 1)) {
    midi_note_on(1, scale2(softpot), 127);
//    Serial.println("Strum2");
    strum1 = 0;
    strum2 = 1;
  } else if ((switch1 == 1) && (switch2 == 1)) {
    midi_note_on(1, 127, 0);
//    Serial.println("None");
    strum1 = 0;
    strum2 = 0;
  }
  
  delay(50);
}


int scale1(int val) {
  if (val < 18) {
    return 72;
  } else if (val < 35) {
    return 76;
  } else if (val < 52) {
    return 79;
  } else if (val < 69) {
    return 83;
  } else if (val < 86) {
    return 86;
  } else if (val < 103) {
    return 89;
  } else {
    return 93;
  }
}


int scale2(int val) {
  if (val < 18) {
    return 74;
  } else if (val < 35) {
    return 77;
  } else if (val < 52) {
    return 81;
  } else if (val < 69) {
    return 84;
  } else if (val < 86) {
    return 88;
  } else if (val < 103) {
    return 91;
  } else {
    return 95;
  }
}

