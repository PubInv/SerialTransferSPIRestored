# TEMPORARY FORK OF POWERBROKER2's SERIALTRANSFER

USE THIS REPO WITH CAUTION!

This is a temporary fork of @PowerBroker2 's very nice SerialTransfer library. It pains me to fork it, but that project has decided not to support SPI, and the [General Purpose Alarm Device (GPAD)](https://github.com/PubInv/general-alarm-device) really needs SPI data transfer.

I considered creating a small SPI libary to do what we need, but @PowerBroker2's code is just too nice; I would end up duplicating a lot of the packet construction work that they have created.

I hope to keep this repo in shape to be reintegrated into that project as needed. I understand some of the things that neeed to be done to do that, but perhaps not all of them---entropy and bit rot are powerful forces.

# Changes From The Original

1. The SPI files are uncommented (they were commented out by the original author when they stopped supporting SPI)
1. I removed a "volatile" keyword which was preventing compilation. I think I understand the meaning of "volatile" and believe it was not needed in this case; but concurrency is complicated and I could be wrong.



# Begin Original Text


<p align="center">
  <img width="475" height="184" src="https://raw.githubusercontent.com/PowerBroker2/SerialTransfer/master/logo.png">
</p>

# SerialTransfer
[![GitHub version](https://badge.fury.io/gh/PowerBroker2%2FSerialTransfer.svg)](https://badge.fury.io/gh/PowerBroker2%2FSerialTransfer)  [![arduino-library-badge](https://www.ardu-badge.com/badge/SerialTransfer.svg?)](https://www.ardu-badge.com/SerialTransfer)<br /><br />
SerialTransfer is an easy to use Arduino library for transferring packetized data fast and reliably via **Serial, I2C, and SPI interfaces**. Interconnect your Arduinos today minus the headache with SerialTransfer!

[A mirror library is also available for Python!](https://github.com/PowerBroker2/pySerialTransfer)

This library:
- can be downloaded via the Arduino IDE's Libraries Manager (search "SerialTransfer.h")
- works with "software-serial", "software-I2C", and "software-SPI" libraries
- is non blocking
- uses packet delimiters
- uses consistent overhead byte stuffing
- uses CRC-8 (Polynomial 0x9B with lookup table)
- allows the use of dynamically sized packets (packets can have payload lengths anywhere from 1 to 254 bytes)
- supports user-specified callback functions
- **can transfer bytes, ints, floats, structs, even large files like JPEGs and CSVs!!**

# Packet Anatomy:
```
01111110 00000000 11111111 00000000 00000000 00000000 ... 00000000 10000001
|      | |      | |      | |      | |      | |      | | | |      | |______|__Stop byte
|      | |      | |      | |      | |      | |      | | | |______|___________8-bit CRC
|      | |      | |      | |      | |      | |      | |_|____________________Rest of payload
|      | |      | |      | |      | |      | |______|________________________2nd payload byte
|      | |      | |      | |      | |______|_________________________________1st payload byte
|      | |      | |      | |______|__________________________________________# of payload bytes
|      | |      | |______|___________________________________________________COBS Overhead byte
|      | |______|____________________________________________________________Packet ID (0 by default)
|______|_____________________________________________________________________Start byte (constant)
```

# ***How To Use:***

See the [example sketches](https://github.com/PowerBroker2/SerialTransfer/tree/master/examples)

- datum = tx/rx a single object
- data = tx/rx multiple objects

# ***NOTE:***

SPITransfer.h and it's associated features are not supported for the Arduino Nano 33 BLE or DUE and other boards. This header file is disabled by default, but can be enabled by commenting out `#define DISABLE_SPI_SERIALTRANSFER 1` within `SerialTransfer.h`.
