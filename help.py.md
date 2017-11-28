### Examples
https://github.com/brendan-w/python-OBD


```py
import obd

connection = obd.OBD() # auto-connects to USB or RF port

cmd = obd.commands.SPEED # select an OBD command (sensor)

response = connection.query(cmd) # send the command, and parse the response

print(response.value) # returns unit-bearing values thanks to Pint
print(response.value.to("mph")) # user-friendly unit conversions
```

**OR**
```py
import obd

obd.debug.console = True

connection = obd.OBD("/dev/tty.OBDII-Port")

cmd = obd.commands.RPM

response = connection.query(cmd)

print(response.value)
print(response.unit)
```




## To output the /dev directory
`echo $(ls /dev/tty*) >> system.txt`


---
From http://blog.brianhemeryck.me/how-to-interface-with-your-cars-ecu-through-obd2-and-python/
For anyone who would like to interface with their car, here is a guide to do so.


For this you need a few things:

A car supporting OBD2 (most cars since 1996 have this).
A laptop/computer running Python 2.7.3
An OBD connector (available online – I have an ELM327 bluetooth one which cost under €10)
So now that you have those, lets get started:

Connecting to the OBD2 connector:
Whether you are using a Bluetooth or USB OBD2 connection, it is a serial connection. To talk to it through python you can use pyserial.

You can list all serial ports on OSX or Linux by typing the following in the terminal:

ls /dev/tty*

(“ls” above is short for “list the following with file name” and the “/dev/tty” is the start of the file path of any serial port).

Take note of the serial device you are using – you will need it below.

- Download pyserial and install on your machine.
- Open the terminal (command prompt on Windows).
- Type `python` to start Python.
- Type `import serial` to make use of Pyserial
- Now connect to the serial device. To do this you will need the name of the serial device you are - using (see above). Type the following: `ser = serial.Serial('NAME_OF_SERIAL_DEVICE', 38400, - timeout=1)`
- ser = serial.Serial('/dev/tty.usbserial-A9CNJXDL', 38400, - timeout=1)
- Now you’ve connected to the OBD so you can start sending commands. For example, to measure speed - type: `ser.write("01 0D \r")` A full list of OBD commands can be obtained here.
- The elm327 device returns values in HEX. To read the value you just requested in Python type - `speed_hex = ser.readline().split(' ')`
- Convert the HEX to decimal by using: `speed = float(int('0x'+speed_hex[3], 0 ))`
- Finally output to the screen: `print 'Speed: ', speed, 'km/h'`
- Now you should see the speed of your vehicle appear on screen.

Any issues/questions shout below in the comments and I will try to help out!

My next project is to get all of this onto a Raspberry Pi so I can monitor the car’s ECU on the go.

---
```csv
2F  47  1   Fuel Tank Level Input   0   100  %  {\displaystyle {\tfrac {100}{255}}A} {\displaystyle {\tfrac {100}{255}}A}
```