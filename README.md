# RISC-V-NeoPixel
SPI Based NeoPixel Driver for CH32Vxxx MCUs

I am making a separate repo for this project because it should work with most (if not all) CH32V MCUs..
My intention is to have a C++ version implemented as a Library, followed by a similar 'C' version.

for right now I only have a demo project as a ".zip" file which is a full workspace for a CH32V203-C8T6

Credit goes to [Controllerstech](https://controllerstech.com/ws2812-leds-using-spi/) who inspired me with his SPI impleentation for the STM32.  It uses some "magic" to develop the 3 bit "tribits" required to feed data to the WS2816.  This seems to work quite well.  It is my intention to expand to, at least, 16bit SPI so I can get more efficient 5 x "tribit" transfers...  Or even more using 24bit I2S (?? 72 bit ??).  The basic process of feeding a strip involves sending streams of 24 "tribits" per LED followed by a 100 uSec pause with the data line low to indicate a new stream.  Actual "tribits" are a '6' for a one and a '4' for a zero.  The WS2816s "remember" the last data (24 tribits) they received.  The 24 reflects RGB colour but most actualy use a GRB perversion thereof.

### Considerations when interfacing CH32Vxxx to the WS2816 based NEOpixel arrays or strips.

* as with any MCU you need to pay attention to the current draw on the 5V power lines of the strips.  The more LEDs, the higher the current, especially if you turn most of the LEDs on.  <b>Not Recommended</b> to power a strip from the +5V of the W-Link.  Arrangement should be made for a separate 5V supply.  Figure on at least 60 mA per LED.
* Driving the data line - The data line needs to be 5V logic.  The CH32V003 and 103 will run happily at 5V whereas 203 and above MCUs are 3V3 only.  That said, current draw is also a factor.  The data line for a typical strip needs around 50 mA +/- 10mA.  This is more than a single GPIO of most MCUs can source.  In my prelim testing I used a 2N2222 transistor in an emitter follower config with a pot between the GPIO and the base of the transistor.  At first I had grabbed the nearest thing handy, a 20K pot, but this was very sensitive.  A 1K would probably be better - or a driver chip like maybe an LM358.  I am not sure that all level convertors source enough current.

As mention in my [RISC-V Overview repo](https://github.com/CanHobby/RISC-V) you can try this code by unzipping it into a separate directory and then using it as a Moun River Studio workspace.  I need to start somewhere so that is where I am at right now.

As mentioned in my <a href="https://github.com/CanHobby/freeRTOS" target=_blank>freeRTOS</a> Repo I am adding another .zip of a whole Workspace which has C++ freeRTOS with a "NOP" based NEOpixel driver.  The driver supports multiple NEOpixel strips, each on a different GPIO.  It is written using C++ Objects/classes where the strips are abstracted to "strips" and "segments".  Individual segments include animations to make things more interesting.  The present version of this driver/examples is very crude with a lot of commented out debug statements and very few useful comments..  I plan to remedy this shortly.


