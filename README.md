WSWireLib
=========

Arduino Wire Library modified to add timeouts to the freeze-prone TWI while() loops.  This also disables the arduino pullup resistors on SDA and SCL lines, contrary to the standard wire lib.

Timeout scheme lifted from this older post on arduino forums: http://www.arduino.cc/cgi-bin/yabb2/YaBB.pl?num=1283887406

The files the code was adapted from are here, and are not compatible with Arduino 1.0: 
- http://liken.otsoa.net/pub/ntwi/twi.h
- http://liken.otsoa.net/pub/ntwi/twi.c