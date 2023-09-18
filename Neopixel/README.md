# Neopixel

## HelloWorld 

In this we will see how to use WS2812 LED aka Neopixels with the rpi pico microcontroller

*Neopixel are RGB or RGBW (W being White) LED with a tiny embedded microcontroller, they are addressable so you can light up one in one color and the second in another. It works by sending all the color data to the Data input (Din) of the microcontroller. The microcontroller will take the first 3 (or 4 when RGBW) bytes of data it receive and send the remaining to the next Neopixel through its Data output pin (Do or Dout). The color are coded with 8 bits each, with a total of 24 bits by pixel and a speed of 800kbps, if your controller has enough memory you could address more then 30 000 LED with only one pin with a refresh rate of 1 refresh per second or a 1000 led with a 30 refresh per second*

What will we need :
- Tell the interpreter we need one pin as Output
- Prepare some memory to manage the Pixels
- Wait a bit between change

With thoose needs the impors will be 

```python
from machine import Pin
from time import sleep
from neopixel import NeoPixel
```

Now we need to declare how much pixel we have and on wich pin

```python
np = NeoPixel(Pin(28), 12)
```

Now we can turn on the first LED in red, the second in green, and the third in yellow

```python
np[0] = (255,0,0)   # it is RGB, the first represent the red, then blue, then green
np[1] = (0,255,0)   # The value are from 0 to 255, and always integers
np[2] = (127,127,0) # the LED can get really bright really fast so I tune off the values a bit
```

With the previous instructions we set the color data in our microcontroller's memory but now we need to send the datas to the Pixels

```python
np.write()
```

Nice but it is always better to extract constant from the running code (you will thank me later)

```python
from machine import Pin
from time import sleep
from neopixel import NeoPixel

PIXEL_PIN = Pin(28)
NB_PIXEL = 12

np = NeoPixel(PIXEL_PIN, NB_PIXEL)
np[0] = (255,0,0)
np[1] = (0,255,0)
np[2] = (127,127,0)
np.write()
```

Write that down in your main.py, STOP and START, and your three first LED should light up.

## Animation
### Basics 
We can now use this object and the sleep function to create animation

```python
from machine import Pin
from time import sleep
from neopixel import NeoPixel

PIXEL_PIN = Pin(28)
NB_PIXEL = 12

np = NeoPixel(PIXEL_PIN, NB_PIXEL)

while True:
    np[0] = (255,0,0)
    np[1] = (255,0,0)
    np[2] = (255,0,0)
    np[3] = (255,0,0)
    np[4] = (255,0,0)
    np[5] = (255,0,0)
    # ...
    np.write()
    sleep(0.5)
    np[0] = (0,255,0)
    np[1] = (0,255,0)
    np[2] = (0,255,0)
    np[3] = (0,255,0)
    np[4] = (0,255,0)
    np[5] = (0,255,0)
    # ...
    np.write()
    sleep(0.5)
    np[0] = (0,0,255)
    np[1] = (0,0,255)
    np[2] = (0,0,255)
    np[3] = (0,0,255)
    np[4] = (0,0,255)
    np[5] = (0,0,255)
    # ...
    np.write()
    sleep(0.5)
```

### Iterate over LED indexes

Well using numbers directly can be quite cumbersome, let's use a loop. We will use a `for <var> in <list of objects>:` loop with the function `range(x,y)`

- `for <var> in <list of objects>:` will iterate the block under it with `<var>` taking one by one the value in `<list of objects>`
- `range(x,y)` will create a list\* of all the numbers between `x` included and `y` excluded

```python
from machine import Pin
from time import sleep
from neopixel import NeoPixel

PIXEL_PIN = Pin(28)
NB_PIXEL = 12
PERIOD = 0.5
np = NeoPixel(PIXEL_PIN, NB_PIXEL)

while True:
    for i in range(0,NB_PIXEL):
        np[i] = (255,0,0)
    np.write()
    sleep(PERIOD)
    for i in range(0,NB_PIXEL):
        np[i] = (0,255,0)
    np.write()
    sleep(PERIOD)
    for i in range(0,NB_PIXEL):
        np[i] = (0,0,255)
    np.write()
    sleep(PERIOD)
```

Ways better, don't you think?
But they are all the same color, let try to light them one by one.

For that we will need to track the lit LED and turn it off when we turn on the next and use a modulo to never go over the 12th LED (number 11).

*The modulo is needed because if we try to set a color to an index that is out of bound (for us over 11) the code will throw an OutOfBound Error and crash the program*

