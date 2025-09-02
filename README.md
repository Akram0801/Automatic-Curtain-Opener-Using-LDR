# Automatic-Curtain-Opener-Using-LDR
// Template ID, Device Name and Auth Token are provided by the Blynk.Cloud
// See the Device Info tab, or Template settings
#define BLYNK_TEMPLATE_ID           "xxxxxxxxxxxxxxxxx" //Paste your template ID here
#define BLYNK_DEVICE_NAME           "MyIOT" //Paste your device name
#define BLYNK_AUTH_TOKEN            "xxxxxxxxxxxxxxxxx" //Paste your Auth Token
#define blinds 13
#define indoor 34
#define outdoor 35
// Comment this out to disable prints and save space
//#define BLYNK_PRINT Serial
//#include <WiFi.h>
//#include <WiFiClient.h>
#include <BlynkSimpleEsp32.h>
#include <ESP32Servo.h>
Servo myservo;
char auth[] = BLYNK_AUTH_TOKEN;
// Your WiFi credentials.
// Set password to "" for open networks.
char ssid[] = "xxxxxxxxx"; //Enter your credentials
char pass[] = "xxxxxxxxx";
BlynkTimer timer;
// This function is called every time the Virtual Pin 0 state changes
BLYNK_WRITE(V0)
{
  // Set incoming value from pin V0 to a variable
  int value = param.asInt();
  myservo.write(map(value, 0, 100, 0, 180));
}
// This function sends Arduino's uptime every second to Virtual Pin 2.
void myTimerEvent()
{
  // You can send any value at any time.
  // Please don't send more that 10 values per second.
  Blynk.virtualWrite(V1, analogRead(indoor));
  Blynk.virtualWrite(V2, analogRead(outdoor));
}
void setup()
{
  // Debug console
  Serial.begin(115200);
  ESP32PWM::allocateTimer(0);
  ESP32PWM::allocateTimer(1);
  ESP32PWM::allocateTimer(2);
  ESP32PWM::allocateTimer(3);
  myservo.setPeriodHertz(50);    // standard 50 hz servo
  myservo.attach(blinds, 1000, 2000);
  pinMode(indoor, INPUT);
  pinMode(outdoor, INPUT);
  Blynk.begin(auth, ssid, pass);
  // You can also specify server:
  //Blynk.begin(auth, ssid, pass, "blynk.cloud", 80);
  //Blynk.begin(auth, ssid, pass, IPAddress(192,168,1,100), 8080);
  // Setup a function to be called every second
  timer.setInterval(1000L, myTimerEvent); 
}
void loop()
{
  Blynk.run();
  timer.run();
  // You can inject your own code or combine it with other sketches.
  // Check other examples on how to communicate with Blynk. Remember
  // to avoid delay() function!
}
