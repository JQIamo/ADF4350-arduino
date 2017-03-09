# ADF4350

Communication library to interface with Analog Devices ADF4350 PLL IC. Designed
with the ADF4350 evaluation board in mind.

## Getting started

In your Arduino sketch, you'll want to include the SPI library in addition to this code:

    #include <SPI.h>
    #include <ADF4350.h>

    #define COM_PIN 32 // sets pin 32 to be the slave-select pin for PLL
    ADF4350 PLL(COM_PIN); // declares object PLL of type ADF4350. Will initialize it below.

    void setup(){
        SPI.begin();          // for communicating with DDS/PLLs
        SPI.setClockDivider(4);
        SPI.setDataMode(SPI_MODE0);
        delay(500); // give it a sec to warm up


        PLL.initialize(400, 10); // initialize the PLL to output 400 Mhz, using an
                                // onboard reference of 10Mhz

    }


### Important note

The ADF4350 works with 3.3V logic levels, not 5V. Be careful if you're using an Arduino Uno or similar!

## Implemented features

Self-explanatory functions...

* `ADF4350::initialize(int frequency, int refClk)` -- initializes PLL with given frequency (Mhz) and reference clock frequency (also in Mhz).
* `ADF4350::getFreq()` -- returns current frequency
* `ADF4350::setFreq(int freq)` -- sets PLL to output new frequency `freq` (in MHz).

Functions you should use after consulting datasheet:

* `ADF4350::setFeedbackType(bool feedback)` -- fundamental vs. divided feedback
* `ADF4350::powerDown(bool pd)` -- power down the VCO (or not).
* `ADF4350::rfEnable(bool rf)` -- enable/disable output on the main RF output.
* `ADF4350::setRfPower(int pow)` -- `pow` should be 0, 1, 2, or 3, corresponding to -4, -1, 3, or 5 dBm.
* `ADF4350::auxEnable(bool aux)` -- enable/disable output on the auxilary output.
* `ADF4350::setAuxPower(int pow)` -- set auxiliary power output. Again, `pow` should be 0, 1, 2, or 3, corresponding to -4, -1, 3, or 5 dBm. 
