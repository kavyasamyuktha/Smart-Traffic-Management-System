#include <TM1637Display.h>

// Define the pins for the TM1637 display for each road
#define CLK_PIN1 27
#define DIO_PIN1 26
#define CLK_PIN2 8
#define DIO_PIN2 9
#define CLK_PIN3 47
#define DIO_PIN3 46
#define CLK_PIN4 37
#define DIO_PIN4 36

// Define maximum sensor distance
#define MAX_DISTANCE 350 

// Define LED pins for each road
#define GREEN_PIN1 30
#define YELLOW_PIN1 29
#define RED_PIN1 28
#define GREEN_PIN2 7
#define YELLOW_PIN2 6
#define RED_PIN2 5
#define GREEN_PIN3 48
#define YELLOW_PIN3 49
#define RED_PIN3 50
#define GREEN_PIN4 40
#define YELLOW_PIN4 39
#define RED_PIN4 38

// Define ultrasonic sensor pins for each road
#define TRIG_PIN1_1 23
#define ECHO_PIN1_1 22
#define TRIG_PIN1_2 25
#define ECHO_PIN1_2 24
#define TRIG_PIN2_1 13
#define ECHO_PIN2_1 12
#define TRIG_PIN2_2 11
#define ECHO_PIN2_2 10
#define TRIG_PIN3_1 43
#define ECHO_PIN3_1 42
#define TRIG_PIN3_2 45
#define ECHO_PIN3_2 44
#define TRIG_PIN4_1 33
#define ECHO_PIN4_1 32
#define TRIG_PIN4_2 35
#define ECHO_PIN4_2 34

TM1637Display display1(CLK_PIN1, DIO_PIN1);
TM1637Display display2(CLK_PIN2, DIO_PIN2);
TM1637Display display3(CLK_PIN3, DIO_PIN3);
TM1637Display display4(CLK_PIN4, DIO_PIN4);

int currentRoad = 0;

void setup() {
    // Initialize the TM1637 displays
    display1.setBrightness(0x0f);
    display2.setBrightness(0x0f);
    display3.setBrightness(0x0f);
    display4.setBrightness(0x0f);
    
    // Initialize LED pins
    initializeLEDs();
    
    // Initialize ultrasonic sensor pins
    initializeSensors();
}

void loop() {
    // Read values from sensors for each road
    int distance1_1 = readUltrasonic(TRIG_PIN1_1, ECHO_PIN1_1);
    int distance1_2 = readUltrasonic(TRIG_PIN1_2, ECHO_PIN1_2);
    int distance2_1 = readUltrasonic(TRIG_PIN2_1, ECHO_PIN2_1);
    int distance2_2 = readUltrasonic(TRIG_PIN2_2, ECHO_PIN2_2);
    int distance3_1 = readUltrasonic(TRIG_PIN3_1, ECHO_PIN3_1);
    int distance3_2 = readUltrasonic(TRIG_PIN3_2, ECHO_PIN3_2);
    int distance4_1 = readUltrasonic(TRIG_PIN4_1, ECHO_PIN4_1);
    int distance4_2 = readUltrasonic(TRIG_PIN4_2, ECHO_PIN4_2);

    // Manage traffic for the current road
    switch (currentRoad) {
        case 1:
            manageTraffic(1, distance1_1, distance1_2, &display1, GREEN_PIN1, YELLOW_PIN1, RED_PIN1);
            break;
        case 2:
            manageTraffic(2, distance2_1, distance2_2, &display2, GREEN_PIN2, YELLOW_PIN2, RED_PIN2);
            break;
        case 3:
            manageTraffic(3, distance3_1, distance3_2, &display3, GREEN_PIN3, YELLOW_PIN3, RED_PIN3);
            break;
        case 4:
            manageTraffic(4, distance4_1, distance4_2, &display4, GREEN_PIN4, YELLOW_PIN4, RED_PIN4);
            break;
    }

    // Move to the next road
    currentRoad = (currentRoad % 4) + 1;

    // Ensure all 7-segment displays are off and only red lights are on
    clearAllDisplays();
    setAllToRed();
}

