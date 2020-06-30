# System Firmware Setup

The system should be updated to the latest OP930 firmware release.

This firmware package and instructions for applying it, can be found at [IBM FixCentral] under "8335-GTX "designation.

| Setup             | Command to run                        | Recommended Release                               |
| ----------------- | ------------------------------------- | ------------------------------------------------- |
| OS type and level | cat /etc/os-release or lsb_release -a | Ubunutu 18.04 or 20.04 LTS / RH 7.6-ALT or RH 8.2 |
| RW level          | sudo lsmcode                          | FW920.20 / BMC2.04                                |

[IBM FixCentral]: https://www.ibm.com/support/fixcentral

Note: When using RH7.6 -ALT release is required to support OpenCAPI on P9