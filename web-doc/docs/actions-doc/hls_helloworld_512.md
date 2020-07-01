# hls_helloworld_512
High Level Synthesis example.

It consists in exchanging a text file with the server host memory and mnipulate it in hardware to change the case and return the result into server host memory.

Inherited from SNAP1/2 512bit wide bus example, it keeps the same bus width at the cost of hardware AXI converter to use the OC 1024 bit wide bus.

It shows the smooth transition from CAPI1/2 design to OC.

