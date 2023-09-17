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
