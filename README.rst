arduinoserial.py 
================

``arduinoserial.py`` is a Python port of Tod E. Kurt's
``arduino-serial.c`` program for communicating with an Arduino
microcontroller board over a serial port. It only uses standard Python
modules (notably ``termios`` and ``fcntl``) and does not require any
special serial communications modules.

Like Tod's program, you can use it from the command line.

Send the string "a5050" to Arduino::

$ ./arduino_serial.py -b 19200 -p /dev/tty.usbserial-A50018fz -s a5050

This would cause the pan-tilt head described in this previous post to
return to its middle position.

Recieve a line of text from Arduino, wait 1000 milliseconds, then send
the string "a0000"::

$ ./arduino_serial.py -b 19200 -p /dev/tty.usbserial-A50018fz -r -d 1000 -s a0000

Complete command line usage information::

Usage: arduino-serial.py -p <serialport> [OPTIONS]
Options:
  -h, --help                   Print this help message
  -p, --port=serialport        Serial port Arduino is on
  -b, --baud=baudrate          Baudrate (bps) of Arduino
  -s, --send=data              Send data to Arduino
  -r, --receive                Receive data from Arduino & print it out
  -n  --num=num                Send a number as a single byte
  -d  --delay=millis           Delay for specified milliseconds

Note: Order is important. Set '-b' before doing '-p'.
      Used to make series of actions:  '-d 2000 -s hello -d 100 -r'
      means 'wait 2secs, send 'hello', wait 100msec, get reply'
You can also import arduino_serial and use its SerialPort class to communicate with an Arduino from a Python program.

import arduino_serial

arduino = arduino_serial.SerialPort('/dev/ttyUSB0', 19200)
print arduino.read_until('\n')
arduino.write('a5050')
