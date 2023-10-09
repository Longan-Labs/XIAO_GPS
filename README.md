---
description: TBD
title: TBD
keywords:
- XIAO
image: TBD
slug: XIAO GPS
last_update:
  date: 2023-10-7
  author: Stephen Lo
---

<p style={{textAlign: 'center'}}><img src="https://raw.githubusercontent.com/Longan-Labs/XIAO_GPS/main/IMG/back.png" alt="pir" width={600} height="auto" /></p>

Welcome to the L76-L GNSS for XIAO - the latest addition to the XIAO product series by Seeed Studio. This GNSS module not only offers precise positioning capabilities for your projects but its seamless integration with the XIAO main controller makes it a powerful tool. Whether you're designing a mobile application, a tracking device, or simply wish to add geolocation capabilities to your project, this module is your go-to choice.

<p style={{textAlign: 'center'}}><a href="https://www.seeedstudio.com/-Grove-VOC-and-eCO2-Gas-Sensor-(SGP30)-p-3071.html" target="_blank"><img src="https://files.seeedstudio.com/wiki/Seeed-WiKi/docs/images/300px-Get_One_Now_Banner-ragular.png" /></a></p>

## Features

- Multi-Constellation Support: Supports GPS, GLONASS, Galileo, and QZSS.
- High Performance: Equipped with 33 tracking channels, 99 acquisition channels, and 210 PRN channels.
- XIAO Compatibility: Designed for seamless integration with the XIAO main controller.
- Flexible Connectivity: Apart from the connection with XIAO, it also provides pads like VCC, GND for broader applications.

## Specification

- GNSS Type: L76-L
- Supported Satellite Systems: GPS, GLONASS, Galileo, and QZSS.
- Connection Port: Tailored for XIAO.
- Connection Port for XIAO: D2/D3(TX/RX)
- Additional Pads: VCC, GND, TX, RX

## Applications

- Mobile Applications: Provide precise geolocation capabilities for your mobile apps.
- Tracking Devices: Design and build location and tracking devices.
- Geolocation Features: Add geolocation capabilities to your projects.


## Getting Started


Welcome to the quick start guide for the L76-L GNSS for XIAO. This guide aims to help you set up and get started with your new GPS expansion board in conjunction with the XIAO ESP32C3 main controller.

### 1. Soldering the Headers:
Upon receiving your product, some soldering will be required. We've provided two pin headers with the package. You'll need to solder these headers onto the expansion board. 

### 2. Connecting the Expansion Board:
Once the soldering is complete, you can proceed to connect the expansion board to the XIAO ESP32C3 main controller.

### 3. Connecting to Your Computer:
For programming the XIAO ESP32C3, you'll need a TYPE-C USB data cable. Connect one end to the XIAO ESP32C3 and the other to your computer. For a detailed guide on programming the XIAO ESP32C3, please refer to this [documentation](https://wiki.seeedstudio.com/XIAO_ESP32C3_Getting_Started/).

### 4. Install the software serial library
If you are using the XIAO ESP32C3 or XIAO ESP32S3, you will first need to install a software serial library. You can click [here](https://github.com/Longan-Labs/XIAO_GPS/raw/main/espsoftwareserial-main.zip) to obtain the library.


### 5. Get the RAW data

The L76-L module outputs GPS information via the serial port every 1 second. In this example, we print the content received from the serial port. You will be able to see a lot of information, including time, satellites, as well as latitude and longitude. Here's the code:

```Arduino

#include <SoftwareSerial.h>

SoftwareSerial gps(D3, D2); // RX, TX

void setup()
{
    gps.begin(9600);    // init the Serial1
    Serial.begin(9600);
}

void loop()
{
    while(gps.available())Serial.write(gps.read());
}
```

Please note that you might need to go outdoors for the GPS module to work properly.

### 6. Working with TinyGPSPlus Library

In the example above, we were only able to view the raw data output from the GPS. To better retrieve and understand the data, we can utilize the tinyGPSPlus library. The link to this library is [here](https://github.com/Longan-Labs/TinyGPSPlus_XIAO_GPS). With this library, we can obtain more detailed and clear information. After installing the library mentioned above, upload the following code to your XIAO ESP32C3:

```Arduino
#include <TinyGPSPlus.h>
#include <SoftwareSerial.h>

static const int RXPin = D3, TXPin = D2;
static const uint32_t GPSBaud = 9600;

// The TinyGPSPlus object
TinyGPSPlus gps;

// The serial connection to the GPS device
SoftwareSerial ss(RXPin, TXPin);

void setup()
{
    Serial.begin(115200);
    ss.begin(GPSBaud);

    Serial.println(F("DeviceExample.ino"));
    Serial.println(F("A simple demonstration of TinyGPSPlus with an attached GPS module"));
    Serial.print(F("Testing TinyGPSPlus library v. ")); Serial.println(TinyGPSPlus::libraryVersion());
    Serial.println(F("by Mikal Hart"));
    Serial.println();
}

void loop()
{
    // This sketch displays information every time a new sentence is correctly encoded.
    while (ss.available() > 0)
    if (gps.encode(ss.read()))
    displayInfo();

    if (millis() > 5000 && gps.charsProcessed() < 10)
    {
        Serial.println(F("No GPS detected: check wiring."));
        while(true);
    }
}

void displayInfo()
{
    Serial.print(F("Location: "));
    if (gps.location.isValid())
    {
        Serial.print(gps.location.lat(), 6);
        Serial.print(F(","));
        Serial.print(gps.location.lng(), 6);
    }
    else
    {
        Serial.print(F("INVALID"));
    }

    Serial.print(F("  Date/Time: "));
    if (gps.date.isValid())
    {
        Serial.print(gps.date.month());
        Serial.print(F("/"));
        Serial.print(gps.date.day());
        Serial.print(F("/"));
        Serial.print(gps.date.year());
    }
    else
    {
        Serial.print(F("INVALID"));
    }

    Serial.print(F(" "));
    if (gps.time.isValid())
    {
        if (gps.time.hour() < 10) Serial.print(F("0"));
        Serial.print(gps.time.hour());
        Serial.print(F(":"));
        if (gps.time.minute() < 10) Serial.print(F("0"));
        Serial.print(gps.time.minute());
        Serial.print(F(":"));
        if (gps.time.second() < 10) Serial.print(F("0"));
        Serial.print(gps.time.second());
        Serial.print(F("."));
        if (gps.time.centisecond() < 10) Serial.print(F("0"));
        Serial.print(gps.time.centisecond());
    }
    else
    {
        Serial.print(F("INVALID"));
    }

    Serial.println();
}
```

// 黎老板帮忙截个图，放在这里

## Work without XIAO

If you wish to utilize the GPS module with other microcontrollers, they can make use of the four solder pads available on the circuit board: 3V, GND, TX, and RX.

By connecting these pads to the respective pins on the desired microcontroller, the L76-L module can be integrated and operated without the XIAO. Ensure to refer to the specific microcontroller's documentation for proper pin configurations and connections.

|L76-L module|Others MCU|
|------------|----------|
|3V|3.3V|
|GND|GND|
|TX|RX|
|RX|TX|

## Resources

- **[Zip]** [Eagle file](https://github.com/Longan-Labs/XIAO_GPS/raw/main/XIAO_GPS.zip)
- **[PDF]** [Datasheet - L76-L](https://github.com/Longan-Labs/XIAO_GPS/raw/main/L76-L.zip)

## Tech Support
If you have any technical issue.  submit the issue into our [forum](https://forum.seeedstudio.com/).
