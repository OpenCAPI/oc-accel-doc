# POWER System Firmware Setup

For the development any X86 system supporting Vivado suits well.

To benefit from the unique "Coherent Accelerator Processor Interface" version 3 named OpenCAPI technology user needs to deploy on a POWER server hosting a P9 processor.

The system should be updated to the latest OP940 firmware release.

This firmware package and instructions for applying it, can be found at [IBM FixCentral] :

- under "8335-GTX" (AC922) designation.
- under "9183-22X" (IC922) designation.



| Setup             | Command to run                        | Recommended Release                                          |
| ----------------- | ------------------------------------- | ------------------------------------------------------------ |
| OS type and level | cat /etc/os-release or lsb_release -a | Ubuntu **20.04** LTS / RHEL 8.2 (with patch of oc-reset to work) or **latest 8.3** |
| FW level          | sudo lsmcode                          | BMC ibm-op940.10 (AC922)<br />BMC ibm-op940.20 (IC922)       |

[IBM FixCentral]: https://www.ibm.com/support/fixcentral
