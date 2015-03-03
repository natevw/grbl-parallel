Note: these match up with http://www.carving-cnc.com/4axis-breakout-board-450jkb.html as well


## Ground

Pin No (DB25) 18–25
http://en.wikipedia.org/wiki/Parallel_port#Pinouts


## Steppers

X Axis Step Pin 2, Dir Pin 3 (Step Low) Step Port 1, Dir Port 1
Y Axis Step Pin 4, Dir Pin 5 (Step Low) Step Port 1, Dir Port 1
Z Axis Step Pin 6, Dir Pin 7 (Step Low) Step Port 1, Dir Port 1
http://www.machsupport.com/forum/index.php/topic,26639.msg187893.html#msg187893

    #define X_STEP_BIT      2  // Uno Digital Pin 2
    #define Y_STEP_BIT      3  // Uno Digital Pin 3
    #define Z_STEP_BIT      4  // Uno Digital Pin 4
    
    #define X_DIRECTION_BIT   5  // Uno Digital Pin 5
    #define Y_DIRECTION_BIT   6  // Uno Digital Pin 6
    #define Z_DIRECTION_BIT   7  // Uno Digital Pin 7

```
$H
G0 X-139.5 Y-190
G0 X-250 Y-250
```

## Stepper enable (???)

Enable Port1, Pin 14
http://www.machsupport.com/forum/index.php/topic,26639.msg187994.html#msg187994
-->
It looks that the pin 16 is the Enable pin.
http://www.machsupport.com/forum/index.php/topic,26639.msg187944.html#msg187944

    #define STEPPERS_DISABLE_BIT    0  // Uno Digital Pin 8

**Many YooCNC-ish boards, does not seem to be defined (motor lock whenever powered on!)


## Spindle

Set Mach3 for Active Low on Pin #1 to turn the Spindle On/Off. (M3/M5)
Set Mach3 to control the Spindle PWM using pin #17
http://www.cnc-arena.com/en/forum/cnc-3020t-dj-spindle-control-jp-382a-jp-1482--241512.html
** needs invert: https://github.com/grbl/grbl/issues/479

    #ifdef VARIABLE_SPINDLE // Z Limit pin and spindle enabled swapped to access hardware PWM on Pin 11.  
    //#define SPINDLE_ENABLE_BIT    3  // Uno Digital Pin 11
    #else
      #define SPINDLE_ENABLE_BIT    4  // Uno Digital Pin 12
    #endif

```
M3
M5
```

## E-stop / probe / etc.

Mine was pin 10 for E-Stop and pin 15 for Probe (F100 G38.2 Z-45)
http://www.cnczone.com/forums/chinese-machines/240934-cnc-forum-posts.html#post1560984
** Uno RESET better option for E-Stop! https://github.com/grbl/grbl/issues/604#issuecomment-75640092

    #define PIN_RESET        0  // Uno Analog Pin 0
    #define PIN_FEED_HOLD    1  // Uno Analog Pin 1
    #define PIN_CYCLE_START  2  // Uno Analog Pin 2
    
    #define PROBE_BIT       5  // Uno Analog Pin 5

```
F50 G38.2 Z-45
$X
F150 G38.2 Z-45
G0 Z-30
```


## Limit switches

Pins 11, 12, and 13 are the outputs of X,Y and Z.
http://www.cnczone.com/forums/chinese-machines/178465-anyone-added-switches-yoocnc-3040t-control-box-8.html#post1508798

    #define X_LIMIT_BIT      1  // Uno Digital Pin 9
    #define Y_LIMIT_BIT      2  // Uno Digital Pin 10
    #ifdef VARIABLE_SPINDLE // Z Limit pin and spindle enabled swapped to access hardware PWM on Pin 11.  
      #define Z_LIMIT_BIT	   4 // Uno Digital Pin 12
    #else
      #define Z_LIMIT_BIT    3  // Uno Digital Pin 11
    #endif

```
G1 F250 X0 Y0 Z0
^X
$H
```
