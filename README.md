# Embedded_C

What is Embedded C?

Embedded C is an extension of the C programming language used to develop firmware for embedded systems such as microcontrollers (MCUs) and microprocessors (MPUs). It allows software to directly interact with hardware like GPIOs, timers, UART, ADC, SPI, I2C, and CAN through memory-mapped registers.

Unlike standard C programming, Embedded C focuses on:

Real-time execution
Hardware control
Low memory usage
High reliability
Power efficiency
What is an Embedded System?

An embedded system is a computer system designed to perform a specific task.

Examples:

Washing machine
Air conditioner
Car ECU
Smartwatch
Printer
IoT Weather Station
Medical devices
Industrial automation controllers
Block Diagram
          +-----------------------+
          |      Sensors          |
          +----------+------------+
                     |
                     v
          +-----------------------+
          | Microcontroller (MCU) |
          |  CPU + RAM + Flash    |
          +----------+------------+
                     |
        +------------+-------------+
        |            |             |
      GPIO         UART         SPI/I2C
        |            |             |
        v            v             v
      LED         Bluetooth      Sensors
Why Embedded C Instead of Standard C?
Standard C	Embedded C
General-purpose programming	Firmware development
Runs on PC	Runs on MCU/MPU
Limited hardware access	Direct hardware access
OS often available	Often bare-metal or RTOS
Uses standard I/O	Uses peripherals like UART, GPIO, ADC
Features of Embedded C
Hardware register programming
Bit manipulation
Interrupt handling
Low memory footprint
Deterministic execution
Peripheral interfacing
Real-time operation
Cross-compilation
Embedded System Architecture
          Application Layer
                 |
          Device Drivers
                 |
        HAL (Hardware Abstraction)
                 |
      Registers / Peripheral Drivers
                 |
         Microcontroller Hardware
Typical Embedded C Project Structure
Project
│
├── main.c
├── gpio.c
├── gpio.h
├── uart.c
├── uart.h
├── adc.c
├── timer.c
├── interrupt.c
├── startup.s
├── linker.ld
└── Makefile
Memory Organization
High Address
--------------------
| Stack            |
--------------------
| Heap             |
--------------------
| BSS (Uninitialized Globals) |
--------------------
| Data (Initialized Globals)  |
--------------------
| Text (Code + Constants)     |
--------------------
Low Address

Sections
Text: Program instructions and constant data.
Data: Initialized global/static variables.
BSS: Uninitialized global/static variables (initialized to zero at startup).
Heap: Dynamic memory (malloc, free).
Stack: Function calls, local variables, return addresses.
Embedded C Data Types
Type	Typical Size (32-bit MCU)
char	1 byte
short	2 bytes
int	4 bytes
long	4 bytes
long long	8 bytes
float	4 bytes
double	8 bytes (or 4 bytes on some MCUs)

Prefer fixed-width types from <stdint.h>:

uint8_t
uint16_t
uint32_t
int8_t
int16_t
int32_t
Memory-Mapped Registers

Peripherals are controlled by reading/writing specific memory addresses.

Example:

#define GPIO_ODR (*(volatile unsigned int *)0x40020014)

int main()
{
    GPIO_ODR |= (1 << 5);   // Set GPIO pin 5
}
Why volatile?

Without volatile, the compiler may optimize away repeated accesses to a hardware register because it assumes the value does not change unexpectedly.

volatile int flag;

Use volatile for:

Hardware registers
ISR-shared variables
DMA-updated memory
Bit Manipulation
Set Bit
reg |= (1 << n);
Clear Bit
reg &= ~(1 << n);
Toggle Bit
reg ^= (1 << n);
Check Bit
if(reg & (1 << n))
{
    // Bit is set
}
Pointers in Embedded C

Pointers are widely used for:

Register access
DMA buffers
Device drivers
Function callbacks
Dynamic data structures

Example:

int value = 10;
int *ptr = &value;

printf("%d", *ptr);
Interrupts

An interrupt temporarily pauses the current program execution to handle an urgent event.

Examples:

UART receives a byte
Timer expires
GPIO button press
ADC conversion complete

Flow:

Main Program
      |
Interrupt Occurs
      |
ISR Executes
      |
Return to Main Program
Polling vs Interrupt
Polling	Interrupt
CPU repeatedly checks status	Hardware notifies CPU
Wastes CPU cycles	Efficient CPU utilization
Simpler	More responsive
GPIO

General Purpose Input/Output pins are used to:

Read switches
Drive LEDs
Control relays
Interface external devices

Example:

GPIOA->MODER |= (1 << 10);   // Configure pin as output
GPIOA->ODR |= (1 << 5);      // Set pin high
Timers

Timers are used for:

Delays
PWM generation
Periodic interrupts
Frequency measurement
Input capture
ADC (Analog-to-Digital Converter)

Converts analog voltage into digital values.

Example:

Temperature sensor
Potentiometer
Battery voltage monitoring
Communication Protocols
UART
Asynchronous communication
TX and RX pins
Debug messages
GPS modules
SPI
High-speed synchronous communication
MOSI, MISO, SCK, CS
Flash memory
Displays
I2C
Two-wire bus (SDA, SCL)
Multiple devices on one bus
EEPROM
Sensors
CAN
Robust automotive communication
Priority-based arbitration
Used in ECUs
Watchdog Timer

A watchdog resets the system if the software hangs.

Flow:

Program Running
      |
Refresh Watchdog
      |
No Refresh?
      |
System Reset
Boot Process
Power ON
     |
Reset Handler
     |
System Initialization
     |
Initialize .data and .bss
     |
main()
RTOS vs Bare Metal
Bare Metal	RTOS
Single execution flow	Multiple tasks
No scheduler	Scheduler manages tasks
Simpler	Better for complex applications
Common Embedded C Interview Concepts
Pointers and pointer arithmetic
Arrays and strings
Structures, unions, and bit-fields
Bitwise operations
Memory layout
volatile, const, static, extern
Function pointers
Interrupt Service Routines (ISRs)
Memory-mapped I/O
Endianness
Dynamic memory (and why it's often avoided)
Circular buffers
Linked lists
Stack vs Heap
Polling vs Interrupts
State machines
Communication protocols (UART, SPI, I2C, CAN)
RTOS basics (tasks, semaphores, queues, mutexes)
Embedded C Best Practices
Use descriptive names for variables and functions.
Prefer fixed-width integer types (uint32_t, int16_t, etc.).
Minimize dynamic memory allocation in embedded systems.
Protect shared data between ISRs and main code.
Keep ISRs short and non-blocking.
Use const for read-only data stored in Flash.
Use volatile only where necessary.
Follow coding standards such as MISRA C for safety-critical software.
Modularize code into drivers and application layers.


