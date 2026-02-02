# XYZ Arithmetic Machine (Logisim)

## Project Description

**XYZ Arithmetic Machine** is a synchronous sequential machine implemented in **Logisim**. The system computes the following expression:

```
f(x) = 1 - 3x + y + z
```

The input data are provided **sequentially** in the following order:

```
x → y → z → t
```

The machine is controlled using two external control signals and two status indicators (LEDs):

* **ST (START)** – starts the computation sequence
* **NR (NUMBER READY)** – indicates that a valid input value is present on the data input
* **RDY (Ready for Input)** – LED indicating that the machine is ready to accept the next input value
* **RR (Result Ready)** – LED indicating that the final result is ready and valid

The project was implemented in two variants:

1. **Logic-gates-based implementation** (FSM + datapath)
2. **ROM-controlled implementation** (microprogrammed control unit)
3. **Counter**

The main goal of the project was to practice:

* finite state machine design,
* synchronization of external control signals,
* sequential data processing,
* datapath control using explicit control signals,
* using ROM as a microprogrammed control unit.

---

## Functionality Overview

The machine operates as follows:

1. Waits for **ST = 1** and **PUSH OF A BUTTON** to begin execution
2. Accepts input values (**x, y, z, t**) one by one when **NR = 1** and **RDY = 1**
3. Stores each value in internal registers
4. Performs arithmetic operations step by step
5. Signals readiness for the next input using **RDY**
6. After completing all operations, asserts **RR = 1** to indicate a valid result (diode blinks)

---

## Interface
=======
## Counter Overview:
**ab:**
**00** - counter mod 4 up
**01** - counter mod 5 up
**10** - undefined behaviour (purposefully)
**11** - counter mid 7 down

### Inputs

| Signal       | Description                        |
| ------------ | ---------------------------------- |
| `x, y, z, t` | Input data (provided sequentially) |
| `ST`         | Start signal                       |
| `NR`         | NUMBER READY – input data valid    |
| `CLK`        | System clock                       |
| `RST`        | Reset                              |

### Outputs / Indicators

| Signal  | Description                                     |
| ------- | ----------------------------------------------- |
| `RDY`   | Ready for Input – machine can accept next value |
| `RR`    | Result Ready – computation finished             |
| `OUTPUT`| Computation result                              |

---

## High-Level Operation

1. **ST** initializes the machine to the idle state
2. **NR** sygnalises redyness for value input
3. Arithmetic operations (addition, subtraction, multiplication) are executed sequentially
4. The control unit:
   * in version 1: implemented as an FSM using combinational logic
   * in version 2: implemented as a microprogram stored in ROM
5. When computation is complete, **RR = 1** is asserted and the result is stable at `OUTPUT`

---

## ROM-Based Version

In the ROM-controlled variant:

* Machine states are encoded as ROM addresses
* The ROM outputs control signals for the datapath
* State transitions depend on:

  * control inputs (`ST`, `NR`)
  * internal condition flags (e.g. zero, sign)

This approach simplifies the control logic and treats the ROM as a **microprogrammed controller**.

---

## Project Structure

```
.
├── sequencer_logic_gates.circ    # FSM implemented using logic gates
├── flowchart_logisim.png         # ASM / control flow diagram
└── README.md
```

---

## How to Run

1. Install **Logisim** or **Logisim Evolution**
2. Open the selected `.circ` file
3. Set `RST = 1`, then return it to `0`
4. Set `ST = 1` to start the machine
5. Provide inputs (`x, y, z, t`) sequentially, asserting `NR = 1` for each value
6. Observe:

   * **RDY LED** – readiness for next input
   * **RR LED** – result availability
   * `OUTPUT` – final computation result

---

## Authors

Project developed jointly as part of digital logic / computer architecture coursework with Kacper Gramaszek (STUD).

---

This project is provided for educational purposes.

---

## Notes

* The design is not optimized for minimal cycle count
* Priority was given to **clarity of control** and correctness
* The ASM / flowchart diagram is included as `flowchart_logisim.png`
