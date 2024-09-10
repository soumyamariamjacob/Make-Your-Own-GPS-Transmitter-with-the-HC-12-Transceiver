# Make-Your-Own-GPS-Transmitter-with-the-HC-12-Transceiver
Create a tracking device with an HC-12 transceiver, a GPS module, an Arduino, and Google Maps.

The first article in this two-part series, Understanding and Implementing the HC-12 Wireless Transceiver Module, uses the HC-12 to create long-distance data transmission between two Arduino Unos. This article uses a pair of HC-12 transceivers, a GPS module, an Arduino, and Google Maps to create a very simple tracking device

HC-12 transceiver (x2)-1
GPS Receiver-1
or Adafruit GPS Logger Shield-1
Arduino Uno R3 (or compatible)-1
This two-part series discussed the HC-12 transceiver module and described how to hook it up to an Arduino and a power source. 

In this article, we will create a remote GPS receiver that can be used to track nearby items without using a cellular network.

Adding and Transmitting GPS

The Global Positioning System (GPS) allows users to accurately determine the location of objects on or above the surface of the Earth. Both of the GPS receivers listed at the top of this article transmit National Marine Electronics Association (NMEA) sentences that provide information that includes latitude and longitude, altitude, time, bearing, speed, and a great many other variables at 9600 baud.

The HC-12s can transmit the information from a GPS receiver with no additional programming or circuitry.

You can transmit GPS coordinates to remote locations with as little as a GPS receiver, an HC-12 transceiver, and a battery. Remotely transmitted coordinates would have to be received by another HC-12 transceiver and then processed with a microcontroller or computer.

RMC (recommended minimum information), GGA (3D location and accuracy), and GLL (latitude and longitude) all include latitude, longitude, and time. GGA provides altitude, and GSA provides the dilution of precision of the reading (lower numbers indicate greater precision).

The following program is backward-compatible with the two programs presented in part one. It reads the NMEA sentences sent by the GPS to the Arduino, discards all but the selected sentence, and transmits the selected sentence to a remote Arduino when requested. It works with either the SparkFun GPS receiver or the Adafruit GPS logger shield, as shown below. This program allows users to remotely "ping" distant transceivers to determine their location.

GPS data from a remote transmitter received, via a pair of HC-12 transceivers, by a local Arduino

HC-12 transceiver paired with an Adafruit GPS shield

HC-12 transceiver paired with a SparkFun GPS module

Connect the power supply, GPS, Arduino, and HC-12 as shown above.

The SparkFun GPS receiver has only three wires; the fourth signal (GPS RXD) is not needed for basic functionality and is not made available to the user. However, if you use the Adafruit shield, the GPS RX pin is enabled, and you can change the refresh rate and which sentences the GPS transmits, eliminating the need for the portion of the Arduino code at the end of the file that simply deletes unwanted sentences.

One potential problem with using the Arduino UNO for this program is that the SoftwareSerial library can only "listen" to one serial port at a time—data sent to a serial port when the software isn't "listening" on that port will be discarded. The program functions as intended during testing, but I would not consider it a robust solution. If you need multiple serial communication ports in your project, consider the Arduino Mega, or a separate chip such as the ATSAMD21.

If you are tracking a single object near your house, it is sufficient to set upper and lower limits for expected latitude and longitude values. If you are trying to determine the distance between two GPS units, you might consider implementing Vincenty's formula on a 16-bit or 32-bit microcontroller. 
