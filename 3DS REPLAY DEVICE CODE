#include <SPI.h>
#include <SD.h>

// Pin assignments
const int chipSelectPin = 9;
const int buttonPin = 4;
const int ledPin = 5;
const int pin1 = 4;
const int pin2 = 5;
const int pin3 = 6;

// Delay times
const int buttonPressDelay = 1000;
const int inputsDelay = 500;

// File handling
File myFile;

void setup() {
  Serial.begin(300);
  pinMode(buttonPin, INPUT);
  pinMode(ledPin, OUTPUT);
  pinMode(pin1, OUTPUT);
  pinMode(pin2, OUTPUT);
  pinMode(pin3, OUTPUT);
  pinMode(chipSelectPin, OUTPUT);

  if (!SD.begin(chipSelectPin)) {
    Serial.println("SD Card initialization failed!");
    return;
  }

  if (!openFile("macro.txt")) {
    Serial.println("Error opening file!");
    return;
  }
}

void loop() {
  simulateMacroButtonPress();
  delay(buttonPressDelay);
  readAndSendMacroInputs();
  delay(inputsDelay);
}

void simulateMacroButtonPress() {
  char buttonPress;
  if (myFile.read(&buttonPress, sizeof(buttonPress)) > 0) {
    digitalWrite(ledPin, HIGH);
    processButtonPress(buttonPress);
    digitalWrite(ledPin, LOW);
  } else {
    // Reached the end of the macro input, close the file and reopen it for the next loop
    resetFile();
  }
}

void processButtonPress(char buttonPress) {
  // Process the button press data and send it through the pins
  // Example: If buttonPress is 'A', set pin1 HIGH and the others LOW
  if (buttonPress == 'A') {
    digitalWrite(pin1, HIGH);
    digitalWrite(pin2, LOW);
    digitalWrite(pin3, LOW);
  } else if (buttonPress == 'B') {
    // Process other button press data
  }
  // Add more conditions for other button press data
}

void readAndSendMacroInputs() {
  char inputs[2];
  int bytesRead = myFile.readBytes(inputs, sizeof(inputs));
  if (bytesRead > 0) {
    digitalWrite(ledPin, HIGH);
    processInputs(inputs);
    digitalWrite(ledPin, LOW);
  } else {
    // Reached the end of the macro input, close the file and reopen it for the next loop
    resetFile();
  }
}

void processInputs(char inputs[2]) {
  // Process the input data and send it through the pins
  // Example: If inputs[0] is '1', set pin1 HIGH; if inputs[0] is '0', set pin1 LOW
  // Repeat the same for pin2 and pin3 using inputs[1]
  digitalWrite(pin1, inputs[0] == '1' ? HIGH : LOW);
  digitalWrite(pin2, inputs[1] == '1' ? HIGH : LOW);
  // Process other input data
}

void resetFile() {
  myFile.close();
  openFile("macro.txt");
}

bool openFile(const char* filename) {
  myFile = SD.open(filename);
  return myFile;
}
