
# CAT6 – Task 2: UART Echo using 8051 Serial Communication

|                 |                                                |
| --------------- | ---------------------------------------------- |
| **EXP**         | CAT6 – Task 2                                  |
| **DATE**        | 17/08/2026                            |
| **NAME OF EXP** | UART Driver and Serial Debugging               |

---

## Hardware Setup / Platform

- **Development Board:** 8051 Development Board
- **Microcontroller:** AT89S52
- **CPU Core:** 8051-compatible 8-bit microcontroller core
- **Crystal Frequency:** 11.0592 MHz
- **Programming Environment:** Keil µVision
- **Programming Language:** Embedded C
- **UART Mode:** Mode 1 (8-bit asynchronous serial communication)
- **Baud Rate:** 9600 bps
- **Data Bits:** 8
- **Stop Bits:** 1
- **Parity:** None
- **Transmit Pin:** P3.1 (TXD)
- **Receive Pin:** P3.0 (RXD)
- **Timer Used:** Timer 1
- **Timer Mode:** Mode 2 (8-bit auto-reload)
- **Serial Interface:** USB-to-TTL Converter
- **Terminal Software:** PuTTY

### Pin Connections

| **Signal** | **AT89S52 Pin** | **USB-to-TTL Converter** |
| ---------- | -------------- | ------------------------ |
| TXD        | P3.1           | RXD                      |
| RXD        | P3.0           | TXD                      |
| GND        | GND            | GND                      |

The USB-to-TTL converter is connected to the PC through USB and communicates with PuTTY through the corresponding serial COM port.

---

## Task No

**Task 2 – UART Echo**

Receive characters transmitted from the PC terminal through UART and transmit the same characters back to the PC.

For example, when the user enters `ABC123` in PuTTY, the AT89S52 receives each character and sends it back to the terminal.

---

## Circuit Diagram

[Circuit Diagram](circuit.jpeg)

The circuit consists of the AT89S52 microcontroller, USB-to-TTL serial converter and PC terminal.

The TXD pin (P3.1) of the AT89S52 is connected to the RXD pin of the USB-to-TTL converter. Similarly, the RXD pin (P3.0) is connected to the TXD pin of the converter.

The grounds of both devices are connected together. The USB-to-TTL converter communicates with the PC through USB.

---

## Core Logic / Pseudocode / Code Base

### Working Principle

The UART is configured in Mode 1 for 8-bit asynchronous serial communication. The receiver is enabled by setting the REN bit through `SCON = 0x50`.

Timer 1 is configured in Mode 2 to generate the baud rate of 9600 using the 11.0592 MHz crystal and the reload value `TH1 = 0xFD`.

The program uses three functions:

- `uart_init()` – Configures the UART and Timer 1.
- `uart_receive()` – Waits for a character to arrive, reads it from SBUF and clears the RI flag.
- `uart_transmit()` – Loads the received character into SBUF, waits for the TI flag and clears it after transmission.

The main function continuously receives a character from PuTTY and transmits the same character back to the PC. This creates a UART echo system.

### UART Configuration

| **Register / Bit** | **Configuration** | **Purpose** |
| ------------------ | ----------------- | ----------- |
| PCON               | `0x00`            | Clears SMOD for normal baud rate |
| SCON               | `0x50`            | Selects UART Mode 1 and enables reception |
| TMOD               | `0x20`            | Configures Timer 1 in Mode 2 |
| TH1                | `0xFD`            | Sets the reload value for 9600 baud |
| TL1                | `0xFD`            | Initializes Timer 1 |
| TR1                | `1`               | Starts Timer 1 |
| REN                | `1`               | Enables serial reception |
| RI                 | `0`                | Cleared after reading received data |
| TI                 | `0`                | Cleared after transmission |

### Pseudocode

```text
START

1. Initialize UART:
   - Set PCON = 0x00.
   - Configure SCON = 0x50 for UART Mode 1.
   - Configure Timer 1 in Mode 2.
   - Load TH1 and TL1 with 0xFD.
   - Start Timer 1.
   - Clear RI and TI.

2. REPEAT FOREVER:

   a. Wait until RI = 1.
   b. Read the received character from SBUF.
   c. Clear RI.
   d. Load the received character into SBUF.
   e. Wait until TI = 1.
   f. Clear TI.

END
```

### Key Observations

- The receiver is enabled using the REN bit in the SCON register.
- The RI flag indicates that a character has been received.
- The received character is read from the SBUF register.
- The RI flag is cleared after reading the received character.
- The same character is transmitted back through SBUF.
- The TI flag indicates completion of character transmission.
- The program uses polling to monitor RI and TI.
- The implementation does not use UART interrupts or an RX ring buffer.

### Expected Output

When the user types the following characters in PuTTY:

```text
ABC123
```

The AT89S52 receives each character and transmits it back to the PC terminal.

Expected echoed output:

```text
ABC123
```

### Source Code

The complete implementation is available in the source file included in this folder:

[TASK2.C](TASK2.C)

---

## Inference

The experiment demonstrates bidirectional serial communication between the AT89S52 microcontroller and a PC terminal using UART.

UART Mode 1 was configured for 9600-8-N-1 communication, while Timer 1 in Mode 2 generated the required baud rate using the 11.0592 MHz crystal.

The RI and TI flags were used to identify the completion of character reception and transmission, respectively. The received character was read from SBUF and transmitted back to the PC, demonstrating UART echo functionality.

The experiment demonstrates UART initialization, character reception, serial data transmission and status monitoring using the RI and TI flags.

