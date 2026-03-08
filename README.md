# harshitha-demo
This is my first GitHub repository
This includes the code which I used for the blind stick project in my college for Engineers Day exhibition 

#include <SoftwareSerial.h>
#include <TinyGPS++.h>
// Pin definitions
const int trigPin = 9;
const int echoPin = 10;
const int buzzerPin = 8;

// GPS setup
SoftwareSerial gpsSerial(4, 3); // RX, TX
TinyGPSPlus gps;

long duration;
int distance;

void setup() 
{
    pinMode(trigPin, OUTPUT);
    pinMode(echoPin, INPUT);
    pinMode(buzzerPin, OUTPUT);

    Serial.begin(9600);
    gpsSerial.begin(9600);

    Serial.println("Blind Stick System Started");
}

int getDistance()
{
    digitalWrite(trigPin, LOW);
    delayMicroseconds(2);

    digitalWrite(trigPin, HIGH);
    delayMicroseconds(10);
    digitalWrite(trigPin, LOW);

    duration = pulseIn(echoPin, HIGH);
    distance = duration * 0.034 / 2;

    return distance;
}

void obstacleAlert(int d)
{
    if (d < 20)
    {
        digitalWrite(buzzerPin, HIGH); // continuous beep
    }
    else if (d >= 20 && d < 50)
    {
        digitalWrite(buzzerPin, HIGH);
        delay(200);
        digitalWrite(buzzerPin, LOW);
        delay(200);
    }
    else
    {
        digitalWrite(buzzerPin, LOW);
    }
}

void readGPS()
{
    while (gpsSerial.available())
    {
        gps.encode(gpsSerial.read());

        if (gps.location.isUpdated())
        {
            Serial.print("Latitude: ");
            Serial.println(gps.location.lat(), 6);

            Serial.print("Longitude: ");
            Serial.println(gps.location.lng(), 6);
        }
    }
}

void loop() 
{
    int dist = getDistance();

    Serial.print("Distance: ");
    Serial.print(dist);
    Serial.println(" cm");

    obstacleAlert(dist);

    readGPS();

    delay(200);
}