This project is a simplified VGA driver for the pico(RP2040). I created this project as a response to my failure to get the example programs to build and/or run. The published examples I found were too complex so I decided to create this to show case the pio and C++ code needed to get a minimal driver working.

The pimornio uses 17 pins of the pico to generate the hsync/vsync/color signals of the VGA signal. 15 pins are allocated to the color (5 red, 5 green, 5 blue) (GPIO 5 is not used). The timing constants are hard coded in the C++ main function (picoVga.cpp).

My code is based on the excellent articles published by Hunter Adams (vha3@cornell.edu) https://vanhunteradams.com/Pico/VGA/VGA.html Here you can find description of the VGA protocol and the pio code needed to generate the HSync and VSync signals. The big difference with Hunter Adams code and the code needed for thr Pimoronio board is the number of bits needed per pixel.

This driver drives 15 bits per pixel for a Pimoroni pico Demo The Pimoroni example programs for the board can be found at https://github.com/pimoroni/pimoroni-pico/tree/main/micropython/examples

However I found these generic drivers to be overly complex and very difficult to read or understand. I struggled to find a very specific concrete example of an implementation. Failing to do so, I rolled up my sleaves and started reading and understanding the implementation details of the Pico. It is a brilliant piece of hardware and is capable of generating a VGA video signal with virtually no overhead on the CPU. My first step was to research and read the articles by Hunter Adams (vha3@cornell.edu) and his demo project https://vanhunteradams.com/Pico/VGA/VGA.html#Mandelbrot-Set

Great exaplanations and example pio code to drive the HSync, VSync and RGB signals of the VGA output. However this example was based on 4 bit per pixel interface where the Pimorni pico demo uses 16 bits per pixel. I spent many hours decoding the changes I made to handle 16 bits per pixel.

Gotcha
My driver ONLY works at 640x480 resulation pixels. The timing constants are hard coded into the code. and only supports 16 bits per pixel. Consult the Pimoroni schematic tp see the pinout
the PIO is limited to 5 pins to drive to 0 using a SET instruction, however the VGA standard requires all pins to be 0 during the fron/back porch times. I got around this limitation by using the OUT instruction instead and setting the OSR register to 0 with mov osr, null first.
my project is in VSCODE using the Pico plugin.
I have not tested this with the RP2350 yet but I will as soon as I get one.
Because the Pimonori uses 16 bits (2 bytes) per pixel, there is not enough RAM in the RP2400 to store a complete Frame buffer. So, my demo code only allocated enough RAM for a single scan line and this line is repeated 280 times per fram on the screen. Possibly the RP2350 has enought RAM for a complete frame buffer.
Well, I hope this project clears up your begining questions of this fantastic processor and its potential for video cheers dale (wdlyons@gmail.com)
