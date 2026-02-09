// --- Constants (Pin Definitions) ---
const int greenBtn = 2;
const int greenLight = 3;

const int yellowBtn = 6;
const int yellowLight = 5;

const int blueBtn = 9;
const int blueLight = 10;

const int redBtn = 12;
const int redLight = 13;

// --- Game Variables ---
int gameSequence[50]; // Stores the pattern to memorize
int currentLevel = 1;
int velocity = 1000;  // Speed of the lights

void setup() {
  Serial.begin(9600);

  // INPUT_PULLUP allows buttons to work without extra resistors
  // (Wire buttons from PIN to GROUND)
  pinMode(greenBtn, INPUT_PULLUP);
  pinMode(yellowBtn, INPUT_PULLUP);
  pinMode(blueBtn, INPUT_PULLUP);
  pinMode(redBtn, INPUT_PULLUP);

  pinMode(greenLight, OUTPUT);
  pinMode(yellowLight, OUTPUT);
  pinMode(blueLight, OUTPUT);
  pinMode(redLight, OUTPUT);

  allLedOff();
  randomSeed(analogRead(0)); // Randomize the game
  startingSequence(); // Play start animation once
}

void loop() {
  // 1. Create the next step in the sequence (0=Green, 1=Yellow, 2=Blue, 3=Red)
  gameSequence[currentLevel - 1] = random(0, 4);

  // 2. Show the sequence to the player
  showSequence();

  // 3. Wait for player input and check if it's right
  if (checkPlayerInput()) {
    // If correct:
    delay(500);
    currentLevel++; // Go to next level
    if (velocity > 200) velocity -= 50; // Speed up
  } else {
    // If wrong:
    losingSequence();
    currentLevel = 1; // Reset to level 1
    velocity = 1000; // Reset speed
    delay(1000);
    startingSequence();
  }
}

// --- Helper Functions ---

void showSequence() {
  for (int i = 0; i < currentLevel; i++) {
    int color = gameSequence[i];
    
    // Turn on the correct light
    if (color == 0) digitalWrite(greenLight, HIGH);
    else if (color == 1) digitalWrite(yellowLight, HIGH);
    else if (color == 2) digitalWrite(blueLight, HIGH);
    else if (color == 3) digitalWrite(redLight, HIGH);
    
    delay(velocity);
    
    // Turn all off
    allLedOff();
    delay(200);
  }
}

bool checkPlayerInput() {
  for (int i = 0; i < currentLevel; i++) {
    int expected = gameSequence[i];
    int pressed = -1;
    
    // Wait for a button to be pressed
    while (pressed == -1) {
      // Note: LOW means pressed when using INPUT_PULLUP
      if (digitalRead(greenBtn) == LOW) pressed = 0;
      else if (digitalRead(yellowBtn) == LOW) pressed = 1;
      else if (digitalRead(blueBtn) == LOW) pressed = 2;
      else if (digitalRead(redBtn) == LOW) pressed = 3;
    }

    // Light up the button the user pressed for feedback
    if (pressed == 0) digitalWrite(greenLight, HIGH);
    if (pressed == 1) digitalWrite(yellowLight, HIGH);
    if (pressed == 2) digitalWrite(blueLight, HIGH);
    if (pressed == 3) digitalWrite(redLight, HIGH);

    // Wait until button is released (Debounce)
    while (digitalRead(greenBtn) == LOW || digitalRead(yellowBtn) == LOW || 
           digitalRead(blueBtn) == LOW || digitalRead(redBtn) == LOW) {
      delay(10); 
    }
    
    allLedOff(); // Turn off light after release

    // Check if the wrong button was pressed
    if (pressed != expected) {
      return false; 
    }
  }
  return true;
}

void allLedOff() {
  digitalWrite(greenLight, LOW);
  digitalWrite(yellowLight, LOW);
  digitalWrite(blueLight, LOW);
  digitalWrite(redLight, LOW);
}

void startingSequence() {
  // Simple sweep animation
  digitalWrite(greenLight, HIGH); delay(100);
  digitalWrite(yellowLight, HIGH); delay(100);
  digitalWrite(blueLight, HIGH); delay(100);
  digitalWrite(redLight, HIGH); delay(100);
  delay(500);
  allLedOff();
  delay(500);
}

void losingSequence() {
  // Flash all lights 3 times
  for(int i=0; i<3; i++){
    digitalWrite(greenLight, HIGH);
    digitalWrite(yellowLight, HIGH);
    digitalWrite(blueLight, HIGH);
    digitalWrite(redLight, HIGH);
    delay(300);
    allLedOff();
    delay(300);
  }
}
