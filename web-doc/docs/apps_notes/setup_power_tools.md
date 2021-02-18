# Setup tools on the POWER server environment

Steps to setup your environment on the Power server:

### 1) Check System Firmware Setup 

[Here](./../../system_firmware_setup/)

### 2) Install the libocxl development libraries + reboot the server after installation. 

For ubuntu: `sudo apt-get install libocxl-dev`

For RHEL:

- ***if RHEL < 8***
    
- `	sudo yum install libocxl-devel`
    
- ***if RHEL > 8*** 
    - Check:

    [https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/package_manifest/codereadylinuxbuilder-repository](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/package_manifest/codereadylinuxbuilder-repository)

    - Edit the following file:	`sudo vi /etc/yum.repos.d/redhat.repo`
    
    - and make sure desired repo is enabled:
    
    `[codeready-builder-for-rhel-8-ppc64le-rpms]`
    `name = Red Hat CodeReady Linux Builder for RHEL 8 Power, little endian (RPMs)`
    `baseurl = https://cdn.redhat.com/content/dist/rhel8/$releasever/ppc64le/codeready-builder/os`
    `enabled = 1`
    
    - Then install your prefered devel lib: `sudo dnf install libocxl-devel` (libocxl-devel package is provided by RedHat Optional repository)   

### 3) Check card detection

Check system OC card detection after startup:

```
user@P9server:~$ lspci|grep accel
0004:00:00.0 Processing accelerators: IBM Device 062b
0004:00:00.1 Processing accelerators: IBM Device 062b
0005:00:00.0 Processing accelerators: IBM Device 062b
0005:00:00.1 Processing accelerators: IBM Device 062b
0006:00:00.0 Processing accelerators: IBM Device 062b
0006:00:00.1 Processing accelerators: IBM Device 062b
```

!!! note
    In this example, we see 3 OC cards, each one having 2 asssociated virtual slots. 062b device code is related to OC mounting.

Check the creation of the proper system directories:

```
user@P9server:~$ll /dev/ocxl/
total 0
drwxr-xr-x  2 root root    100 Feb 17 04:24 ./
drwxr-xr-x 22 root root   4000 Feb 15 18:26 ../
crw-rw-rw-  1 root root 240, 0 Feb 17 01:54 IBM,oc-snap.0004:00:00.1.0
crw-rw-rw-  1 root root 240, 1 Feb 17 02:28 IBM,oc-snap.0005:00:00.1.0
crw-rw-rw-  1 root root 240, 2 Feb 17 04:24 IBM,oc-snap.0006:00:00.1.0
```

### 4) Clone the FPGA Image loader

```
sudo git clone  https://github.com/OpenCAPI/oc-utils.git 
cd oc_utils
sudo make install 
```

This will provide oc-reset, oc-reload and oc-flash-script routines.

### 5) Clone the oc-accel framework 

If you don't need the "contribution" mode:    `git clone https://github.com/OpenCAPI/oc-accel.git`

 If you need the "contribution" mode use ssh: `git clone git@github.com:OpenCAPI/oc-accel.git`

```
cd oc-accel
make software
```

or eventually `make apps`if you want to test some examples

### 6) use OC tools to check card contents:

```
user@p9server:~/oc-accel$ ./software/tools/oc_find_card -v -AALL
oc_find_card version is 2.7

OC-AD9V3 card has been detected in OPENCAPI card position: 5
 Device ID    is                                                : 0x062b
 Sub device   is                                                : 0x060f
 Image loaded is self defined as                                : factory
 Virtual Card PCI location is                                   : 0005:00:00.1
 Card PCI physical slot is                                      : Not Applicable

OC-AD9H3 card has been detected in OPENCAPI card position: 4
 Device ID    is                                                : 0x062b
 Sub device   is                                                : 0x0667
 Image loaded is self defined as                                : factory
 Virtual Card PCI location is                                   : 0004:00:00.1
 Card PCI physical slot is                                      : Not Applicable

OC-AD9H7 card has been detected in OPENCAPI card position: 6
 Device ID    is                                                : 0x062b
 Sub device   is                                                : 0x0666
 Image loaded is self defined as                                : factory
 Virtual Card PCI location is                                   : 0006:00:00.1
 Card PCI physical slot is                                      : Not Applicable

Total 3 cards detected
```

!!! note
    oc_find_card will also detect potential CAPI2 cards. Don't get confused with cards numbering. Here OC card OC-AD9H3 is card #4

```
user@p9server:~/oc-accel$ ./software/tools/oc_maint -C4    # 4 is OC-AD9H3 card number
OC-ACCEL Card Id: 0x32 Name: OC-AD9H3.  0 HBM AXI interfaces. (Align: 1 Min_DMA: 1)
OC-ACCEL FPGA Release:       v8.0.0 Distance: 45 GIT: 0xd8bc1c1d
OC-ACCEL FPGA Build (Y/M/D): 2021/02/17 Time (H:M): 01:00
OC-ACCEL FPGA Up Time:       100109sec : 1d 3h 48mn 29sec
   Short |  Action Type |   Level   | Action Name
   ------+--------------+-----------+------------
     0     0x10143009     0x00000010  IBM HLS Helloworld_1024   (1024b)
```

!!! note
    We observe a OC-AD9H3 card containing a hardware code generated 2021/02/17 with a basic IBM HSL_helloworld_1024 example.

### 7) Privileges

Important: libocxl requires root privileges to allow card exchanges (like oc-reset, oc_maint, usage in general).

When using the card without sudo privileges, you get an normal error.

Your administrator can provide user privileges using this process:

- Permanently create a /etc/udev/rules.d/20-ocaccel.rules file including:

  ```
  SUBSYSTEM=="ocxl", DEVPATH=="*/ocxl/IBM,oc-snap*", MODE="666", RUN="/bin/chmod 666 %S/%p/global_mmio_area"
  ```

- Reboot
  

