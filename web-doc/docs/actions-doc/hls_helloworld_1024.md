# hls_helloworld_1024
High Level Synthesis example.

It consists in exchanging a text file with the server host memory and manipulate it in hardware to change the case and return the result into server host memory.

Inherited from SNAP1/2 512bit wide bus example, it has been changed to use a 1024 bit wide bus.

This allows to remove the bus converter and uses direct 1024 bits AXI bus.

It can be checked in /action.Kconfig file that the ACTION_HALF_WIDTH bloc is not used for this example, so the interface uses the OpenCAPI 1024 bit bus.

