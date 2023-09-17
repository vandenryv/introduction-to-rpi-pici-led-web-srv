# introduction-to-rpi-pici-led-web-srv
Introduction to rpi pi pico Webserver controlling neopixel using GurgleWebServer


# Instruction

## Step 1 - Flash the Raspberry pi pico with microPython firmware

The rpi pico is designed to be easily flashable with differente type of firmware, even your own. Here we will be using the last version of microPython.

- First download the latest version of the microPython firmware for raspberry pi pico W  : [HERE](https://micropython.org/download/RPI_PICO_W/) (the W part is important here otherwise you will not be able to use wifi) You should get a **.uf2** file.
- Prepare your raspberry pi pico and your microUsb cable by pluging **ONLY** the USB-A(The big rectangular part) part of your cable in your computer.
- *The Pico is flashable with the 'BOOTSEL' button. If the Pico is powered on while this button is pushed the Pico will simulate a USB drive, your computer will see it as such allowing you to simply drag-and-drop (or just copy) the firmware into it.* Be aware that this operation will erase everything in the pi memory. Instruction : While pushing on the rpi pico 'BOOTSEL' button insert the other end of the cable in it.
- Your computer should react and show you a new drive containing two files, just ignore them and copy your .uf2 file in it. It should disconnect automatically.

Well done you just successfully flash a rpi pico with a micropython firmware.

## Step 2 - Install and configure Thonny for microPyhton

Thonny is a simple python IDE that integrate microPython and microcontroller interface.

- Download the last version of Thonny for your os. Which you can find [here](https://thonny.org/).
- Install it, instructions vary depending on your OS.
- Launch it, go to Tools -> Options.
- In the Interpreter tab under "Which kind of interpreter ..." choose "MicroPython (Paspberry Pi Pico)".
- Click OK.

That's it. Thonny is ready to read and write your code to your rpi pico.

## Step 3 - Hello World

A necessary step to check that your installation works is to make the rpi pico say Hello World : We will do it 2 ways, the Dev way and the Maker way.

### The Dev way :
We will simply make the rpi pico execute a python command to print "Hello World" to its serial output.

*The serial output of the rpi is connected to the serial input of the virtual\* serial interface of your computer so we will see it*.

In your brand new configured Thonny click on the red STOP button, the STOP button stops the current operation and restart the connection. You should now have a Shell tab on the bottom side of Thonny in it write (or copy) after the `>>>`

```python
print('Hello World') 
```

and then press Enter, the Raspberry should answer with "Hello World".

Good job, the Rpi just obey your command.

### The Maker way

A microcontroller is made to interact with the world so one of the best way to make it Hello World is to make it do something elsewhere than on our screen. We will make it blink a LED. How convinient there is a small LED on the rpi.

To make a LED blink we need a few more step :
- Declare the pin where the led is connected as binary output <- Because we will send highs and lows to it
*Usually we address pins as numbers but the LED Pin on the rpi pico W is special so it is addressed as "PIN", shocking I know*.
- Send the led a 1 to turn it on.
- Wait a bit.
- Send the led a 0 to turn it off.

*Again in the rpi pico the LED Pin is sPeCiAl\*\* so we will send .on() and .off() command instead.*
- Repeat the last 3 commands.

Now to code the blinking you can use the untitled file in front of you :
- First we need to import a few things from the micropython firmware. To interact with pins we need the `Pin` object, you will find it in the `machine` package. And to wait a bit between the different states we will need the `sleep()` function from the `time` package

```python
from machine import Pin
from time import sleep
``` 

- Then we declare than we want control of the "LED" pin as an Output.

```python
led = Pin("LED", Pin.OUT)
```

- Now that we have our led object that control the led on the pi we can turn it on.

```python
led.on()
```
Now you should have something like this.

```python
from machine import Pin
from time import sleep
led = Pin("LED", Pin.OUT)
led.on()
```

With that in your editor click on the green Play button, it will send the code to your pi and command it to run the code. The LED should light up.

__"But the LED doesn't blink it, just stays on"__ you might say. Yes now we can add a loop with the `while <condition>:` keyword with `True` as condition, effectively creating a infinite loop.

*For the non-dev people, python creates blocks with lines that follow each other and have the same indentation.*

In the loop we turn the LED on, wait 1 second, turn it off, and wait 1 more second.

```python
from machine import Pin
from time import sleep
led = Pin("LED", Pin.OUT)
while True:
    led.on()
    sleep(1)
    led.off()
    sleep(1)
```

*If you forgot the second `sleep(1)` you might think that the LED never turns off, but it does, the next instruction is to turn it on and it is so fast that you will never have the time to see it turn off.*

Now if you press Play, your LED should turn on and off each second. If you want it to go faster you can send decimal value to sleep or import and use sleep_ms for millisecond or sleep_us for micro second.

Good job, the Rpi now blink its LED.

### They grow so fast.

Last step of the "Hello World", we will make the rpi do the blinking without the computer sending it the script. 

The micropython firmware allows you to save file to rpi, if you save a script with the name "main.py" it will automatically start it when powered on.

From the screen with your script in the untitled file tab:
- Press Stop to stop the process and reconnect the pi.
- Press the Save button.
- Choose "Raspberry Pi Pico".
- Give "main.py" as name for the file.
- Press Ok.

Your script is now saved as main.py at the root of the filesystem of the pi. You can unplug it from your computer and plug it ANYWHERE and the script will run and the LED will start blinking.

**HELLO WORLD of electronics and microcontrollers.**


\* Yeah!! it is virtual, the rpi has full control of the way it speaks to your computer, remember just a bit ago it was seen as a USB drive ;) It can change itself to a USBDrive, a serial interface, a keyboard, a mouse, a gamepad, a camera, and just anything you want as long as you have the code for it.

\*\* In the non wifi rpi pico the LED is on the PIN 25, we would turn it on and off by puting its pin high with a 1 or low with a 0. But because the pin 25 is now used for wifi communication the LED on the rpi pico W is controlled by the wifi chip. Hence the weird "LED" address and the use of commands to turn it on and off (you see it as a simple .on() or .off() but the microPython interpreter in the rpi will send commands to the wifi chip to turn on and off the LED).