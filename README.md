**8-Bit SAR ADC Using Split Capacitive DAC and Dynamic Comparator**

**Overview**

This project presents the design and transistor-level implementation of an **8-bit Successive Approximation Register Analog-to-Digital Converter (SAR ADC)** using a **split capacitive digital-to-analog converter (CDAC)** and a **dynamic comparator**. The proposed architecture converts an analog input voltage into an 8-bit digital output through a successive approximation process. The design integrates analog and digital building blocks including the CDAC, dynamic comparator, SAR controller, SAR register, and shift register. The project is implemented and simulated at the transistor level using **Synopsys Custom Compiler** with **SAED 14 nm FinFET technology**.

**Project Objectives**

The main objectives of this project are:

* Design an 8-bit SAR ADC architecture.
* Implement a split capacitive DAC for analog voltage generation.
* Design a dynamic comparator for analog voltage comparison.
* Implement SAR control logic for sequential bit-by-bit conversion.
* Integrate the analog and digital blocks into a complete SAR ADC system.
* Verify the functionality through transient simulations.
* Evaluate conversion behavior, conversion time, power consumption, and CDAC efficiency.

**Proposed Architecture**

The proposed SAR ADC consists of the following major blocks:

                    +-------------------+
                    |   Analog Input    |
                    |       VIN         |
                    +---------+---------+
                              |
                              v
                    +-------------------+
                    |      Split CDAC   |
                    |   Generates VDAC  |
                    +---------+---------+
                              |
                              |
                 VIN ---------+--------- VDAC
                              |
                              v
                    +-------------------+
                    | Dynamic Comparator|
                    |  VIN vs VDAC       |
                    +---------+---------+
                              |
                              | COMP
                              v
                    +-------------------+
                    |   SAR Controller  |
                    | Bit-by-Bit Control|
                    +---------+---------+
                              |
                              v
                    +-------------------+
                    |    SAR Register   |
                    | Stores Decisions  |
                    +---------+---------+
                              |
                              v
                    +-------------------+
                    | 8-Bit Digital Out |
                    |     D7 - D0       |
                    +-------------------+

**System Blocks**

**1. Split Capacitive DAC**

The Split Capacitive DAC generates the analog comparison voltage \(V_{DAC}\) based on the current digital code stored in the SAR register.

Compared with a conventional binary-weighted capacitor array, a split CDAC architecture can reduce the required total capacitance and therefore potentially reduce:

* Capacitor area
* Switching energy
* Circuit loading

The CDAC output is connected directly to one input of the dynamic comparator.

**2. Dynamic Comparator**

The dynamic comparator compares:

VIN  ↔  VDAC

Based on the comparison result, the comparator generates a digital decision signal. The SAR control logic uses the output to determine whether the currently tested bit should be retained as logic `1` or cleared to logic `0`. The comparator operates dynamically and avoids the requirement for continuous static bias current during the comparison operation.

**3. SAR Controller**

The SAR controller manages the successive approximation process.

Its primary functions include:

* Selecting one bit at a time.
* Starting from the Most Significant Bit (MSB).
* Receiving the comparator decision.
* Updating the corresponding bit.
* Moving sequentially toward the Least Significant Bit (LSB).

The controller works together with the shift register and SAR register to perform the 8-bit conversion.

**4. Shift Register**

The shift register generates sequential timing signals for bit selection.

The conversion proceeds as:
D7 → D6 → D5 → D4 → D3 → D2 → D1 → D0

Only the currently active bit is processed during each approximation cycle.

**5. SAR Register**

The SAR register stores the successive approximation decisions.

For each conversion cycle:

1. A trial bit is set.
2. The CDAC generates the corresponding \(V_{DAC}\).
3. The comparator compares \(V_{IN}\) and \(V_{DAC}\).
4. The bit is either retained or cleared.
5. The controller moves to the next bit.

After eight comparison cycles, the SAR register contains the final 8-bit digital representation of the analog input.


**SAR Conversion Process**

The complete conversion follows the steps below:

START
  |
  v
Sample Analog Input VIN
  |
  v
Initialize SAR Register
  |
  v
Set MSB = 1
  |
  v
CDAC Generates VDAC
  |
  v
Comparator Compares VIN and VDAC
  |
  +---- VIN >= VDAC ----> Keep Bit = 1
  |
  +---- VIN < VDAC -----> Clear Bit = 0
  |
  v
Move to Next Bit
  |
  v
