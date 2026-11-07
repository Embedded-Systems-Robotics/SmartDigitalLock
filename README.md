# Smart Digital Lock System

A microcontroller-based digital access-control system developed as the final project for a Microcontrollers course. The system uses a password-based authentication process to control access to a simulated door, provides visual feedback through LEDs and an LCD, limits incorrect password attempts, and allows the authorized user to change the password.

## Table of Contents

- [Project Overview](#project-overview)
- [Objectives](#objectives)
- [System Features](#system-features)
- [System Operation](#system-operation)
  - [Entering the Password](#1-entering-the-password)
  - [Correct Password](#2-correct-password)
  - [Incorrect Password and Alarm](#3-incorrect-password-and-alarm)
  - [Changing the Password](#4-changing-the-password)
- [Password Rules](#password-rules)
- [Inputs and Outputs](#inputs-and-outputs)
- [System Workflow](#system-workflow)
- [Repository Contents](#repository-contents)
- [Simulation and Project Files](#simulation-and-project-files)
- [Possible Applications](#possible-applications)
- [Limitations and Security Notes](#limitations-and-security-notes)
- [Author](#author)

## Project Overview

The Smart Digital Lock System is an electronic access-control project designed to demonstrate how a microcontroller can manage password authentication, access decisions, user feedback, and basic security handling.

A user enters a numeric password to request access. When the entered password matches the stored password, the system grants access. In this project, the door-opening action is represented by a **green LED**, and the LCD displays a successful-access message.

If the entered password is incorrect, the system allows a maximum of **three attempts**. Once all three attempts have been used without entering the correct password, the system activates an alarm represented by a **flashing red LED** and displays a failure message on the LCD.

The system also provides a password-change function. Before setting a new password, the user must authenticate using the existing password. The new password must consist of exactly **four numeric digits**.

The repository includes the project files and a visual representation of the smart digital lock system.

![Smart Digital Lock System](smart%20digital%20lock%20system%20project.png)

## Objectives

The main objectives of this project are to:

- Implement password-based access control using a microcontroller.
- Validate a user-entered password against the stored password.
- Grant or deny access according to the authentication result.
- Limit authentication to three incorrect attempts.
- Provide clear visual status indication using LEDs.
- Display system messages and authentication results on an LCD.
- Trigger an alarm condition after three consecutive unsuccessful attempts.
- Allow an authorized user to change the existing password.
- Enforce a fixed four-digit format for the new password.

## System Features

| Feature | Description |
|---|---|
| Password authentication | Compares the entered password with the currently stored password. |
| Access granted indication | Turns on the green LED and displays a successful-access message on the LCD. |
| Access denied handling | Uses the available attempts to allow the user to retry an incorrect password. |
| Three-attempt limit | Allows up to three unsuccessful password attempts before activating the alarm state. |
| Alarm indication | Flashes the red LED after all three attempts have been used unsuccessfully. |
| LCD feedback | Displays messages that communicate the system state and authentication result. |
| Password modification | Allows the password to be changed after the existing password has been entered correctly. |
| Four-digit password validation | Accepts a new password only when it contains exactly four numeric digits. |

## System Operation

### 1. Entering the Password

The system prompts the user to enter the password through the system's password-input interface.

The entered value is checked against the currently stored password. The result determines whether access is granted or denied.

### 2. Correct Password

If the entered password matches the stored password:

1. The system recognizes the user as authenticated.
2. The door-opening state is represented by turning on the **green LED**.
3. The LCD displays a successful-passing message.
4. The system indicates that access has been granted.

### 3. Incorrect Password and Alarm

If the entered password does not match the stored password:

1. The system registers an unsuccessful attempt.
2. One attempt is deducted from the maximum allowance of three attempts.
3. The user may try entering the password again while attempts remain.
4. If all three attempts are incorrect, the system enters its alarm state.
5. The **red LED starts flashing** to indicate the alarm condition.
6. The LCD displays a failure message indicating that the password was not identified correctly.

This mechanism prevents unlimited password-entry attempts and provides a visible indication when the allowed attempts have been exhausted.

### 4. Changing the Password

The system allows the user to change the currently stored password, but the operation first requires authentication with the existing password.

The password-change procedure is as follows:

1. The user selects or initiates the password-change operation.
2. The system requests the current password.
3. The user enters the existing password.
4. If the entered password is incorrect, the system deducts one attempt from the three-attempt allowance.
5. If the existing password is entered correctly, the system allows the user to provide a new password.
6. The new password is validated to ensure that it consists of exactly four numeric digits.
7. Once the new password satisfies the required format, it becomes the password used for subsequent authentication.

**Important:** The existing password must be entered correctly before a new password can be set. An incorrect old password is treated as a failed authentication attempt.

## Password Rules

The password follows these requirements:

- It must contain numeric digits only.
- It must contain exactly four digits.
- Passwords containing fewer than four digits are invalid.
- Passwords containing more than four digits are invalid.
- Changing the password requires successful verification of the existing password.
- Incorrect entry of the existing password consumes one of the three permitted attempts.

Examples of the required format:

| Example | Validity |
|---|---|
| `1234` | Valid format |
| `0000` | Valid format |
| `123` | Invalid — fewer than four digits |
| `12345` | Invalid — more than four digits |
| `12A4` | Invalid — contains a non-numeric character |

These examples illustrate the password format only; they do not indicate the actual password configured in the project.

## Inputs and Outputs

The system's main interaction and feedback elements are summarized below.

| Element | Role |
|---|---|
| Password-input interface | Receives the password entered by the user. |
| Password-change selection | Initiates the process of changing the stored password. |
| LCD | Displays prompts, successful-access feedback, and failure or alarm messages. |
| Green LED | Represents the door-unlocked or access-granted state. |
| Red LED | Flashes to represent the alarm state after three unsuccessful attempts. |
| Microcontroller | Executes the authentication logic, manages the attempt limit, validates password changes, and controls the outputs. |

The door is represented by the green LED in the project demonstration; the LED is a status indicator rather than a physical door actuator.

## System Workflow

The following flow summarizes the main authentication process:

```text
                 +----------------------+
                 |       Start          |
                 +----------+-----------+
                            |
                            v
                 +----------------------+
                 | Request password     |
                 | from the user        |
                 +----------+-----------+
                            |
                            v
                 +----------------------+
                 | Compare entered      |
                 | password with stored |
                 | password             |
                 +----------+-----------+
                            |
                  +---------+---------+
                  |                   |
               Correct             Incorrect
                  |                   |
                  v                   v
       +-------------------+  +----------------------+
       | Turn ON green LED |  | Deduct one attempt   |
       | Display successful|  +----------+-----------+
       | access message    |             |
       +-------------------+             v
                              +-----------------------+
                              | Are all three attempts|
                              | used?                 |
                              +----------+------------+
                                         |
                               +---------+---------+
                               |                   |
                              No                  Yes
                               |                   |
                               v                   v
                     +------------------+  +------------------+
                     | Allow another    |  | Flash red LED    |
                     | password attempt |  | Display failure  |
                     +------------------+  | / alarm message  |
                                           +------------------+
```

The password-change operation follows a separate authentication path: the system verifies the old password first, and only then accepts a new password that satisfies the four-digit requirement.

## Repository Contents

The repository contains the project and simulation-related files used for the Smart Digital Lock System.

| File or directory | Purpose |
|---|---|
| `final.X/` | Microcontroller firmware project directory. |
| `FinalProject.pdsprj` | Proteus project file associated with the circuit/simulation. |
| `Project Backups/` | Backup files associated with the project. |
| `Backup Of FinalProject.pdsbak` | Proteus project backup. |
| `Last Loaded FinalProject.pdsbak` | Additional Proteus backup file. |
| `final.X` | Project-related file or entry shown in the repository. |
| `Makefile` | Build configuration generated for the firmware project. |
| `main.err` | Compiler output or warning log from a firmware build. |
| `smart digital lock system project.png` | Project image included in the repository. |

File availability and exact directory contents may change as the repository is updated. Open the relevant project in its corresponding development or simulation environment to inspect the complete implementation.

## Simulation and Project Files

The repository includes a Proteus project file (`FinalProject.pdsprj`) and a microcontroller firmware project directory (`final.X`).

To inspect the project:

1. Clone or download this repository.
2. Open the firmware project using the compatible MPLAB X IDE installation, if the project configuration is supported by your installed version.
3. Review the project configuration and source files.
4. Open `FinalProject.pdsprj` in Proteus to inspect the circuit and simulation setup.
5. Ensure that the firmware build output is correctly associated with the microcontroller component in the Proteus design before running the simulation.

The exact microcontroller model, compiler version, required libraries, and simulation configuration should be taken from the firmware source and project settings. They are not specified here to avoid documenting unverified hardware or software details.

## Possible Applications

The concepts demonstrated in this project can be applied to:

- Electronic door-lock prototypes.
- Password-protected storage compartments.
- Access-control demonstrations.
- Microcontroller-based security systems.
- Educational projects involving digital input, output control, and embedded decision-making.

## Limitations and Security Notes

This project is an educational microcontroller-based prototype. The green LED represents an unlocked door, and the red LED represents an alarm condition. The repository description does not establish that the system controls a production door lock or includes physical security hardware.

For deployment in a real access-control environment, additional engineering would be required, including secure password storage, protection against power-loss or reset-based attempt bypass, a properly rated lock actuator and driver circuit, and appropriate fail-safe behavior.

## Author

**Lawrence Abou Karroum**

Developed as a final project for a **Microcontrollers course**.

---

*This project demonstrates the integration of embedded programming, password validation, access-control logic, LCD messaging, LED status indication, and basic alarm handling in a microcontroller-based digital lock system.*
