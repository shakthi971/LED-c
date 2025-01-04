#include <ESP8266WiFi.h>
#include <ESP8266HTTPClient.h>

const char* ssid = "Your_WiFi_SSID";       // WiFi SSID
const char* password = "Your_WiFi_Password"; // WiFi Password
const char* fileURL = "https://raw.githubusercontent.com/username/repository/main/firmware.bin"; // GitHub File URL

void setup() {
    Serial.begin(115200);
    WiFi.begin(ssid, password);
    while (WiFi.status() != WL_CONNECTED) {
        delay(1000);
        Serial.println("Connecting to WiFi...");
    }
    Serial.println("Connected to WiFi");

    downloadFile(); // File download කිරීම
}

void loop() {
    // Main loop එක හිස්ව තබන්න (අවශ්‍ය නම්, අනෙකුත් tasks එකතු කරන්න)
}

void downloadFile() {
    if (WiFi.status() == WL_CONNECTED) {
        HTTPClient http;
        http.begin(fileURL); // File URL එක begin කිරීම
        int httpCode = http.GET(); // HTTP GET request එක send කිරීම

        if (httpCode == HTTP_CODE_OK) {
            String payload = http.getString(); // File content එක ලබා ගැනීම
            Serial.println("File content:");
            Serial.println(payload); // File content එක print කිරීම
        } else {
            Serial.printf("HTTP GET request failed, error code: %d\n", httpCode);
        }
        http.end();
    }
}
