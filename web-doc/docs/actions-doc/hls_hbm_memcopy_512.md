# hls_hbm_memcopy_512
## Code location:

Code can be found at:[https://github.com/OpenCAPI/oc-accel/blob/master/actions/hls_hbm_memcopy_512/](https://github.com/OpenCAPI/oc-accel/blob/master/actions/hls_hbm_memcopy_512/) 

## Overview:

This is an action using HLS, 512 bits bus wide towards OpenCAPI interface which is 1024 bits wide.

To achieve this a 1024 to 512b converter is introduced, as P9 OpenCAPI uses a 1024 bits wide bus.

It can be checked in /action.Kconfig file that the ACTION_HALF_WIDTH bloc is set for this example, so the interface will implement the half width converter.

This allows older actions to be converted in a snap at the cost of lower performance.

This generic test can be also used to evaluate the throughput to/from FPGA and LCL memories (local can be DDR or HBM depending on cards used).

It reports bandwidth of:

* Host -> FPGA_RAM
* FPGA_RAM -> Host
* FPGA (LCL -> RAM)
* FPGA (RAM -> LCL)

## hw_test

Example on IC922 with a OC-AD9V3 card:

```
$ cd actions/hls_memcopy_512/tests
$ sudo ./hw_throughput_test.sh -dINCR
```

```
Build Date:  [00000008] 0000202009150920
+-------------------------------------------------------------------------------+
|            OC-Accel hls_memcopy_512  Throughput (MBytes/s)                    |
+-------------------------------------------------------------------------------+
+------------LCL stands for DDR or HBM memory accordingto hardware--------------+

       bytes   Host->FPGA_RAM   FPGA_RAM->Host  FPGA(LCL->BRAM)  FPGA(BRAM->LCL)
 -------------------------------------------------------------------------------
```

         512           10.240           10.240           10.449           10.449
        1024            1.497           20.480           21.333           25.600
        2048           40.960           41.796           42.667           41.796
        4096           81.920           81.920           83.592           83.592
        8192          132.129           11.855           11.872           11.977
       16384           23.918          321.255           12.319           12.319
       32768           24.582           46.946           47.628           47.628
       65536           94.432           95.394           94.980           94.980
      131072          188.593          189.959          187.782          186.979
      262144          370.260          366.635          372.364          371.309
      524288          717.220          729.191          718.203          717.220
     1048576         1354.749         1361.787         1353.001         1476.868
     2097152         2621.440         2427.259         2441.388         2467.238
     4194304         4084.035         2523.649         2520.615         2593.880
     8388608         4167.217         4120.141         4158.953         4136.394
    16777216         6028.464         8140.328         7084.973         8101.022
    33554432         9877.666         8212.049         7983.448         8152.194
    67108864         9884.941         9565.117         9774.084         9765.551
    134217728        10507.925        10888.110        10844.124        10824.883
    268435456        12041.244        11543.625        11482.887        11460.336
 To get the best results, it may be useful to ensure you have the ocapi link attached to the core where the program is executed. If you have 2 nodes (check with numactl -s), you can try the 4 following combinations:

```
sudo numactl -m0 -N0 ./oc-accel/actions/hls_memcopy_1024/tests/hw_throughput_test.sh -d INCR
sudo numactl -m8 -N0 ./oc-accel/actions/hls_memcopy_1024/tests/hw_throughput_test.sh -d INCR
sudo numactl -m0 -N8 ./oc-accel/actions/hls_memcopy_1024/tests/hw_throughput_test.sh -d INCR
sudo numactl -m8 -N8 ./oc-accel/actions/hls_memcopy_1024/tests/hw_throughput_test.sh -d INCR
```

Note:

"-m" stands for memory: allocate selected memory  from  nodes 

"-N" stands for nodes: execute command on the CPUs of selected nodes