# Electronic Gaspedal ECU (AUTOSAR RTE Based)

This project implements an **Electronic Gaspedal ECU** following the **AUTOSAR Runtime Environment (RTE)** concept.  
It demonstrates message-based communication between runnables, cyclic/event activations, watchdog-based supervision, and error handling.

---

## Project Overview
- **Architecture:** AUTOSAR-style with RTE separating application software and basic software.  
- **Communication:** Runnables communicate only through RTE signals (no direct hardware/OS calls).  
- **Error Handling:** Includes driver error codes, signal status monitoring, and signal age checks.  
- **Supervision:** Alive watchdog and deadline monitoring to ensure timing correctness.  

---

## System Functionality
1. **Joystick Input**  
   - `run_readJoystick()` executes every **10 ms** and updates the joystick signal via `pullPort()`.

2. **Control Calculation**  
   - `run_calcControl()` is triggered by joystick updates.  
   - If joystick > 0 → `speed = 2 × joystick value`.  
   - If joystick ≤ 0 → `engine = 0`.  

3. **Engine Update**  
   - `run_setEngine()` executes every **100 ms**.  
   - Copies the speed signal to the engine output if it’s valid, otherwise sets engine = 0.  

4. **Brake Light Control**  
   - `run_setBrakeLight()` checks the speed signal.  
   - If speed = 0 → Brake light **ON**.  
   - If speed > 0 → Brake light **OFF**.  

---

## Error Handling & Supervision
- **Driver Errors:** `pushPort()` and `pullPort()` return error codes.  
- **Signal Status:** Each signal carries explicit/implicit error codes.  
- **Signal Age:** Ensures data freshness before use.  
- **Watchdog (Alive Monitoring):** Verifies that all runnables report activity.  
- **Deadline Monitoring:** Detects runnables exceeding execution time limits.  

---

![Electronic Gaspedal ECU](/Electronic_Gaspedal/Images/ElectronicGaspedalECU.png)

## Setup & Usage
1. Install [Python](https://www.python.org/downloads/) for RTE Generator.  
2. Use the provided RTE generator tool to create `rte` sources.  
3. Import into an **Erika OS project** with the following tasks:  
   - `tsk_Init()`  
   - `tsk_Background()`  
   - Application tasks for cyclic & event-driven runnables.  
4. Compile and run on the **PSoC/embedded platform**.  
5. Observe outputs:  
   - **Engine control** → Green LED (PWM).  
   - **Brake light** → Red LED (GPIO).  

---

## Extensions (Optional)
- Add a **brake pedal ISR**.  
- Integrate a **TFT display** for system data.  
- Introduce **critical sections** for safe signal handling.  

---

## References
For more details on system design, iterations, and error handling, refer to the project report:  
👉 [WorkBook](https://github.com/DheerajSwaroopSaligramaMahesh/Embedded_Architectures_Applications-ElectronicGaspedal/blob/main/Eletronic_Gaspedal_WorkBook.pdf)

---
