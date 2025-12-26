# GPIO-LED-cycle

## Overview

This repository contains firmware written in C for the **STM32F411E-Discovery** development board.  
The program controls the on-board LEDs connected to port **PD12–PD15** and changes their lighting combination in response to the user button connected to **PA0**.

The implementation is based entirely on GPIO peripherals and uses polling with a simple software debounce mechanism.

---

## Hardware Usage

- **User Button**
  - Port: `GPIOA`
  - Pin: `PA0`

- **LEDs**
  - Port: `GPIOD`
  - Pins:
    - `PD12` (Green)
    - `PD13` (Orange)
    - `PD14` (Red)
    - `PD15` (Blue)

---

## LED Patterns

The firmware cycles through **five predefined LED combinations**:

1. `PD12` – Green LED
2. `PD13` – Orange LED
3. `PD14` – Red LED
4. `PD15` – Blue LED
5. `PD12 + PD14` – Green and Red LEDs

After the fifth pattern, the sequence restarts from the first one.

---

## Button Handling Logic

- The button is sampled using GPIO input polling.
- A **20 ms delay** is applied after detecting a press to mitigate contact bouncing.
- A state flag ensures that:
  - **Only one LED pattern change occurs per button press**
  - Holding the button does not cause repeated changes
  - The next change is allowed only after the button is released

---

## Program Behavior

- On startup, the first LED pattern is displayed.
- Each valid button press advances the LED pattern index.
- The LED output is updated by clearing all LED pins and setting the pins corresponding to the current pattern.
- The firmware runs continuously inside an infinite loop.

---

## Source Structure

- LED patterns are defined as a constant array of GPIO pin masks.
- GPIO initialization configures:
  - `PA0` as input (button)
  - `PD12–PD15` as push-pull outputs (LEDs)
- User logic is implemented within the main loop and dedicated helper functions.

---

## Development Environment

- Language: **C**
- Framework: **STM32 HAL**
- Target board: **STM32F411E-Discovery**
- Project generated and built using **STM32CubeIDE**

---

## Notes

- The implementation relies solely on GPIO and HAL delay-based debouncing.
- No interrupts or timers are used for button handling.
- LED states are mutually exclusive except for the combined pattern.
