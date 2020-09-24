# hls_memcopy_1024
This is an action using HLS, 1024b. This is the optimum configuration as P9 OpenCAPI uses a 1024 bits wide bus.

It can be checked in /action.Kconfig file that the ACTION_HALF_WIDTH bloc is not used for this example, so the interface uses the OpenCAPI 1024 bit bus.

This generic test can be also used to evaluate the throughput to/from FPGA and LCL memories (local can be DDR or HBM depending on cards used).

It reports bandwidth of:

* Host -> FPGA_RAM
* FPGA_RAM -> Host
* FPGA (DDR -> RAM)
* FPGA (RAM -> DDR)

## hw_test

Example on IC922 with a OC-AD9V3 card:

```
$ cd actions/hls_memcopy_1024/tests
$ sudo ./hw_throughput_test.sh -dINCR
```

```
Build Date:  [00000008] 0000202009150921
+-------------------------------------------------------------------------------+
|            OC-Accel hls_memcopy_1024 Throughput (MBytes/s)                    |
+-------------------------------------------------------------------------------+
+------------LCL stands for DDR or HBM memory accordingto hardware--------------+

       bytes   Host->FPGA_RAM   FPGA_RAM->Host  FPGA(LCL->BRAM)  FPGA(BRAM->LCL)
 -------------------------------------------------------------------------------
```

         512            8.828           10.240           10.240           11.907
        1024           23.814           20.480            1.484            1.476
        2048            3.225            2.926            2.985            2.985
        4096            5.971            6.554            6.491           80.314
        8192           11.924            6.192            6.141            6.466
       16384           12.337           12.911           12.921           12.870
       32768           24.768           24.693           24.787           25.863
       65536           49.461           95.118           92.959          102.721
      131072          204.800          188.052          188.322           97.815
      262144          195.484          203.055          195.193          202.741
      524288          404.856          399.305          380.194          383.251
     1048576          759.838         1351.258          775.574          741.567
     2097152         1457.368         1408.430         1402.777         1391.607
     4194304         2720.042         4185.932         4096.000         4096.000
     8388608         7483.147         6732.430         6091.945         6061.133
    16777216         7584.637        10292.771         6193.140         6181.730
    33554432        10525.230        13584.790         9683.819         9703.422
    67108864        13899.930        16615.218        10789.206        10764.977
    134217728        17563.168        16927.447        11443.237        11411.131
    268435456        17688.156        20650.470        11786.409        11749.265
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

