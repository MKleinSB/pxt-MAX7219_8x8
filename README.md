# Calliope mini MakeCode editor extension for MAX7219 8x8 matrix LED modules

This extension works with single or multiple MAX7219 8x8 LED matrix display modules.

To import this extension, go to Advanced -> +extension and enter "MAX7219" in the search box, 
or copy/paste [https://github.com/MKleinSB/pxt-MAX7219_8x8](https://github.com/MKleinSB/pxt-MAX7219_8x8). Press enter and click the extension.



## Modules Wiring

For the module at the head of the chain, connect it to Calliope as follows:

* VCC -> 3.3V 
* GND -> GND
* DIN (MOSI or MO in SPI) -> for excample C17
* CS (LOAD pin) -> for excample C16
* CLK (SCK in SPI) -> for excample P3

MISO or MI is not used, but included anyway for SPI pins are reassigned together.

Of course, you can reassign these SPI pins in anyway you want; just use the setup block and remember to set the correct number of matrixs.

To Wire you can use normal JumperWires on the Caliope mini Pinout like this:
![img_0003](https://github.com/r00b1nh00d/pxt-MAX7219/blob/master/Calliope_JumperWire.jpg)
![img_0003](https://github.com/r00b1nh00d/pxt-MAX7219/blob/master/MAX_JumperWire.jpg)

Or you can use the Grove to female Pin connector with an aditional croco clamp without soldering anything
![img_0003](https://github.com/r00b1nh00d/pxt-MAX7219/blob/master/Calliope_GroveWire.jpg)
![img_0003](https://github.com/r00b1nh00d/pxt-MAX7219/blob/master/MAX_GroveWire2.jpg)

The rest of the modules (if any) are connected basicly the same as the first one, except all module's DIN connects to DOUT on the previous one.



## Index of Modules

This extension assumes that you arrange single MAX7219 modules as a "chain", that they linked into a larger horizonal LED display.

For linked individual matrix modules, the one directly connected to micro:bit has the highest index number (total number - 1), and the furthest one (far right) has index of 0. You can use the index number to print something onto a specific module.

![img_0004](https://user-images.githubusercontent.com/44191076/50699988-5e941000-1084-11e9-841e-5ff173872540.JPG)

## Matrix Rotation/Reverse Printing Order

Some people use the 4-in-1 MAX7219 modules, which are 4 matrixs already linked together, but wired in different directions and/or order.

![max7219-dot - main-500x500](https://user-images.githubusercontent.com/44191076/53904356-d2e93080-4080-11e9-96bd-c1c3e5111a4b.jpg)

You can choose to rotate and reverse the display by using the following block in the advanced section:

![1](https://github.com/r00b1nh00d/pxt-MAX7219/blob/master/MakecodeScreenshot2.PNG)

Warning: the extra rotation/reverse calculating process slow down the text scrolling/refreshing speed. You may have to reduce the scrolling delay time to 0.

## Text Scrolling

There are currently two built-in display modes; first is the simple text printing mode, which use the whole LED display to show or scroll a string.

![makecode-screenshot](https://github.com/r00b1nh00d/pxt-MAX7219/blob/master/MakecodeScreenshot1.PNG)

```blocks
MAX7219_Matrix.setup(
4,
DigitalPin.C16,
DigitalPin.C17,
DigitalPin.P0,
DigitalPin.P3
)
basic.forever(function () {
    MAX7219_Matrix.scrollText(
    "Hello world!",
    75,
    500
    )
})
```

![img_0002](https://user-images.githubusercontent.com/44191076/50700052-88e5cd80-1084-11e9-843f-2fa339c39b6f.JPG)

The scrolling text block shows the whole sentence by scrolling it from right to left, speed adjustable. However, the program will not contiune to do anything until scrolling is finished.

Be noted that if you put in a very very long string, the micro:bit may run out of memory and show the error code of 20.

## Text Printing

Next, the display text block prints words on the LED display without (perceivable) delays, and you can choose offset (the starting point along the LED display, from -8 to the end of line). This is more sutiable for very short texts or characters, and you can print them seperatly along the LED display. (You will have to set the clear screen option to false.)

You can also print a custom character or image on the LED display. Use [this 8x8 LED generator](http://robojax.com/learn/arduino/8x8LED/) (right side is "up") and copy the byte array as input string. Paste the text in the block you find in "more" section; it would transform the text into a number array which can be used to print special characters.
Der läuft bei mir leider nicht.

![3](https://user-images.githubusercontent.com/44191076/50700687-2261af00-1086-11e9-8451-aff7c771dc64.jpg)

![MakeCode-screenshot 3](https://github.com/r00b1nh00d/pxt-max7219/blob/master/MakecodeScreenshot3.PNG)
Each line get his own Custom array like "B11111111" if you use more than one 8x8 Matrix use an offset like 8 to print it on an other module
```blocks
let ChstomChr: number[] = []
MAX7219_Matrix.setup(
4,
DigitalPin.C16,
DigitalPin.C17,
DigitalPin.P0,
DigitalPin.P3
)
ChstomChr = MAX7219_Matrix.getCustomCharacterArray(
"B00100000,B01000000,B10000110,B10000000,B10000000,B10000110,B01000000,B00100000"
)
MAX7219_Matrix.displayCustomCharacter(
ChstomChr,
0,
true
)
```

![img_0001](https://user-images.githubusercontent.com/44191076/50700621-ff36ff80-1085-11e9-942d-0ef1c3cef84f.JPG)

So far the extension only contains a simple ASCII character library. If you input a character that's not in the library, it will not be displayed (skipped) anyway. However, you can add custom character or images into the library by using some unusual [Unicode characters](https://en.wikipedia.org/wiki/List_of_Unicode_characters) as index/tokens.

There's a block that displays all the characters in the library.


## Known Issue Combining With Bluetooth Extension

If you use Bluetooth extension along with this extension, you would get error code 20 (out of memory) on your micro:bit. It's probably because the BLuetooth extension use a lot of memory and there's not enough RAM to run both.

## License

MIT

## Als Erweiterung verwenden

Dieses Repository kann als **Erweiterung** in MakeCode hinzugefügt werden.

* öffne [https://makecode.calliope.cc/](https://makecode.calliope.cc/)
* klicke auf **Neues Projekt**
* klicke auf **Erweiterungen** unter dem Zahnrad-Menü
* nach **MKleinSB/pxt-MAX7219_8x8** suchen und importieren


#### Metadaten (verwendet für Suche, Rendering)

* for PXT/calliopemini
<script src="https://makecode.com/gh-pages-embed.js"></script>