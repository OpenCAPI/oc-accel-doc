



# hdl_single_engine

A very simple Verilog written example to interface with AXI-lite slave and AXI-master and measure the bandwidth and latency.

## Code location:

Code can be found at:[https://github.com/OpenCAPI/oc-accel/blob/master/actions/hdl_single_engine/](https://github.com/OpenCAPI/oc-accel/blob/master/actions/hdl_single_engine/) 

## Latency Evaluation

Single Engine test offers a simple way to evaluate latency.

The goal is that the FPGA generates a sequence of Bytes reads or writes from/into server memory.

The routine launches a counter whose initial and end values can be read afterwards in dedicated registers.

Each counter increment represents 5ns.

A file is automatically written to store the initial and end values.

The difference represents a complete multiple words operation, so a short calculation is required to obtain the minimum time required to read/write 1 single Byte.



Here are examples of commands to evaluate latency on a OC-AD9H3 card in an IBM IC922 server:

#### Read of 4096B at default memory location provided by the server:

```
cd oc-accel/action/hdl_single_engine/sw
# make sure make apps has been run from your oc-accel directory
./hdl_single_engine -c 100 -w 0x00000601 -n1 -N0 -p 0x00001F07  -t 100000 -C5
cat file_rd_cycle
    arid,           rd_cmd,      rid,          rd_resp
       0,				 3,		   0,		   	   131
```

**Minimum latency  in this configuration is thus 485ns**

![Read_latency_measurement](./hdl_single_engine.assets/Read_latency_measurement.png)

#### Write of 4096B to default memory location provided by the server:

```
cd oc-accel/action/hdl_single_engine/sw
# make sure make apps has been run from your oc-accel directory
./hdl_single_engine -c 100 -w 0x00000601 -n 0 -N1 -p 0x00001F07  -t 100000 -C5
cat file_wr_cycle
 -- Counter values for different signals --
    awid,           wr_cmd,      bid,          wr_resp
       0,				 3,		   0,		   	   143
```

**Minimum latency in this configuration is thus 545ns**

![Write_latency_measurement](./hdl_single_engine.assets/Write_latency_measurement.png)



## Bandwidth Evaluation

Using following test commands allow to get a summary of R/W measured bandwidth:

```
cd oc-accel/action/hdl_single_engine/sw
# make sure make apps has been run from your oc-accel directory
./hdl_single_engine/sw/hdl_single_engine -c 50 -w 0x00000600 -n10000 -N0 -p 0x00001F07  -t 10000 -C5 -vv
Read average bandwidth (Host->FPGA): 40960000 bytes in 2020 usec ( 20274.421 MB/s )
Read bandwidth min, max and variance: 40960000 bytes in 2020 usec ( 20257.171 MB/s, 20307.387 MB/s, 72.731 )
```

We can check the read bandwidth is around **20GB/sec** with a OC-AD9H3 installed in a IBM IC922 server.

```
cd oc-accel/action/hdl_single_engine/sw
# make sure make apps has been run from your oc-accel directory
./hdl_single_engine/sw/hdl_single_engine -c 50 -w 0x00000600 -N10000 -n0 -p 0x00001F07 -t 10000 -C5 -vv
Write average bandwidth (FPGA->Host): 40960000 bytes in 1894 usec ( 21611.797 MB/s )
Write bandwidth min, max and variance: 40960000 bytes in 1894 usec ( 20940.695 MB/s, 21671.958 MB/s, 9266.666 )
```

We can check the write bandwidth is around **21GB/sec** with a OC-AD9H3 installed in a IBM IC922 server.

## Command arguments details : 

- `-c 100` : perform 100 tests. eg to initiate the path and get an average latency
- `-w 0x00000601` : setp the configuration to read 4096
- `-n1` : n read operations requested (1 here)
- `-N0` : N write operations requested (0 here)
- `-p 0x00001F07` : p stands for "pattern", can be anything.
- `-t 100000` : t stands for "time out" in seconds
- `-C5` : C stands for "Card", here we choose the card number 5 in the server