Repeat Until D0
  |
  v
Conversion Complete
  |
  v
8-Bit Digital Output


# Example Conversion

Assuming:

* Resolution: **8-bit**
* Reference voltage: **1.5 V**
* Input voltage: **1.0 V**

The ideal output code is approximately:

VIN = 1.0 V

Digital Output ≈ 170

Binary Output = 10101010


Therefore:

D7 D6 D5 D4 D3 D2 D1 D0

 1  0  1  0  1  0  1  0

Note: The exact transistor-level output may vary depending on the implemented CDAC architecture, comparator behavior, reference voltage, timing, and non-ideal circuit effects.

**Simulation Environment**

| Parameter        | Value                    |
| ---------------- | ------------------------ |
| Technology       | SAED 14 nm FinFET        |
| Design Tool      | Synopsys Custom Compiler |
| Simulation Type  | Transient Analysis       |
| ADC Architecture | SAR ADC                  |
| Resolution       | 8-bit                    |
| Supply Voltage   | 1.5 V                    |
| DAC Architecture | Split Capacitive DAC     |
| Comparator       | Dynamic Comparator       |


**Simulation Results**

The following blocks have been individually designed and verified through simulation:

* Dynamic Comparator
* Split Capacitive DAC
* Shift Register
* SAR Register
* SAR Controller
* Top-Level SAR ADC Integration

Simulation waveforms demonstrate the operation of the individual circuit blocks and the integrated conversion architecture.


**Performance Analysis**

The project focuses on the following performance evaluations:

**1. Functional Verification**

Verification of the complete conversion path:

VIN
 ↓
CDAC
 ↓
Comparator
 ↓
SAR Controller
 ↓
SAR Register
 ↓
D7–D0

**2. Transfer Characteristic**

The relationship between analog input voltage and the generated digital output code is evaluated.

Expected behavior:

Increasing VIN
      ↓
Increasing Digital Code

The transfer characteristic is expected to exhibit an approximately staircase-shaped response.


**3. Conversion Time**

The conversion time is measured from the start of the SAR conversion until the final 8-bit output becomes stable.
For an 8-bit SAR ADC, the conversion requires sequential bit decisions from:

MSB → LSB

The exact conversion time is obtained from transient simulation.

**4. Power Consumption**

Power consumption is evaluated for:

* Dynamic Comparator
* Complete SAR ADC system

The final values are obtained from transistor-level simulation.


**5. Split CDAC Analysis**

The split CDAC architecture is compared with a conventional binary-weighted capacitor array based on:

* Capacitor configuration
* Total capacitance
* Potential area reduction
* Switching behavior

This comparison forms one of the primary architectural contributions of the project.

**Key Contributions**

The major contributions of this work include:

**Reduced-Capacitance DAC Architecture**

A split capacitive DAC architecture is implemented as an alternative to a conventional binary-weighted capacitor array.

**Complete Transistor-Level Integration**

The analog and digital blocks are integrated to form a complete 8-bit SAR ADC architecture.

**Dynamic Comparator Integration**

A dynamic comparator is used for the successive approximation process.

**Modular SAR Control**

The SAR control architecture combines sequential bit selection using a shift register with storage of approximation decisions in the SAR register.

**Performance Evaluation**

The design is evaluated using:

* Functional verification
* Input voltage versus digital output analysis
* Conversion time
* Power consumption
* Split CDAC comparison


**Future Work**

The current work can be extended by:

* Performing detailed power optimization.
* Measuring DNL and INL.
* Performing FFT-based dynamic analysis.
* Evaluating SNDR and ENOB.
* Performing PVT and Monte Carlo analysis.
* Optimizing the CDAC switching architecture.
* Extending the architecture to higher resolution.
* Performing layout design and post-layout simulation.

**Conclusion**

An 8-bit SAR ADC architecture based on a split capacitive DAC and dynamic comparator has been designed and integrated at the transistor level. The system performs successive approximation by comparing the input voltage with the DAC-generated voltage and sequentially updating the SAR register from MSB to LSB.The modular architecture combines analog and digital circuit blocks to achieve complete analog-to-digital conversion. The split CDAC architecture is investigated as a potential approach for reducing capacitor requirements compared with conventional binary-weighted CDAC structures.The design is implemented and verified using transistor-level simulations in Synopsys Custom Compiler with SAED 14 nm FinFET technology.


**License**

This project is currently intended for **Cadence academic and research purposes**.
