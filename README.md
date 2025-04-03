# FPGA Reliability Tips
  1. **Identify the Safety Integrity Level(SIL):**\
Determine the appropriate SIL for your project by consulting
high-level project documents that specify these requirements. SIL levels
range from 1 to 4, and there is a correlation between the SIL level, the
required Mean Time Between Failures (MTBF), and system availability.
This should be calculated in the high-level documents.
  2. **Establish Your FPGA Design Flow:**\
Define the specific FPGA design flow for your
project. Tutorials like those available
[here](https://xilinx.github.io/xup_fpga_vivado_flow/index.html) or
from Xilinx UG892 and UG888 can guide you through the design flow.
  3. **Use ECC Modules for DRAM:**\
If your design utilises DRAM, ensure
that you incorporate Error-Correcting Code (ECC) modules, which include
an additional memory chip for error checking.
  4. **Choosing the right memory blocks and SSD Chips:**\
Verify that the manufacturers of your
target memory and SSD components have screening processes to identify
chips that may fail during the early life failure period. They may
employ Test During Burn-In (TDBI) methods for this purpose.
  5. **Implement Redundancy for Safety-Critical logics:**\
Consider using double or triple logic redundancy based on the SIL level. De Morgan's
law will help you a lot in creating redundant modules. Keep in mind that
logic-level redundancy does not impact system availability since it
operates at the chip level. For improved system availability, consider
redundancy at the board level and how control is managed if the master
board fails.
  6. **Manage Latency in Design:**\
In latency-critical designs, avoid excessive synchronising registers, utilise more
asynchronous logic design. Asynchrnous approach, however, can increase
the risk of glitches, bus skews, and timing issues. Seek solutions to
ensure uniform propagation time across all logic paths. For
safety-critical designs where latency is less of a concern, a sequential
approach may be more effective but may result in slower processing.
  7. **Address Metastability Effects:**\
Metastability has an exponential effect on MTBF. Generally, FPGAs with faster transistors in their fabric
enhance MTBF. This is particularly true for technologies at or above the
90-nm fabrication process, where supply voltages can exceed 1V. In FPGA
with more advanced chip fabrication, the supply voltage drops below 1V,
bringing the metastability voltage (half of the supply voltage) close to
the threshold voltage of transistors. This proximity results in a
reduced gain of the circuit, leading to longer setup times for
flip-flops and slower transitions out of metastability. Therefore, while
faster transistors may be present in advanced technology, the MTBF could
actually decrease. Fortunately, MTBF can be significantly improved
through techniques such as: - Using synchronisation registers when
crossing clock domains. - Synchronizing reset signals with the negative
edge of the chip's global clock. - Synchronizing inputs and outputs to
the FPGAs.
  8. **Design Finite State Machines Carefully:**\
Missed transitions or incorrect state assignments in Finite State Machines
(FSMs) can create vulnerabilities. Refer to this [article](http://www.sunburst-design.com/papers/CummingsSNUG2019SV_FSM1.pdf) for
best practices in FSM design.
  9. **Mitigate Ionising Radiation Effects:**\
In applications such as space, avionics, military, and
nuclear power plants, ionizing radiation can disrupt charge in storage
elements like configuration memories and registers, leading to Single
Event Upsets (SEUs). To mitigate SEUs, consider using radiation-hardened
FPGAs and boards. At the logic level, incorporate error detection and
correction, error-initiated reconfiguration, and watchdog timers.
Implement safe shutdown procedures in the event of error-initiated
reconfiguration.
  10. **Acknowledge Limitations of Simulation Tools:**\
Be aware that formal simulation and verification methods for
safety-critical FPGAs are not fully mature. Achieving 100% simulation
and verification is not practically feasible. Instead, aim for simpler
designs (less code), partition large designs, and consider
insertion-based verification methods.
  11.  **Consider logic isolation for seurity-critical FPGAs:**\
When designing security-critical FPGA systems, it is important to isolate sensitive
logics, such as encryption engines, from the rest of the logic within
the FPGA fabric. FPGA vendors provide isolation tools in their software
packages that can be used to enhance the security of these designs.
