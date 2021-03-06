#DS18S20/DS18B20 sensor C library 

##Short Description

DS18S20library is an AVR C library for connecting DS18S20 and DS18B20 temperature sensors to 8-bit AVR microcontrollers.
The library implements 1-Wire interface for communication with DS18S20/DS18B20.
Originally it was written for DS18S20 and starting from version 0.4 the library fully supports
DS18B20 sensor model as well.


What's New

Version 0.5.1

- DS18x20_MeasureTemperature() has been extended with READ_SCRATCHPAD command so the temperature can be measured and read in one step;
- Minor update of DS18x20_Init().

##Supported MCU Hardware

The library is compatible with every 8-bit AVR microcontroller.

##DS18x20 Support and Implemented Functions

The following sensor models are supported by the library:

- DS18S20;
- DS18B20.

The library can be used only in cases when DS18x20 sensor is powered by external power supply.
Although the library includes a function that can determine the power supply type, this information is not taken in 
consideration when the library communicates with the sensor via the 1-Wire bus.

Current version supports 1-Wire bus with only one device attached to it.
SEARCH_ROM is not implemented in the library so it cannot query the bus for connected devices/sensors.

Implemented DS18x20 ROM Commands:

- DS18S20/DS18B20 sensor initialization;
- READ ROM command for fetching the ROM address of the sensor;
- SKIP ROM command so the library can speak to the sensor directly without using sensors 1-Wire bus address.

Implemented DS18x20 Function Commands:

- Convert T command that initiates a single temperature conversion;
- READ SCRATCHPAD command that allows the master to read the contents of the sensor memory;
- READ POWER SUPPLY command to determine power supply type of DS18S20/DS18B20;
- WRITE SCRATCHPAD command that allows the master to write 2 bytes of data to the scratchpad of DS18S20/DS18B20;
- COPY SCRATCHPAD command that copies the contents of the scratchpad TH and TL registers (bytes 2 and 3) to EEPROM;
- RECALL E2 recalls the alarm trigger values (TH and TL) from EEPROM of the sensor.

1-Wire interface

The underline implementation of 1-Wire interface relies on timer functions that are part of AVR-libc and defined in util/delay.h
header. Those functions require the CPU freq to be defined via F_CPU macro. This value can be set globally during the compiling time
using -DF_CPU=<value>L switch. If this macro is not defined, the library will assign a default value of 4MHz.

##Usage

The library use DS18x20_Init function to define the 1-Wire bus and initialize the sensor. The function detects the sensor
model (DS18B20 or DS18S20) during the initialization process.

For bus definition, the init function requires two arguments:

- the address of MCU PORT used for 1-Wire bus;
- MCU pin that is connected to the DQ pin of DS18S20/DS18B20.

The first argument of the init function is a pointer to the structure that represents the sensor. 

For example:

	TSDS18x20 DS18x20;
	TSDS18x20 *pDS18x20 = &DS18x20;

	DS18x20_Init(pDS18S20,&PORTD,PD5);

After the initialization step, the rest of the library functions/macros can be used to
get the address of the sensor, start a temperature conversion, define temperature resolution in case of DS18B20 sensor etc.

For example:

	DS18x20_MeasureTemperature(pDS18x20);

initiates a temperature conversion and fetches the sensor memory with the temperature reading. The only argument of the function is 
the same pointer as the one in DS18x20_Init() function.

See the documentation section for further details.


Tested on Atmega32 and Atiny2313 microcontrollers.

WARNING: 
The source is provided as it is without any warranty. Use it on your own risk!
The author does not take any responsibility for the damage caused while using this software.

