# hls_hbm_memcopy_1024
This is an action using HLS, 1024b and HBM memory found on fpga used on cards like OC-9Hx. 1024b is the optimum configuration as P9 OpenCAPI uses a 1024 bits wide bus.

It can be checked in *action/Kconfig* file that the ACTION_HALF_WIDTH bloc is not used for this example, so the interface uses the OpenCAPI 1024 bit bus.

## Note on HBM usage in OC-ACCEL:

OC-9H3 and OC-9H7 cards for example each contain 8GB of HBM reachable through up to 32 AXI buses connected to 256MB HBM memories. 
The test done here exercises only 1 HBM.

As all these HBM can be accessed independently in parallel, this means that the overall throughout of the 8GB of HBM can be multiplied by 32. 

The HBM can also be configured in different manners (One 8GB HBM, multiple access to 256MB modules,...).

The default choice in Kconfig menu is 12 HBM memories set in parallel.
Reducing to 1 memory can easily be done by modifying at the same time:

- in the Kconfig menu
- in the hls_hbm_memcopy_1024.cpp code changing the parameter #define HBM_AXI_IF_NB to 1.

## Bandwidth Evaluation Test

This generic test can be also used to evaluate the throughput to/from FPGA and LCL memories (local can be DDR or HBM depending on cards used).

It reports bandwidth of:

* Host -> FPGA_RAM
* FPGA_RAM -> Host
* FPGA (HBM -> RAM)
* FPGA (RAM -> HBM)

## hw_throughput_test

Example on IC922 with a OC-AD9H3 card:

```
$ cd actions/hls_hbm_memcopy_1024/tests
$ sudo ./hw_throughput_test.sh -dINCR
```

    +-------------------------------------------------------------------------------+
    |            OC-Accel hls_hbm_memcopy_1024 Throughput (MBytes/s)                |
    +-------------------------------------------------------------------------------+
           bytes   Host->FPGA_RAM   FPGA_RAM->Host   FPGA(HBM->RAM)   FPGA(RAM->HBM)
     -------------------------------------------------------------------------------
             512            0.739            0.742            0.741            9.481
            1024           17.965           18.963            1.488            1.482
            2048            2.955            2.934            2.968            3.012
            4096            5.945            5.885            5.902            5.911
            8192           11.924           11.907           11.703          148.945
           16384          292.571          292.571          227.556           23.406
           32768           46.612           47.080           46.217          555.390
           65536          897.753         1110.780         1024.000          101.292
          131072          186.447          185.918          185.918          199.805
          262144          403.298          366.635          359.594         2759.411
          524288         6393.756         5825.422          673.892          682.667
         1048576         1396.240         7231.559         1216.445         1299.351
         2097152        12409.183         8774.695         5475.593         5282.499
         4194304         4832.147         4185.932         2166.479         2075.361
         8388608         4639.717         4269.012         3063.772         4080.062
        16777216        10343.536         9805.503         5027.634         4178.634
        33554432        10485.760        10277.008         5053.378         4919.283
        67108864        13166.346        13560.086         5627.106         5445.822
       134217728        15080.644        16192.270         5969.477         5760.912
       268435456        16460.354        17956.750         6062.776         5929.392
       536870912        17014.893        19000.917
      1073741824        17391.630        19322.677
    ok
    Test OK
To get the best results, it may be useful to ensure you have the ocapi link attached to the core where the program is executed. If you have 2 nodes (check with numactl -s), you can try the 4 following combinations:

```
sudo numactl -m0 -N0 ./oc-accel/actions/hls_hbm_memcopy_1024/tests/hw_throughput_test.sh -d INCR
sudo numactl -m8 -N0 ./oc-accel/actions/hls_hbm_memcopy_1024/tests/hw_throughput_test.sh -d INCR
sudo numactl -m0 -N8 ./oc-accel/actions/hls_hbm_memcopy_1024/tests/hw_throughput_test.sh -d INCR
sudo numactl -m8 -N8 ./oc-accel/actions/hls_hbm_memcopy_1024/tests/hw_throughput_test.sh -d INCR
```

Note:

"-m" stands for memory: allocate selected memory  from  nodes 

"-N" stands for nodes: execute command on the CPUs of selected nodes

