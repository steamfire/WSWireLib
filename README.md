WSWireLib
=========

This is a modification of the Arduino Wire Library, to add timeouts to the freeze-prone TWI while() loops.  It is compatible with Arduino 1.0.  After 100000 iterations of the while() loop it simply breaks the while loop and re-runs twi_init() to reinitialize the TWI perhipheral and I2C pins of the AVR.  It is probably not the best solution for this problem, but it does appear to work!

HARDWARE I2C BUS NOTE: This library also disables the arduino pullup resistors on SDA and SCL lines, contrary to the standard wire lib, which enables the pullups.  You will need external pullup resistors, or another device that has pullups enabled, or just change the two lines in twi.h that contain digitalWrite commands.

Installation
----

The WSWire folder should be installed just like any other 3rd party arduino library, in the "libraries" folderof your Arduino sketchbook directory ( the folder where arduino creates new sketches by default).  This folder will need to be created if you haven't done so for another 3rd party library already.  There is no need to change or discard the Arduino Wire library that comes with arduino.

Using it in your sketch
----

Include the correct library - Wherever you would normally have put this:

     #include <Wire.h>
     
Just change that to:

     #include <WSWire.h>
    
That's all!  You still call the Wire functions by the same name in your sketches.  For example, this works for both the OEM Wire lib and the WSWire lib:

    sentStatus = Wire.endTransmission();

Additional Returned Error Codes
----
The wire library function endTransmission normally will output integer error codes when it fails to send data.  Two new values are returned, to better facilitate your debugging, numbers 5 and 6.  All error codes are shown below:

    * Output   0 .. success
    *          1 .. length to long for buffer
    *          2 .. address send, NACK received
    *          3 .. data send, NACK received
    *          4 .. other twi error (lost bus arbitration, bus error, ..)
    *          5 .. timed out while trying to become Bus Master
    *          6 .. timed out while waiting for data to be sent
    
While there are no returned error codes for the slave receive data functions in the original Wire lib, nor this version, the receive functions use while() loops, and they have also been modified to use the same timeout mechanism.  You won't be able to tell when they timeout at the moment, but once the AVR I2C system reinitializes that data will likely just get retried by whatever master was trying to send it.

Origin
----
This was written by @steamfire for the [White Star Balloon](http://whitestarballoon.org) high altitude balloon payload, which could not fly with multiple flight-critical the arduinos communicating over I2C freezing up every few hours when there was heavy I2C bus traffic.

Timeout scheme lifted verbatim from this older post on arduino forums: http://www.arduino.cc/cgi-bin/yabb2/YaBB.pl?num=1283887406  Updated for Arduino 1.0 and error number 6 added by @steamfire.

The files the code was adapted from are located here, and are not compatible with Arduino 1.0: 
- http://liken.otsoa.net/pub/ntwi/twi.h
- http://liken.otsoa.net/pub/ntwi/twi.c