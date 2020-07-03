# POWER System Firmware Setup

For the development any X86 systeme supporting Vivado suits well.

To to benefit from the unique "Coherent Accelerator Processor Interface" version 3 named OpenCAPI technology user needs to deploy on a POWER server hosting a P9 processor.

The system should be updated to the latest OP940 firmware release.

This firmware package and instructions for applying it, can be found at [IBM FixCentral] under "8335-GTX" designation.

| Setup             | Command to run                        | Recommended Release                                  |
| ----------------- | ------------------------------------- | ---------------------------------------------------- |
| OS type and level | cat /etc/os-release or lsb_release -a | Ubuntu 18.04 or **20.04** LTS / RH 7.6-ALT or RH 8.2 |
| FW level          | sudo lsmcode                          | FW920.20 / BMC2.04                                   |

[IBM FixCentral]: https://www.ibm.com/support/fixcentral

Note: When using RH7.6 -ALT release is required to support OpenCAPI on P9

