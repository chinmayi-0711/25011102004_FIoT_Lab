

# CAT6 – UART Driver and Serial Debugging

|                 |                                      |
| --------------- | ------------------------------------ |
| **EXP**         | CAT6                                 |
| **DATE**        | 17/08/2026                           |
| **NAME OF EXP** | UART Driver and Serial Debugging    |

---

## Hardware Setup / Platform

- **Development Board:** 8051 Development Board
- **Microcontroller:** AT89S52
- **CPU Core:** 8051-compatible 8-bit microcontroller core
- **Crystal Frequency:** 11.0592 MHz
- **Programming Environment:** Keil µVision
- **Programming Language:** Embedded C
- **Serial Interface:** USB-to-TTL Converter
- **Terminal Software:** PuTTY
- **UART Mode:** Mode 1 (8-bit asynchronous serial communication)
- **Baud Rate:** 9600 bps
- **Communication Format:** 9600-8-N-1
- **Timer Used:** Timer 1, Mode 2 (8-bit auto-reload)

### UART Pin Connections

| **Signal** | **AT89S52 Pin** | **Function** |
| ---------- | -------------- | ------------ |
| TXD        | P3.1           | Serial data transmission |
| RXD        | P3.0           | Serial data reception |
| GND        | GND            | Common ground |

---

## Task No

1. Configure the AT89S52 UART for 9600-8-N-1 serial communication.
2. Continuously transmit the message `Hello from 8051` to a PC terminal.
3. Receive characters transmitted from the PC terminal through UART.
4. Transmit the received characters back to the PC, implementing UART echo functionality.

### Task 1 – UART Initialization and Character Transmission

Configure the UART and continuously transmit the predefined message `Hello from 8051` to the PC terminal using PuTTY.

### Task 2 – UART Echo

Receive characters from the PC terminal and transmit the same characters back through UART.

---

## Circuit Diagram

### Task 1 – UART Transmission

[Circuit Diagram](TASK1/circuit.jpeg)

### Task 2 – UART Echo

[Circuit Diagram](TASK2/circuit.jpeg)

### Hardware Connections

| **AT89S52 / Board Pin** | **USB-to-TTL Converter** |
| ----------------------- | ------------------------ |
| P3.1 (TXD)              | RXD                      |
| P3.0 (RXD)              | TXD                      |
| GND                     | GND                      |

The USB-to-TTL converter is connected to the PC through USB and communicates with PuTTY through the corresponding serial COM port.

The AT89S52 uses an external 11.0592 MHz crystal connected to XTAL1 and XTAL2 for clock generation.

---

## Core Logic / Pseudocode / Code Base

### UART Configuration

The UART is configured in Mode 1 for 8-bit asynchronous serial communication. Timer 1 is configured in Mode 2 to generate the required baud rate.

| **Register / Bit** | **Configuration** | **Purpose** |
| ------------------ | ----------------- | ----------- |
| PCON               | `0x00`            | Clears SMOD for normal baud rate |
| SCON               | `0x40` / `0x50`   | Selects UART Mode 1; receiver enabled in Task 2 |
| TMOD               | `0x20`            | Configures Timer 1 in Mode 2 |
| TH1                | `0xFD`            | Reload value for 9600 baud |
| TL1                | `0xFD`            | Initial Timer 1 counter value |
| TR1                | `1`               | Starts Timer 1 |

### Task 1 – UART Initialization and Character Transmission

**Working:**

1. Configure UART in Mode 1 for 8-bit asynchronous communication.
2. Clear the SMOD bit for normal baud-rate selection.
3. Configure Timer 1 in Mode 2.
4. Load TH1 and TL1 with `0xFD` to generate 9600 baud using the 11.0592 MHz crystal.
5. Start Timer 1.
6. Store the message `Hello from 8051` in a character array.
7. Load each character into SBUF for transmission.
8. Wait until TI becomes 1, indicating transmission completion.
9. Clear TI and transmit the next character.
10. Repeat the message continuously.

### Task 2 – UART Echo

**Working:**

1. Initialize UART in Mode 1 with reception enabled using REN.
2. Configure Timer 1 in Mode 2 for 9600 baud.
3. Start Timer 1.
4. Continuously monitor the RI flag.
5. When RI becomes 1, read the received character from SBUF.
6. Clear RI after reading the character.
7. Pass the received character to the transmission function.
8. Load the character into SBUF.
9. Wait until TI becomes 1.
10. Clear TI and repeat the process for the next character.

### Pseudocode

**Task 1 – UART Transmission**

```text
START

INITIALIZE UART:
    Configure UART Mode 1
    Clear SMOD bit
    Configure Timer 1 in Mode 2
    Load TH1 = 0xFD
    Load TL1 = 0xFD
    Start Timer 1

STORE MESSAGE:
    "Hello from 8051"

REPEAT FOREVER:
    FOR each character in the message:
        Load character into SBUF
        Wait until TI = 1
        Clear TI
    END FOR

END
```

**Task 2 – UART Echo**

```text
START

INITIALIZE UART:
    Configure UART Mode 1
    Enable receiver using REN
    Configure Timer 1 in Mode 2
    Load TH1 = 0xFD
    Load TL1 = 0xFD
    Start Timer 1
    Clear RI and TI

REPEAT FOREVER:
    Wait until RI = 1
    Read received character from SBUF
    Clear RI

    Load received character into SBUF
    Wait until TI = 1
    Clear TI

END
```

### Key Observations

- Timer 1 in Mode 2 generates the baud-rate timing for UART communication.
- The 11.0592 MHz crystal with TH1 = 0xFD produces a baud rate of 9600 bps when SMOD is cleared.
- The SBUF register is used for both serial data transmission and reception.
- The TI flag indicates completion of character transmission.
- The RI flag indicates that a character has been received.
- Task 1 continuously transmits a predefined message.
- Task 2 receives and echoes characters entered through PuTTY.
- Both implementations use polling to monitor UART status flags.
- UART interrupts and an RX ring buffer are not implemented.

### Communication Verification

PuTTY communication settings:

| **Parameter** | **Value** |
| ------------- | --------- |
| Baud Rate     | 9600      |
| Data Bits     | 8         |
| Stop Bits     | 1         |
| Parity        | None      |
| Flow Control  | None      |

**Task 1 Expected Output:**

```text
Hello from 8051
Hello from 8051
Hello from 8051
...
```

**Task 2 Expected Output:**

Input entered in PuTTY:

```text
ABC123
```

Characters echoed by the 8051:

```text
ABC123
```

### Source Code

- [Task 1 – TASK1.C](Task1/TASK1.C)
- [Task 2 – TASK2.C](Task2/TASK2.C)

---

## Inference

The experiment demonstrates UART-based serial communication between the AT89S52 microcontroller and a PC terminal.

UART Mode 1 was configured for 8-bit asynchronous communication, while Timer 1 in Mode 2 generated the required baud rate of 9600 bps using an 11.0592 MHz crystal.

In Task 1, the microcontroller continuously transmitted a predefined message to the PC terminal through a USB-to-TTL converter.

In Task 2, the microcontroller received characters from the PC and transmitted the same characters back, demonstrating UART echo functionality.

The experiment demonstrates UART initialization, baud-rate generation, serial data transmission and reception using SBUF, and status monitoring using the TI and RI flags.

The implementation uses polling rather than interrupt-driven UART communication.
