# ppmto565
Convert images (ppm format) to rgb 565 (16 bit format) for use with many common LCD's

Usage: ppmto565 [options] [input]

Generates a .C source file of RGB 16-bit data, suitable for use with many LCD
displays.  Modification might be necessary if your MCU or API/libraries can't
make use of the unsigned int[] output.  It also is customized for my ESP8266
use, with the Arduino IDE, where the INFLASH define, which you'll see in its
output, places the image data in flash memory so as not to consume program
space.  Improvements are welcome (jaggz.h at gmail)

As its name states, it requires PPM input (magic P6 in the file); this means,
if you're converting a greyscale or bw image with, say, pngtopnm, the output
will likely be rejected by this program (because it'll probably be in pbm
or pgm format).  See the 'netpbm' package of utilities.

Options:
  -n name, --var=name  Output variable name for image data
                       Also defines name_w and name_h for width/height
                       Default: img
                       Results in an array img[] and img_w img_h defined
                       (They are not capitalized, but they are #defines)
  -f, --useflash       Export defines and variable for alignment and use
                       in PROGMEM (Arduino/ESP8266 use).
  -h, --help           This help

Examples:
  ppmto565 -n myimg someimage.ppm > someimage.cpp
     Outputs unsigned int myimg[] data, as well as defines
     myimg_w and myimg_h
  pngtopnm file.png | ppmto565 -f -n myimg someimage.ppm > someimage.cpp
     Same as above, but includes #defines, and modifies the declaration,
     for storage in the flash memory of the ESP8266.

Example steps:
  1. Run: ppmto565 -n myimg someimage.ppm > someimage.cpp
  2. Copy the top lines, from the #include to the //extern variable
     declaration for myimg[], into your project's .c/.cpp file.
  3. Remove // from the extern line (within your .c/.cpp file)
