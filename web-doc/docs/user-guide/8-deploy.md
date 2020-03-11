# Deploy to Power Server

## Program FPGA

There are two ways to program FPGA: 
* Program Flash
* Program FPGA chip

### Program Flash

This is the default way to program FPGA. 

* Log on to Power9 server, 

``` 
$ git clone https://github.com/OpenCAPI/oc-utils/
$ make
$ sudo make install
```

* Copy the generated `hardware/build/Images/*.bin` from the development machine to Power9 server, and execute: 

For cards with SPIx8 Flash interface:
```
$ sudo oc-flash-script <file_primary.bin> <file_secondary.bin>
```

A `oc-reload` script is called automatically if the flash programming succeeds. This script reloads the bitstream from Flash and set up the OpenCAPI links again. 

!!!Warning
    AD9H7 card doesn't support above scripts temporally. (TODO)

* Check if the device is valid: 

```
$ ls /dev/ocxl
IBM,oc-snap.0007:00:00.1.0
```

### Program FPGA chip

Not like "programming flash" which permanently stores FPGA image into the flash on the FPGA board, programming FPGA chip is a temporal method and mainly used for debugging purpose. It uses ***.bit** file and the programmed image will be lost if the server is powered off. 

Prepare a laptop/desktop machine and install **Vivado Lab**. Use USB cable to connect it to the FPGA board's USB-JTAG debugging port. Then in Vivado Lab, right click the FPGA device name and select "program device..."


Then run 
```
sudo oc-reset
```

This command will bring the new bitstream working.

!!!Warning
    Host Flashing and oc-reset require firmware and OS kernel support. Contact your system provider for detailed information. 

## Install libocxl

See [https://github.com/OpenCAPI/libocxl/]

[https://github.com/OpenCAPI/libocxl/]: https://github.com/OpenCAPI/libocxl/


## Compile OC-Accel software and actions

```
$ git clone https://github.com/<MY_NAME>/oc-accel
$ make apps
```

You can check the FPGA image version, name and build date/time by 

```
$ cd software/tools
$ sudo ./oc_maint -vvv
```

```
[main] Enter
[snap_open] Enter: IBM,oc-snap
[snap_open] Exit 0x141730670
[snap_version] Enter
SNAP Card Id: 0x31 Name: AD9V3. NVME disabled, 0 MB DRAM available. (Align: 1 Min_DMA: 1)
SNAP FPGA Release: v0.2.0 Distance: 255 GIT: 0x12fb0b24
SNAP FPGA Build (Y/M/D): 2019/09/13 Time (H:M): 11:24
[snap_version] Exit
[snap_action_info] Enter
   Short |  Action Type |   Level   | Action Name
   ------+--------------+-----------+------------
     0     0x10140002     0x00000002  IBM hdl_single_engine in Verilog (1024b)
[snap_action_info] Exit rc: 0
[main] Exit rc: 0
[snap_close] Enter
[snap_close] Exit 0
```

# Run Application

```
$ cd actions/<my_new_action>/sw
$ sudo ./<app_name>
```

!!!Note
    Whenever calling the FPGA card, `sudo` is needed.

