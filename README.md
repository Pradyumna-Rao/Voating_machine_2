# 8051-Based Electronic Voting Machine

This project is a simple electronic voting machine designed using the AT89C51 (8051) microcontroller. The circuit was designed and simulated in Proteus, and the logic is implemented in 8051 Assembly language. The system allows users to cast votes for multiple candidates and view the results on a character LCD.

<img width="993" height="747" alt="voting_machine_sim" src="https://github.com/user-attachments/assets/60174648-1c10-4bd5-83c8-f9faa55b66a9" />


---

## Key Features

-   **Multi-Candidate Support:** Allows voting for up to four different candidates.
-   **LCD Interface:** Displays real-time vote counts and final results on a 16x2 character LCD.
-   **Simple Button Interface:** Uses simple push buttons for casting votes and viewing results.
-   **Secure & Simple:** The logic is self-contained within the microcontroller, making it a robust and straightforward system.

---

## Hardware Components

| Component | Purpose |
| :--- | :--- |
| **AT89C51 Microcontroller** | The central processing unit that runs the assembly code. |
| **16x2 Character LCD** | Displays voting information and results. |
| **Push Buttons** | Used as inputs for casting votes. |
| **10k Potentiometer** | Adjusts the contrast of the LCD screen. |
| **Resistors** | Used as pull-up resistors for the input ports. |
| **Crystal Oscillator** | Provides the clock signal for the microcontroller. |

---

## How It Works

1.  **Initialization:** When powered on, the system initializes the LCD and displays a welcome message.
2.  **Voting Phase:** Users can press the button corresponding to their chosen candidate. The microcontroller reads the button press from **Port 0**.
3.  **Vote Counting:** For each valid button press, the corresponding vote counter in the microcontroller's memory is incremented.
4.  **Displaying Results:** The LCD screen is continuously updated to show the current vote count for each candidate.

---

## How to Simulate

1.  Open the `.pdsprj` file in **Proteus**.
2.  Compile the `voting_machine.asm` file to generate a `.hex` file.
3.  Double-click the AT89C51 microcontroller in the Proteus schematic.
4.  Load the generated `.hex` file into the "Program File" field.
5.  Click **Run Simulation** to start the voting machine.

---

## License

This project is licensed under the **MIT License**. See the `LICENSE` file for more details.
