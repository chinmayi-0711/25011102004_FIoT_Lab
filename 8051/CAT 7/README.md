

# CAT7 – 16×2 LCD Driver Development and Dynamic Display Control

| EXP | CAT7 |
|---|---|
| DATE | 24/08/2026 |
| NAME OF EXP | 16×2 LCD Driver Development and Dynamic Display Control |

## Hardware Setup / Platform

Development Board: 8051 Development Board  
Microcontroller: AT89S52  
CPU Core: 8051-compatible 8-bit microcontroller core  
Display: 16×2 HD44780-compatible Character LCD  
Input Device: 4×4 Matrix Keypad (Task 3)  
LCD Interface: 4-bit GPIO-based interface  
Programming Environment: Keil µVision  
Programming Language: Embedded C  

### LCD Pin Connections

| Signal | AT89S52 Pin | LCD Pin | Function |
|---|---|---|---|
| Data | P3.4 | D4 | Data transmission |
| Data | P3.5 | D5 | Data transmission |
| Data | P3.6 | D6 | Data transmission |
| Data | P3.7 | D7 | Data transmission |
| Enable | P3.3 | E | Enables data transfer |
| Register Select | P3.2 | RS | Selects command or data |

## Task No

Develop a 4-bit GPIO-based LCD driver for interfacing a 16×2 character LCD with the AT89S52 microcontroller.

Initialize the LCD for 4-bit, 2-line operation and configure display control and cursor movement.

Transmit character and string data to the LCD.

Implement cursor positioning using DDRAM addresses.

Display integer values by converting them into individual decimal characters.

Interface a keypad with the LCD and control the movement of a character using directional inputs.

### Task 1 – LCD Initialization and Character/String Display

Configure the 16×2 LCD in 4-bit mode and implement basic LCD initialization and character transmission.

### Task 2 – Cursor Positioning and Dynamic Value Update

Develop reusable LCD functions for command transmission, character transmission, cursor positioning, string display and integer display.

### Task 3 – 4×4 Keypad-Based Character Movement

Interface a keypad with the 8051 and move the character A across the 16×2 LCD based on directional inputs.

## Circuit Diagram

### Task 1 – LCD Initialization and Character/String Display

[Circuit Diagram](TASK1/circuit.jpeg)

### Task 2 – Cursor Positioning and Dynamic Value Update

[Circuit Diagram](TASK2/circuit.jpeg)

### Task 3 – 4×4 Keypad-Based Character Movement

[Circuit Diagram](TASK3/circuit.jpeg)

## Hardware Connections

### Common LCD Connections

| AT89S52 Pin | LCD Pin |
|---|---|
| P3.4 | D4 |
| P3.5 | D5 |
| P3.6 | D6 |
| P3.7 | D7 |
| P3.3 | E |
| P3.2 | RS |

The LCD also requires power, ground, contrast and RW connections according to the actual hardware wiring.

### Task 3 – Keypad Connections

| AT89S52 Pin | Program Signal |
|---|---|
| P2.7 | ROW7 |
| P2.6 | ROW6 |
| P2.2 | UP and DOWN |
| P2.3 | LEFT |
| P2.1 | RIGHT |

## Core Logic / Pseudocode / Code Base

### LCD Configuration

The LCD is interfaced using four data lines. The RS pin selects between command and data transmission, while the Enable pin controls the transfer of information.

| Signal / Concept | Configuration | Purpose |
|---|---|---|
| Data Lines | P3.4–P3.7 | Transmit 4-bit data |
| RS | P3.2 | Selects command or data |
| E | P3.3 | Enables LCD data transfer |
| Interface | 4-bit mode | Reduces the number of data pins |
| DDRAM | 0x00 / 0x40 | Starting addresses of the two LCD rows |

### Task 1 – LCD Initialization and Character/String Display

**Working:**

1. Configure the LCD interface pins.
2. Send the Function Set command to select 4-bit communication and 2-line operation.
3. Configure the display control settings.
4. Enable the display and cursor blinking.
5. Configure the entry mode to increment the cursor automatically.
6. Clear the LCD display.
7. Transmit character data through the LCD data pins.

