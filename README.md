# BeanBoard
## ASCII Keyboard and a simple LCD display for [BeanZee](https://github.com/PainfulDiodes/BeanZee)

This is a work in progress...  
Current status: Initial design thoughts, nothing prototyped yet.  

Schematic: [BeanBoard.pdf](/kicad/BeanBoard.pdf)

## Keyboard  
Aiming for the simplest possible design, using a simple matrix of switches. Diodes are used with each switch to prevent "[ghosting](https://en.wikipedia.org/wiki/Key_rollover#Key_jamming_and_ghosting)".  

The circuit expects the CPU to send a "strobe" - output to a defined port. A latch is used to capture the strobe - so that the state persists after the CPU has stopped outputting to the port. This strobe should have only one bit set high. This is the "live" row.

Then the CPU will read from a defined port to sense which columns are high - activated by switches being pressed. A buffer is used to gate the rows onto the data bus.

The CPU will loop back and successively set each strobe bit high. Having gone through each bit and then read back the columns, we will have 64 bits of data (8x8) representing which keys are being pressed.

## LCD
The [Hitachi HD44780 LCD controller](https://en.wikipedia.org/wiki/Hitachi_HD44780_LCD_controller) has been around since the 1980's and is still popular to control character-based LCD displays:  
[HD44780 datasheet](https://cdn-shop.adafruit.com/datasheets/HD44780.pdf)   
[Adafruit Standard LCD 20x4](https://www.adafruit.com/product/198)  
A parallel interface makes it CPU bus friendly - with 8 data bits, enable, R/W and register select inputs.

## Port address decoding

A couple of options were considered: [PortDecoding.pdf](/kicad/PortDecoding/PortDecoding.pdf), and the simpler is selected.

A 3 to 8 decoder has been used to select pairs of ports. A0 is then used to chose between ports in a pair. The BeanZee board has the logic to separate ports based on A0, and there's a similar arrangement here for LCD. The keyboard ignores A0.

