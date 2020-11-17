# Setup tools on the POWER server environment

Setup the followings to get your environment on the Power server

1)	Clone the oc-accel framework (in "no contribution" mode)

```
git clone https://github.com/OpenCAPI/oc-accel.git
```

 Clone the oc-accel framework (in "contribution" mode using ssh)

```
git clone git@github.com:OpenCAPI/oc-accel.git
```

2)	Install the libocxl development libraries + reboot the server after installation. 

(for ubuntu) 

```
sudo apt-get install libocxl-dev
```

(for RHEL < 8) 

```
sudo yum install libocxl-devel
```

(for RHEL > 8) 

Check:

https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/package_manifest/codereadylinuxbuilder-repository

Edit the following file:

```
sudo vi /etc/yum.repos.d/redhat.repo
```

and make sure desired repo is enabled 

```
[codeready-builder-for-rhel-8-ppc64le-rpms]
name = Red Hat CodeReady Linux Builder for RHEL 8 Power, little endian (RPMs)
baseurl = https://cdn.redhat.com/content/dist/rhel8/$releasever/ppc64le/codeready-builder/os
enabled = 1
```

Then install your prefered devel lib:

```
sudo dnf install libocxl-devel
```

(libocxl-devel package is provided by RedHat Optional repository)   

3)	Clone the FPGA Image loader

```
sudo git clone  https://github.ibm.com/OC-Enablement/oc-utils/ 
cd oc_utils
sudo make install 
```



## Privileges

Important: libocxl requires root privileges to allow card exchanges (like oc-reset, oc_maint, usage in general).

When using the card without sudo privileges, you get an normal error.

Your administrator can provide user privileges using this process:

- Permanently create a /etc/udev/rules.d/20-ocaccel.rules file including:

  ```
  SUBSYSTEM=="ocxl", DEVPATH=="*/ocxl/IBM,oc-snap*", MODE="666", RUN="/bin/chmod 666 %S/%p/global_mmio_area"
  ```

- Reboot
  