int readUltrasonic(int trigPin, int echoPin) {
    digitalWrite(trigPin, LOW);
    delayMicroseconds(2);
    digitalWrite(trigPin, HIGH);
    delayMicroseconds(10);
    digitalWrite(trigPin, LOW);
    
    long duration = pulseIn(echoPin, HIGH);
    int distance = duration * 0.034 / 2;
    return distance;
}

void manageTraffic(int roadNumber, int distance1, int distance2, TM1637Display *display, int greenPin, int yellowPin, int redPin) {
    int greenDuration = 0;
    
    // Determine the green duration based on sensor distances
    if (distance1 < MAX_DISTANCE && distance2 < MAX_DISTANCE) {
        greenDuration = 15;
    } else if (distance1 < MAX_DISTANCE) {
        greenDuration = 10;
    } else {
        greenDuration = 0;
    }

    if (greenDuration > 0) {
        digitalWrite(greenPin, HIGH);
        digitalWrite(yellowPin, LOW);
        digitalWrite(redPin, LOW);
        showTimer(display, greenDuration, greenPin, yellowPin, redPin);
    } else {
        digitalWrite(greenPin, LOW);
        digitalWrite(yellowPin, LOW);
        digitalWrite(redPin, HIGH);
        display->clear();
    }
}

void showTimer(TM1637Display *display, int duration, int greenPin, int yellowPin, int redPin) {
    for (int i = duration; i >= 0; i--) {
        display->showNumberDec(i);
        if (i <= 5) {
            digitalWrite(greenPin, LOW);
            digitalWrite(yellowPin, HIGH);
            digitalWrite(redPin, LOW);
        }
        if (i != 0){
          delay(1000);
        }
    }
}

void initializeLEDs() {
    // Initialize all LED pins
    pinMode(GREEN_PIN1, OUTPUT);
    pinMode(YELLOW_PIN1, OUTPUT);
    pinMode(RED_PIN1, OUTPUT);
    pinMode(GREEN_PIN2, OUTPUT);
    pinMode(YELLOW_PIN2, OUTPUT);
    pinMode(RED_PIN2, OUTPUT);
    pinMode(GREEN_PIN3, OUTPUT);
    pinMode(YELLOW_PIN3, OUTPUT);
    pinMode(RED_PIN3, OUTPUT);
    pinMode(GREEN_PIN4, OUTPUT);
    pinMode(YELLOW_PIN4, OUTPUT);
    pinMode(RED_PIN4, OUTPUT);
}

void initializeSensors() {
    // Initialize all ultrasonic sensor pins
    pinMode(TRIG_PIN1_1, OUTPUT);
    pinMode(ECHO_PIN1_1, INPUT);
    pinMode(TRIG_PIN1_2, OUTPUT);
    pinMode(ECHO_PIN1_2, INPUT);
    pinMode(TRIG_PIN2_1, OUTPUT);
    pinMode(ECHO_PIN2_1, INPUT);
    pinMode(TRIG_PIN2_2, OUTPUT);
    pinMode(ECHO_PIN2_2, INPUT);
    pinMode(TRIG_PIN3_1, OUTPUT);
    pinMode(ECHO_PIN3_1, INPUT);
    pinMode(TRIG_PIN3_2, OUTPUT);
    pinMode(ECHO_PIN3_2, INPUT);
    pinMode(TRIG_PIN4_1, OUTPUT);
    pinMode(ECHO_PIN4_1, INPUT);
    pinMode(TRIG_PIN4_2, OUTPUT);
    pinMode(ECHO_PIN4_2, INPUT);
}

void clearAllDisplays() {
    display1.clear();
    display2.clear();
    display3.clear();
    display4.clear();
}

void setAllToRed() {
    digitalWrite(GREEN_PIN1, LOW);
    digitalWrite(YELLOW_PIN1, LOW);
    digitalWrite(RED_PIN1, HIGH);
    
    digitalWrite(GREEN_PIN2, LOW);
    digitalWrite(YELLOW_PIN2, LOW);
    digitalWrite(RED_PIN2, HIGH);
    
    digitalWrite(GREEN_PIN3, LOW);
    digitalWrite(YELLOW_PIN3, LOW);
    digitalWrite(RED_PIN3, HIGH);
    
    digitalWrite(GREEN_PIN4, LOW);
    digitalWrite(YELLOW_PIN4, LOW);
    digitalWrite(RED_PIN4, HIGH);
}
