# UART Communication Between STM32F4 and Arduino Uno

## Project Overview

This project demonstrates a **serial UART communication** between two embedded boards:

- **STM32F4** configured as the **master/transmitter**
- **Arduino Uno** configured as the **receiver/slave**

The STM32F4 sends a message through the UART interface, and the Arduino Uno receives the message and displays it on the **Arduino Serial Monitor**.

The project provides a simple example of communication between two different microcontroller platforms using the **UART (Universal Asynchronous Receiver/Transmitter)** protocol.

## Objectives

The main objectives of this project are:

- Configure UART communication on the STM32F4.
- Configure the Arduino Uno to receive UART data.
- Send a message from the STM32F4.
- Receive the message on the Arduino Uno.
- Display the received message on the Arduino Serial Monitor.
- Understand the basic principles of UART communication between embedded systems.

## Hardware Requirements

- STM32F4 development board
- Arduino Uno
- USB cable for STM32F4
- USB cable for Arduino Uno
- Jumper wires
- Computer

## Communication Diagram

```text
        STM32F4                         Arduino Uno
     (UART Master)                   (UART Receiver)
          TX  ---------------------->  RX
          GND -----------------------  GND
          
          USB                            USB
           |                              |
           v                              v
        Computer                    Arduino IDE
                                      Serial Monitor
```

## UART Configuration

The same UART parameters must be configured on both boards.

| Parameter | Configuration |
|---|---|
| Baud Rate | 9600 bps |
| Data Bits | 8 |
| Parity | None |
| Stop Bits | 1 |
| Flow Control | None |
| Mode | Asynchronous |

The communication uses the standard **8N1** UART format.

## Wiring

Connect the boards as follows:

| STM32F4 | Arduino Uno |
|---|---|
| TX | RX |
| GND | GND |

### Important

UART communication requires a **common ground** between the two boards.

Also verify the voltage levels of the specific STM32F4 board before connecting it directly to an Arduino Uno. Many STM32F4 boards use **3.3 V logic**, while the Arduino Uno uses **5 V logic**. A suitable level-shifting solution may be required to protect the STM32F4 RX input if bidirectional communication is implemented.

For this project, only the STM32F4 TX → Arduino RX direction is required.

## STM32F4 Operation

The STM32F4 is configured as the UART master/transmitter.

Its firmware initializes the UART peripheral and periodically sends a predefined message.

Example message:

```text
Hello from STM32F4!
```

The STM32F4 sends the message through its UART TX pin.

Example transmission logic:

```c
HAL_UART_Transmit(&huart2,
                  (uint8_t*)"Hello from STM32F4!\r\n",
                  20,
                  HAL_MAX_DELAY);
```

> The UART instance and `huart` variable may need to be modified depending on the STM32F4 board and project configuration.

## Arduino Uno Operation

The Arduino Uno receives the data through its RX pin.

The received characters are then displayed on the Arduino Serial Monitor.

Example Arduino code:

```cpp
void setup() {
  Serial.begin(9600);
}

void loop() {
  if (Serial.available()) {
    String message = Serial.readStringUntil('\n');
    Serial.println("Received: " + message);
  }
}
```

### Serial Monitor Configuration

Open the Arduino IDE and select:

**Tools → Serial Monitor**

Set the baud rate to:

```text
9600 baud
```

When the STM32F4 sends the message, the Arduino Serial Monitor should display:

```text
Received: Hello from STM32F4!
```

## Project Structure

```text
STM32F4-Arduino-UART/
│
├── STM32F4/
│   ├── Core/
│   ├── Drivers/
│   └── ...
│
├── Arduino/
│   └── UART_Receiver/
│       └── UART_Receiver.ino
│
└── README.md
```

## How to Run the Project

### 1. Program the STM32F4

Open the STM32 project using **STM32CubeIDE**.

Build and flash the firmware to the STM32F4 board.

Make sure the UART peripheral is configured with:

```text
Baud Rate: 9600
Data: 8 bits
Parity: None
Stop Bits: 1
```

### 2. Upload the Arduino Code

Open the `.ino` file using the **Arduino IDE**.

Select:

```text
Board: Arduino Uno
```

Select the correct COM port and upload the program.

### 3. Connect the Boards

Connect:

```text
STM32F4 TX → Arduino RX
STM32F4 GND → Arduino GND
```

### 4. Open the Serial Monitor

Open the Arduino Serial Monitor and select:

```text
9600 baud
```

The received STM32F4 message should appear automatically.

## Expected Result

When the system is running correctly, the Arduino Serial Monitor displays the message sent by the STM32F4:

```text
Received: Hello from STM32F4!
```

This confirms that UART communication between the two embedded boards is working correctly.

## Technologies Used

- **STM32F4**
- **Arduino Uno**
- **UART**
- **C**
- **Arduino C/C++**
- **STM32CubeIDE**
- **Arduino IDE**

## Learning Outcomes

Through this project, the following concepts are demonstrated:

- UART initialization
- Serial data transmission
- Serial data reception
- TX/RX communication
- Baud rate configuration
- Microcontroller-to-microcontroller communication
- Debugging using a serial monitor
- Interfacing different embedded platforms

## Future Improvements

The project can be extended to support:

- Bidirectional UART communication
- Sending commands from Arduino to STM32F4
- Receiving sensor data
- Sending structured data or packets
- Error detection using checksums
- Interrupt-based UART reception
- DMA-based UART communication
- Communication between multiple embedded devices

## Author

**Ghazy Laffet**

Embedded Systems Engineering Student

---

⭐ This project is a basic demonstration of UART communication and can be used as a starting point for more advanced embedded communication systems.