```python
from machine import Pin
from time import sleep
from neopixel import NeoPixel

PIXEL_PIN = Pin(28)
NB_PIXEL = 12
PERIOD = 0.5
np = NeoPixel(PIXEL_PIN, NB_PIXEL)

tracker = 0
while True:
    # Turn off the previous LED
    np[tracker] = (0,0,0)
    # The tracker goes 0 1 2 3 4 5 6 7 8 9 10 11 0 1 2 3 4 5 6 7 ...
    tracker = (tracker + 1) % 12 
    # Turn on the next LED
    np[tracker] = (0,0,255)
    np.write()
    sleep(PERIOD)
```

With this code a blue dot should go accross your led indefenitly.

### Iterate over colors

You can also prepare some colors in variables to use them later

```python
from machine import Pin
from time import sleep
from neopixel import NeoPixel

PIXEL_PIN = Pin(28)
NB_PIXEL = 12
PERIOD = 0.5
np = NeoPixel(PIXEL_PIN, NB_PIXEL)

RED = (255,0,0)
GREEN = (0,255,0)
BLUE = (0,0,255)
YELLOW = (127,127,0)
CYAN = (0,127,127)
MAGENTA = (127,0,127)
OFF = (0,0,0)


tracker = 0
while True:
    # Turn off the previous LED
    np[tracker] = OFF
    # The tracker goes 0 1 2 3 4 5 6 7 8 9 10 11 0 1 2 3 4 5 6 7 ...
    tracker = (tracker + 1) % 12 
    # Turn on the next LED
    np[tracker] = BLUE
    np.write()
    sleep(PERIOD)
```

If we put our colors in an list, we can use a variable the same way we use `tracker` to get a new color each time 

```python
from machine import Pin
from time import sleep
from neopixel import NeoPixel

PIXEL_PIN = Pin(28)
NB_PIXEL = 12
PERIOD = 0.5
np = NeoPixel(PIXEL_PIN, NB_PIXEL)

RED = (255,0,0)
GREEN = (0,255,0)
BLUE = (0,0,255)
YELLOW = (127,127,0)
CYAN = (0,127,127)
MAGENTA = (127,0,127)
OFF = (0,0,0)

COLORS = [RED,GREEN,BLUE,YELLOW,CYAN,MAGENTA]

color_tracker = 0
tracker = 0
while True:
    np[tracker] = OFF
    tracker = (tracker + 1) % 12 
    # len is a function that return the lenght of the list you give it
    color_tracker = (color_tracker + 1) % len(COLORS)
    # We get the color at the color_tracker index
    np[tracker] = COLORS[color_tracker]
    np.write()
    sleep(PERIOD)
```

Now that you have the basics to do animations you can try yourself to do more crazy things

### Rainbows

Would it not be nice to be able to have very smooth colors. Well, we can use the chromatic disk, but for that we need ... MATHS !!!

Don't worry I already did the maths, you can just use them. In Thonny go to the top menu, in "View", check "Files" if it is not already checked. Two tabs will display on the left, the top one represents the files in your computer and the bottom one the files in your rpi. In the computer file tab navigate to this repository folder, go to the common folder, right click on color and choose "upload to /".

It will transfer the file to your rpi. Now we can use it in our main.py script.

As usual, we tell the interpreter we need something :

```python
from color import from_hsv
```
*The chromatic disk represent value with an angle, a lengh and an intensity, also know as hue, saturation and value -> HSV*

```python
from machine import Pin
from time import sleep
from neopixel import NeoPixel
from color import from_hsv

PIXEL_PIN = Pin(28)
NB_PIXEL = 12
PERIOD = 0.001
np = NeoPixel(PIXEL_PIN, NB_PIXEL)

color_tracker = 0
tracker = 0
while True:
    for angle in range(0,360):
        for i_led in range(0,NB_PIXEL):
            # from_hsv return a Color use in the color library, Neopixels need a tuple, the method as_tuple() convert the Color as a tuple of RGB
            np[i_led] = from_hsv(angle,1,0.2).as_tuple()
        np.write()
        sleep(PERIOD)
```

Now your LED should smoothly change color over time. 

With a little bit of code and maths we can create an angle offset between all LEDs, creating an ilusion of a moving rainbow.

```python
from machine import Pin
from time import sleep
from neopixel import NeoPixel
from color import from_hsv

PIXEL_PIN = Pin(28)
NB_PIXEL = 12
PERIOD = 0.001
np = NeoPixel(PIXEL_PIN, NB_PIXEL)

# We prepare the angle
ANGLE_BETWEEN_LED =  360 / NB_PIXEL

color_tracker = 0
tracker = 0
while True:
    for angle in range(0,360):
        for i_led in range(0,NB_PIXEL):
            # We add as much angle as the offset
            np[i_led] = from_hsv(angle + ANGLE_BETWEEN_LED * i_led, 1, 0.2).as_tuple()
        np.write()
        sleep(PERIOD)
```
Now you should have a rainbow going across your LEDs

You now have a few of tool to build animation with the led matrix

\* Technically it is not a list, it is a sequence, the number are not kept in memory but generated on the go each time you need them.
