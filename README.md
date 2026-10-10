# Smart-Attendance-System-Using-RFID
# Smart Attendance System Using RFID

An RFID-based smart attendance system using an **ESP32-WROOM-32**, **MFRC522 RFID reader**, and **16×2 I2C LCD** to identify registered students, mark attendance, and provide instant feedback.

## Overview

Manual attendance can be time-consuming and prone to human error. This project demonstrates an automated attendance prototype that reads RFID cards, verifies registered students, prevents duplicate attendance marking during the current power session, and displays the result on an LCD.

## Objectives

- Automate attendance marking using RFID technology.
- Identify registered students using their RFID card UID.
- Display student names and attendance status on an LCD.
- Provide visual and audible feedback using an LED and buzzer.
- Display attendance information in the Arduino Serial Monitor.
- Provide a foundation for future timestamping and permanent storage.

## Components Required

| Component | Quantity |
|---|---:|
| ESP32-WROOM-32 development board | 1 |
| MFRC522 RFID reader module | 1 |
| Compatible RFID card/tag | 1 or more |
| 16×2 LCD with I2C module | 1 |
| Green LED | 1 |
| 220–330 Ω resistor | 1 |
| Compatible buzzer | 1 |
| Breadboard and jumper wires | As required |
| USB cable and power supply | 1 |

## Pin Connections

### MFRC522 RFID Reader

| RC522 Pin | ESP32 Pin |
|---|---|
| SDA / SS | GPIO 5 |
| SCK | GPIO 18 |
| MOSI | GPIO 23 |
| MISO | GPIO 19 |
| RST | GPIO 4 |
| 3.3V | 3V3 |
| GND | GND |
| IRQ | Not connected |

### LCD, LED and Buzzer

| Component | ESP32 Connection |
|---|---|
| LCD SDA | GPIO 21 |
| LCD SCL | GPIO 22 |
| LCD VCC | Suitable supply for the module |
| LCD GND | GND |
| Green LED anode | GPIO 26 through a 220–330 Ω resistor |
| Green LED cathode | GND |
| Buzzer signal / positive | GPIO 25 |
| Buzzer negative | GND |

**Hardware safety:** Power the MFRC522 using 3.3 V only. If the LCD I2C module pulls its data lines up to 5 V, use a bidirectional logic-level shifter to protect the ESP32. Use a transistor driver if the buzzer exceeds the safe GPIO current.

## Software Requirements

- Arduino IDE
- ESP32 board support package
- `MFRC522` library
- `LiquidCrystal_I2C` library

The `SPI` and `Wire` libraries are included with the Arduino environment.

## Installation and Setup

1. Install Arduino IDE.
2. Install the ESP32 board support package.
3. Install the `MFRC522` and `LiquidCrystal_I2C` libraries using Library Manager.
4. Connect the hardware according to the pin tables.
5. Open the Arduino sketch in the repository.
6. Select **ESP32 Dev Module** under Tools → Board.
7. Select the correct COM port.
8. Upload the program.
9. Open Serial Monitor at **115200 baud**.
10. Scan an RFID card and copy its UID from the Serial Monitor.
11. Add the actual UID to the registered student list and upload the updated sketch.

## Registering Students

Student details are stored in the Arduino sketch.

Example:

```cpp
Student students[] = {
  {"A1 BA 9A 0A", "Arun"},
  {"12 34 56 78", "Priya"}
};
```

The UID `A1 BA 9A 0A` was observed during testing and is assigned to the example student `Arun`. Replace all other sample UIDs with the actual UIDs of your RFID cards.

## Working Principle

1. The ESP32 initializes the RFID reader, LCD, LED, and buzzer.
2. The student scans an RFID card.
3. The MFRC522 reads the card UID and sends it to the ESP32.
4. The ESP32 compares the UID with the registered student list.
5. If the UID is registered and has not already been marked, attendance is recorded in RAM.
6. The LCD displays the student's name and `Attendance OK`.
7. The LED and buzzer provide confirmation.
8. If the card is unknown, the LCD displays `Invalid Card` and `Not Registered`.
9. If the same card is scanned again during the current power session, the LCD displays `Already Marked`.

## Expected Output

| Condition | LCD Output | System Response |
|---|---|---|
| Registered card | Student name / Attendance OK | LED and short beep |
| Unregistered card | Invalid Card / Not Registered | Error beep |
| Duplicate scan | Already Marked / Student name | Short beep |
| Standby | RFID Attendance / Scan Your Card | Waits for a card |

## Testing

- **Firmware upload:** Arduino IDE reported `Hash of data verified`.
- **RFID reading:** The Serial Monitor displayed UID `A1 BA 9A 0A`.
- **Registration check:** The card initially returned `NOT REGISTERED` because its UID was not in the sample list.
- **Registration update:** The observed UID was added to the student list as `Arun`.
- **Final validation:** Confirm `Attendance OK`, LED/buzzer feedback, and duplicate-scan prevention on the assembled hardware.

## Current Limitations

- Attendance records are stored in RAM and disappear when the ESP32 restarts or loses power.
- The current version does not record the actual date and time.
- Student UIDs are maintained in the firmware.
- The fixed attendance array supports up to 100 unique entries per power session.
- RFID UID matching alone does not prevent all forms of card cloning or card sharing.

## Future Enhancements

- Add a DS3231 RTC module or Wi-Fi/NTP time synchronization.
- Store attendance records on an SD card in CSV format.
- Integrate a database for permanent attendance records.
- Develop a web-based attendance dashboard.
- Add student registration and attendance report generation.
- Improve authentication and administrative access controls.

## Applications

- Classroom attendance management
- College laboratory attendance
- Employee attendance prototypes
- Small institutional access and check-in systems

## Project Information

**Project Title:** Smart Attendance System Using RFID  
**Controller:** ESP32-WROOM-32  
**RFID Module:** MFRC522  
**Display:** 16×2 I2C LCD  
**Programming Language:** Embedded C++ (Arduino framework)

## Author

**Name:** Kathiravan V  
**Department:** Electronics and Communication Engineering (ECE)

## License

This project is intended for educational and demonstration purposes. Add a suitable open-source license if you intend to distribute or reuse the source code publicly.