### Task 2 – Cursor Positioning and Dynamic Value Update

**Working:**

1. Initialize the LCD in 4-bit mode.
2. Implement functions for command and character transmission.
3. Calculate DDRAM addresses based on the required row and column.
4. Use lcd_goto() to position the cursor.
5. Use lcd_puts() to display strings character by character.
6. Use lcd_print_number() to extract digits using modulus and division.
7. Display the extracted digits in the correct order.
8. Clear and update the display with new messages.

### Task 3 – 4×4 Keypad-Based Character Movement

**Working:**

1. Initialize the LCD and keypad interface.
2. Display the character A at row 1, column 1.
3. Continuously scan the keypad inputs.
4. Use software delays for basic key debouncing.
5. Identify the selected direction.
6. Erase the character from its current position.
7. Update the row or column based on the selected direction.
8. Display A at the new position.
9. Prevent movement beyond the LCD boundaries.

### Pseudocode

**Task 1 – LCD Initialization and Character Display**

```text
START

CONFIGURE LCD PINS

INITIALIZE LCD:
    Select 4-bit mode
    Enable display and cursor
    Enable cursor increment
    Clear display

REPEAT FOREVER:
    Transmit character data
    Apply delay

END
```

**Task 2 – Cursor Positioning and Dynamic Value Update**

```text
START

INITIALIZE LCD

DEFINE LCD FUNCTIONS:
    lcd_cmd()
    lcd_data()
    lcd_goto()
    lcd_puts()
    lcd_print_number()

REPEAT FOREVER:
    Display "LCD testing..."
    Clear display
    Reset position counter

    Display "Hello Dinesh Sir :)"
    Apply delay
    Clear display
    Reset position counter

END
```

**Task 3 – Keypad-Based Character Movement**

```text
START

INITIALIZE LCD

SET:
    row = 1
    col = 1

DISPLAY CHARACTER A

REPEAT FOREVER:
    Scan keypad

    IF UP is detected:
        Move character upward

    ELSE IF DOWN is detected:
        Move character downward

    ELSE IF LEFT is detected:
        Move character left

    ELSE IF RIGHT is detected:
        Move character right

    Check LCD boundaries
    Update character position

END
```

### Key Observations

The LCD communicates with the AT89S52 using four data lines.

The RS pin selects between command and character data transmission.

The Enable pin controls the transfer of information to the LCD.

The LCD uses DDRAM addresses for cursor positioning.

The first row starts at DDRAM address 0x00, while the second row starts at 0x40.

Software delays are used to provide the required LCD timing.

Task 1 demonstrates basic LCD initialization and character transmission.

Task 2 demonstrates cursor positioning, string handling and integer conversion.

Task 3 combines keypad scanning with dynamic character movement.

### Display Verification

**Task 1 Expected Output:**

The program intends to display the character L repeatedly.

**Task 2 Expected Output:**

```text
LCD testing...

Hello Dinesh Sir :)
```

The two messages are displayed alternately with the LCD cleared between them.

**Task 3 Expected Output:**

The character A initially appears at row 1, column 1.

| Key | Action |
|---|---|
| 2 | Move Up |
| 5 | Move Down |
| 4 | Move Left |
| 6 | Move Right |

The character is intended to move according to the directional keypad inputs.

### Source Code

Task 1 – [TASK1.C](TASK1/TASK1.C)

Task 2 – [TASK2.C](TASK2/TASK2.C)

Task 3 – [TASK3.C](TASK3/TASK3.C)

## Inference

The experiment demonstrates the interfacing and control of a 16×2 character LCD using the AT89S52 microcontroller.

In Task 1, LCD initialization and character transmission are explored using a 4-bit GPIO interface.

In Task 2, reusable LCD driver functions are developed for cursor positioning, string display and integer conversion.

In Task 3, keypad inputs are used to control the position of a character on the LCD while maintaining row and column boundaries.

The experiment provides practical experience in LCD command transmission, DDRAM addressing, GPIO interfacing, software timing delays and peripheral control using Embedded C.

The supplied programs contain some initialization and keypad pin-mapping issues that should be verified against the actual hardware connections and LCD datasheet.
