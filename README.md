# tektronix-4052-progs
Recently I restored an old Tektronix 4052A Computer. Thanks to VCF Forums, specially to [Monty McGraw](https://github.com/mmcgraw74) and his repository of lot of documentation and programs to use with Tek 405x!
This little repo that contains Columbia Space Shuttle CAD code converted HP85 BASIC program from [Calculator Nostalgia](https://calc.fjk.ch/bibliothek/programme/hp-85-space-shuttle/) to Tektronix 405x BASIC with Plotter HP support included.

# Upload procedure programs to Tektronix 4052A:

## Configure Tektronix:

Configuration by and thanks to [Monty McGraw](https://github.com/mmcgraw74).

First you need check if CMFLAGS are exists into parameters list. If exists, then you have a 4052 with v5.1 BASIC firmware OR a 4052A.

If you see a CMFLAG parameter - then you have a 4052 with v5.1 BASIC firmware OR a 4052A.
That means you can use XON/XOFF serial protocol to transfer files at 9600 baud to/from your PC to your 4052 computer!

```
CALL "PRLIST"
```

Now configure your 4052 serial interface:

```
CALL "RATE",9600,5,0      | to set the rate to 9600 baud, 8-bits, No Parity, 1 Stop Bit
CALL "TSTRIN","","",""    | stops the 4050 from modifying control characters on serial transmit
CALL "RSTRIN","","",""    | stops the 4050 from modifying control characters on serial receive
CALL "CMFLAG",3           | set XON/XOFF protocol for serial TX and RX
```

## Configure terminal application on PC

Download [RealTerm](https://realterm.sourceforge.io/) on windows or use MiniTerm on Linux for example. 
Configure terminal program with: (9600-N-8-1)

9600 baud 
No parity
8 data bits
1 stop bit
none Hardware Flow Control
XON/XOFF Receive and Transmit Software Flow Control

## Check on Tektronix configuration works:

```
PRINT@40:"HELLO FROM TEKTRONIX"
```

And see your terminal application if string shows or not.

## Prepare to receive program from PC

For my little experience, when you create BASIC programs with PC editor you need create ASCII/ANSI with CR end of line!

```
DELETE ALL
OLD@40:
```

### Clear serial buffers on Tektronix (If is necessary)

When program are transmited from PC to Tektronix, you need interrupt with press BREAK two times. If transfer is interrupted for somethin BASIC line erroneus, you ensure flush Tektronix buffers:

```
CALL"TERMIN"
```
Wait to flush all content from serial buffers, and then press User Definable Key 5 (two times) to interrupt Serial terminal program on Tektronix.
