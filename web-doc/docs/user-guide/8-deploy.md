# Deploy to Power Server

## Setup POWER tools on your server

Follow this [guide](../../apps_notes/setup_power_tools/)

## Program FPGA card

There are two ways to program an FPGA card:

* Program Flash: the configuration flash contains the code used at poweron.
* Program FPGA chip: (the chip will keep its hardware configuration as long as power is maitained.

### Program Flash

This is the default way to program FPGA, as hardware description is kept in card's flash and reused at each start-up.
Check with `lspci|grep accel` how is the card currently configured.

- In case the flash **doesn't contain an OC-accel image or a snap** (CAPI2) image, use the method provided by the **card supplier** to load the flash
- if the card contains a CAPI2 hardware (with device code 0477), use `capi-flash-script` routine as per:  [https://github.com/ibm-capi/capi-utils](https://github.com/ibm-capi/capi-utils)
- If the hardware is already an OC-accel hardware (with device code 062b), please use the following process:

!!! note
    The following steps assume the running code is an OC-accel one.

* Log on to Power9 server, 

  * make sure you have installed the required OpenCAPI tools : 
  	A general procedure is available [here](../../apps_notes/setup_power_tools/)
  	Check the debug option of oc-find_card tool.

* Copy the generated `hardware/build/Images/*.bin` from the development machine to the Power9 server, and execute: 

  For cards with 8 bit Flash interface (requiring only one bin file):

```
	$ sudo oc-flash-script <file.bin> 
```

​		For cards with 2 x SPIx4 Flash interface (requiring 2 bin files):

```
	$ sudo oc-flash-script <file_primary.bin> <file_secondary.bin>
```

A `oc-reload` script is called automatically if the flash programming succeeds. This script reloads the bitstream from Flash and set up the OpenCAPI links again. 

* Eventually check if the device is valid: 

```
$ ls /dev/ocxl
IBM,oc-snap.0007:00:00.1.0
```

### Program FPGA chip

Not like "programming flash" which permanently stores FPGA image into the flash on the FPGA board, programming FPGA chip is a temporal method and mainly used for debugging purpose. It uses ***.bit** file and the programmed image will be lost if the server is powered off. 

Prepare a laptop/desktop machine and install **Vivado Lab Manager** on a X86 system. Use USB cable to connect it to the FPGA board's USB-JTAG debugging port. Then in Vivado Lab, right click the FPGA device name and select "program device..."


Then run 
```
sudo oc-reset
```

This command will bring the new bitstream working.

!!! note "NOTE"
    oc-reset require lastest firmware and OS kernel (see [here](./../../system_firmware_setup/))




## Compile OC-Accel software and actions

```
$ git clone https://github.com/Opencapi/oc-accel
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
    Whenever calling the FPGA card, `sudo` is needed by default. Check "Privileges" section of the [guide](../../apps_notes/setup_power_tools/) to setup extended rights.

