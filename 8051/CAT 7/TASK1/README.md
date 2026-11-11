
# CAT7 – Task 1: LCD Initialization and Character/String Display

| EXP | CAT7 |
|---|---|
| DATE | 24/08/2026 |
| NAME OF EXP | 16×2 LCD Driver Development and Dynamic Display Control |

## Hardware Setup / Platform

Development Board: 8051 Development Board  
Microcontroller: AT89S52  
Display: 16×2 HD44780-compatible Character LCD  
LCD Interface: 4-bit GPIO-based interface  
Data Pins: P3.4–P3.7  
Enable Pin (E): P3.3  
Register Select (RS): P3.2  
Programming Environment: Keil µVision  
Programming Language: Embedded C  

## Task No

Task 7A – LCD Initialization and Character/String Display

Develop a basic 4-bit GPIO-based LCD interface.

Configure the 16×2 LCD for 4-bit and 2-line operation.

Configure display ON, cursor ON and cursor blinking.

Configure automatic cursor increment.

Clear the display and transmit character data.

## Circuit Diagram

LCD Initialization and Character/String Display

[Circuit Diagram](circuit.jpeg)

## Hardware Connections

| AT89S52 Pin | LCD Pin | Function |
|---|---|---|
| P3.4 | D4 | Data |
| P3.5 | D5 | Data |
| P3.6 | D6 | Data |
| P3.7 | D7 | Data |
| P3.3 | E | Enable |
| P3.2 | RS | Register Select |

The LCD also requires power, ground, contrast and RW connections according to the actual laboratory wiring.

## Core Logic / Pseudocode / Code Base

### LCD Configuration

| Command | Value | Purpose |
|---|---|---|
| Function Set | 0x28 | Selects 4-bit mode and 2-line operation |
| Display Control | 0x0F | Display ON, cursor ON and blinking ON |
| Entry Mode | 0x06 | Automatically increments cursor |
| Clear Display | 0x01 | Clears LCD contents |

### Working

1. Configure the LCD interface pins.
2. Send the Function Set command for 4-bit operation.
3. Configure display ON, cursor ON and blinking.
4. Set the entry mode to increment the cursor automatically.
5. Clear the LCD display.
6. Transmit the upper nibble of the character.
7. Transmit the lower nibble of the character.
8. Apply software delays between transmissions.

### Pseudocode

```text
START

CONFIGURE LCD PINS

SEND FUNCTION SET COMMAND
    Select 4-bit mode

CONFIGURE DISPLAY CONTROL
    Display ON
    Cursor ON
    Cursor blinking ON

CONFIGURE ENTRY MODE
    Increment cursor

CLEAR LCD

REPEAT FOREVER:
    Send upper nibble of character
    Send lower nibble of character
    Apply delay

END
```

### Key Observations

The LCD uses four data lines for 4-bit communication.

The RS pin is used to select command or data transmission.

The Enable pin controls the transfer of information to the LCD.

The Function Set command configures the LCD operating mode.

The Entry Mode command enables automatic cursor increment.

Software delays are used to provide the required LCD timing.

### Implementation Observation

The program intends to transmit the character L (ASCII 0x4C).

However, the lower-nibble output 0x3C does not correctly place the required lower nibble 0xC on P3.4–P3.7.

The Function Set sequence also contains a lower-nibble mismatch because the second port value does not correctly place the lower nibble 0x8 on the LCD data pins.

The initialization and character transmission sequences should be verified against the HD44780 datasheet.

### Source Code

[TASK1.C](TASK1.C)

## Inference

The experiment demonstrates the basic interfacing of a 16×2 character LCD with the AT89S52 microcontroller using a 4-bit GPIO-based interface.

It introduces LCD initialization commands, command and data transmission, control signals and software timing delays.

The experiment provides an understanding of how a microcontroller communicates with an LCD using a reduced number of data pins.

