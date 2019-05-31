---
title: Arduino ESP8266 WIFI Switch
---
#include <ESP8266WiFi.h>

const char* ssid = "SSID";
const char* password = "SSID_PW";

int relayPin1 = D1;
int relayPin2 = D2;

WiFiServer server(80);

int state = LOW; //USER

void setup() {
Serial.begin(9600);
delay(10);


pinMode(relayPin1, OUTPUT);
pinMode(relayPin2, OUTPUT);
digitalWrite(relayPin1, LOW);
digitalWrite(relayPin2, LOW);

// Connect to WiFi network
Serial.println();
Serial.println();
Serial.print("Connecting to ");
Serial.println(ssid);

WiFi.begin(ssid, password);

while (WiFi.status() != WL_CONNECTED) {
delay(500);
Serial.print(".");
}
Serial.println("");
Serial.println("WiFi connected");

// Start the server
server.begin();
Serial.println("Server started");

// Print the IP address
Serial.print("Use this URL : ");
Serial.print("http://");
Serial.print(WiFi.localIP());
Serial.println("/");

}

void loop() {
// Check if a client has connected
WiFiClient client = server.available();
if (!client) {
return;
}

// Wait until the client sends some data
Serial.println("new client");
while(!client.available()){
delay(1);
}

// Read the first line of the request
String request = client.readStringUntil('\r');
Serial.println(request);
client.flush();

// Match the request

int value = LOW;
state = digitalRead(relayPin1); //USER
if (request.indexOf("/LED=ON") != -1) 
{
digitalWrite(relayPin1, HIGH);
delay(1000);
digitalWrite(relayPin2, HIGH);
value = HIGH;
}
if (request.indexOf("/LED=OFF") != -1)
{
digitalWrite(relayPin1, LOW);
digitalWrite(relayPin2, LOW);
value = LOW;
}

//USER

if (request.indexOf("/LED=SWITCH") != -1 && state == LOW)
{
digitalWrite(relayPin1, HIGH);
delay(1000);
digitalWrite(relayPin2, HIGH);
value = HIGH;
}
if (request.indexOf("/LED=SWITCH") != -1 && state == HIGH)
{
digitalWrite(relayPin1, LOW);
digitalWrite(relayPin2, LOW);
value = LOW;
}

if (request.indexOf("/ONE") != -1) 
{
digitalWrite(relayPin1, HIGH);
delay(1000);
digitalWrite(relayPin2, HIGH);
delay(1000);
digitalWrite(relayPin1, LOW);
digitalWrite(relayPin2, LOW);
}

if (request.indexOf("/TWO") != -1) 
{
digitalWrite(relayPin1, HIGH);
delay(1000);
digitalWrite(relayPin2, HIGH);
delay(2000);
digitalWrite(relayPin1, LOW);
digitalWrite(relayPin2, LOW);
}

if (request.indexOf("/THREE") != -1) 
{
digitalWrite(relayPin1, HIGH);
delay(1000);
digitalWrite(relayPin2, HIGH);
delay(3000);
digitalWrite(relayPin1, LOW);
digitalWrite(relayPin2, LOW);
}

if (request.indexOf("/FIVE") != -1) 
{
digitalWrite(relayPin1, HIGH);
delay(1000);
digitalWrite(relayPin2, HIGH);
delay(5000);
digitalWrite(relayPin1, LOW);
digitalWrite(relayPin2, LOW);
}

if (request.indexOf("/TEN") != -1) 
{
digitalWrite(relayPin1, HIGH);
delay(1000);
digitalWrite(relayPin2, HIGH);
delay(10000);
digitalWrite(relayPin1, LOW);
digitalWrite(relayPin2, LOW);
}

// Return the response
client.println("HTTP/1.1 200 OK");
client.println("Content-Type: text/html");
client.println(""); // do not forget this one
client.println("<!DOCTYPE HTML>");
client.println("<html>");

client.print("Relay is now: ");

if(value == HIGH) {
client.print("On"); 
} else {
client.print("Off or temporarily ON");
}
client.println("<br><br>");
client.println("Click <a href=\"/LED=ON\">here</a> to FIRE<br>");
client.println("Click <a href=\"/LED=OFF\">here</a> to STOP FIRING<br>");
//USER
client.println("Click <a href=\"/LED=SWITCH\">here</a> to SWITCHING firing state<br>");
client.println("Click <a href=\"/ONE\">here</a> to fire for 1 second<br>");
client.println("Click <a href=\"/TWO\">here</a> to fire for 2 seconds<br>");
client.println("Click <a href=\"/THREE\">here</a> to fire for 3 seconds<br>");
client.println("Click <a href=\"/FIVE\">here</a> to fire for 5 seconds<br>");
client.println("Click <a href=\"/TEN\">here</a> to fire for 10 seconds<br>");
client.println("</html>");

delay(1);
Serial.println("Client disconnected");
Serial.println("");

}