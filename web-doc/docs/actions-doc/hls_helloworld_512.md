# hls_helloworld_512
## Code location:

Code can be found at:[https://github.com/OpenCAPI/oc-accel/blob/master/actions/hls_helloworld_512/](https://github.com/OpenCAPI/oc-accel/blob/master/actions/hls_helloworld_512/) 

## In short:

This is a basic High Level Synthesis example.

It consists in exchanging a text file with the server host memory and manipulate it in hardware to change the case and return the result into server host memory.

Inherited from SNAP1/2 512bit wide bus example, it keeps the same bus width at the cost of hardware AXI converter to use the OC 1024 bit wide bus.

It shows the smooth transition from CAPI1/2 design to OC.

It can be checked in /action.Kconfig file that the ACTION_HALF_WIDTH bloc is set for this example, so the interface will implement the half width converter.

This allows older actions to be converted in a snap at the cost of lower performance.

