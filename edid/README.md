Writing eeprom with Raspberry Pi  
(tested with an rpi 3B)  

add following line to /boot/config.txt (do NOT put it in the [pi4] section) 
`dtparam=i2c2_iknowwhatimdoing`  
  
this will enable i2c2 on which the hdmi eeprom is connected  
also enable i2c support with the raspi-config tool  
reboot   
  
verify if eeprom is detected by executing
`i2cdetect -y 2`    

this will give an output like this
```
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- -- -- -- -- -- -- -- --
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
50: 50 51 52 53 54 55 56 57 -- -- -- -- -- -- -- --
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
70: -- -- -- -- -- -- -- --
```
address 0x50 is the eeprom we want to manipulate !  


Dumping the eeprom data can be done by executing  
`i2cdump 2 0x50 b`
this will give an output like 
```
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f    0123456789abcdef
00: 00 ff ff ff ff ff ff 00 04 81 04 00 01 00 00 00    ........???.?...
10: 01 11 01 03 80 0f 0a 00 0a 00 00 00 00 00 00 00    ???????.?.......
20: 00 00 00 00 00 00 01 01 01 01 01 01 01 01 01 01    ......??????????
30: 01 01 01 01 01 01 c4 09 20 80 30 e0 2d 10 28 30    ???????? ?0?-?(0
40: d3 00 6c 44 00 00 00 18 00 00 00 10 00 00 00 00    ?.lD...?...?....
50: 00 00 00 00 00 00 00 00 00 00 00 00 00 10 00 00    .............?..
60: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 10    ...............?
70: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 d6    ...............?
80: ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff    ................
90: ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff    ................
a0: ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff    ................
b0: ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff    ................
c0: ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff    ................
d0: ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff    ................
e0: ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff    ................
f0: ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff    ................
```

The edid header is visible in the first 8 bytes.
  
  
Reading it back and decoding it will result in  
```
EDID version: 1.3
Manufacturer: ADA Model 4 Serial Number 1
Made in week 1 of 2007
Digital display
Maximum image size: 15 cm x 10 cm
Gamma: 1.00
RGB color display
First detailed timing is preferred timing
Display x,y Chromaticity:
  Red:   0.0000, 0.0000
  Green: 0.0000, 0.0000
  Blue:  0.0000, 0.0000
  White: 0.0000, 0.0000
Established timings supported:
Standard timings supported:
Detailed mode: Clock 25.000 MHz, 108 mm x 68 mm
                800  840  888  928 hborder 0
                480  493  496  525 vborder 0
               -hsync -vsync
               VertFreq: 51 Hz, HorFreq: 26939 Hz
Dummy block
Dummy block
Dummy block
Checksum: 0xd6 (valid)
```


links :  
https://github.com/tomka/write-edid
https://github.com/bulletmark/edid-rw  
https://chalk-elec.com/?p=1905  
