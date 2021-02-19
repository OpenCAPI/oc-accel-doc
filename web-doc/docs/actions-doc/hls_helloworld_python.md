# hls_helloworld_python
![oc-accel-bar](../pictures/oc-accel-bar.png)

## Code location:

Code can be found at:[https://github.com/OpenCAPI/oc-accel/blob/master/actions/hls_helloworld_python/](https://github.com/OpenCAPI/oc-accel/blob/master/actions/hls_helloworld_python/) 

## In short:

This example provides a simple base allowing to discover OC-ACCEL. It uses a python described application (running on the host) to exercise an "hls" written action (the part running in the FPGA).

The action is the exact same action as in the helloworld_1024 example.

It can be checked in /action.Kconfig file that the ACTION_HALF_WIDTH bloc is not used for this example, so the interface uses the OpenCAPI 1024 bit bus.

Hence the binary file to load into the FPGA is the same as the helloworld_1024 binary file.

## Users' Guide:

Python code is changing characters case of a user phrase

- ​	code can be executed on the CPU (will transform all characters to lower case)
- ​    code can be simulated (will transform all characters to upper case in simulation)
- ​    code can then run in hardware when the FPGA is programmed (will transform all characters to upper case in hardware)

The example code uses the copy mechanism to get/put the file from/to system host memory to/from DDR FPGA attached memory

### Prerequisites:

Some packages may need to be installed on your system:

```
# Installing python development library, swig and curl
sudo apt-get install python3-dev python3-venv swig curl
```

### Get started - Compilation:

To get started use the following steps:

```
git clone https://github.com/OpenCAPI/oc-accel.git #if using regular https method
#git git@github.com:OpenCAPI/oc-accel.git  # if using ssh method with privileges
git clone https://github.com/OpenCAPI/ocse
cd oc-accel
make snap_config        #select (X) HLS_helloworld_python 
# if you come back and use a new environment use 
. ./snap_path.sh # to set the SNAP_ROOT path
# either manually restore the path kile this :
vim snap_env.sh # -> export SNAP_ROOT=/home/~/oc-accel  #or your oc-accel directory
# Check ocse PATH if not default ~/ocse
. snap_env.sh
make software           # this will compile any required tools
cd actions/hls_helloworld_python/sw
python3 -m venv env     # to create a local dev environment
                        # note you will have a "env" before the prompt to remind
                        # you the local environment you work in
source env/bin/activate # 
pip3 install -r requirements.txt # to be improved : some errors might occur depending on environment
make pywrap  # to compile the appropriate libraries for SWIG
```

### Get started - Simulation:

To launch a Python shell and use the OCSE (RTL simulation)

- Run action simulation in your swig env

```
cd ${SNAP_ROOT}
make sim 
```

- On the xTerm that pops-up:

```
oc_maint -vvv

LD_LIBRARY_PATH=$OCSE_ROOT/libocxl/ python3
# once inside python call the libs
import sys
import os

snap_action_sw=os.environ['SNAP_ROOT'] + "/actions/hls_helloworld_python/sw"
print(snap_action_sw)
sys.path.append(snap_action_sw)

import snap_helloworld_python
 
input = "Hello world. This is my first CAPI SNAP experience with Python. It's extremely fun"
output = "11111111111111111111111111111111111111111111111111111111111111111111111111111111111111"

out, output = snap_helloworld_python.uppercase(input)

print("Output from FPGA:"+output)

print("Output from CPU :"+input.upper())

exit() # from python shell

exit # from xTerm
```

### 

### Get started -Simulation with Jupyter Notebook

To launch a Jupyter Notebook and use the OCSE (RTL simulation)

- Run action simulation

```
cd ${SNAP_ROOT}
make sim 
```

- On the xTerm that pops-up:

```
oc_maint -vvv

cp ../../../../actions/hls_helloworld_python/sw/snap_helloworld_python.py .
cp ../../../../actions/hls_helloworld_python/sw/trieres_helloworld_cosim.ipynb .

# For Jupyter:
jupyter notebook trieres_helloworld_cosim.ipynb # and follow the instrunctions: Select every cell and run it with Ctrl+d

# For Jupyter Lab:
jupyter-lab trieres_helloworld_cosim.ipynb # and follow the instrunctions: Select every cell and run it with Ctrl+d

Ctrl+c # kill Jupyter Notebook / Jupyter Lab

exit # from xTerm
```

### 

### Get started - Excution on P9

To launch a Jupyter Notebook on P9 (on the FPGA card)

- Ensure you have compiled oc-accel's software on P9

```
git clone https://github.com/OpenCAPI/oc-accel.git #if using regular https method
#git git@github.com:OpenCAPI/oc-accel.git  # if using ssh method with privileges
cd <your oc-accel dir>
. ./snap_path.sh  # this will set the SNAP_ROOT variable to your oc-accel dir
make software
```

- Continue as any action (you may need sudo when execution jupyter to have valid access rights for the card):

```
sudo oc_maint -vvv

cd ${SNAP_ROOT}/actions/hls_helloworld_python/sw/

sudo jupyter notebook trieres_helloworld.ipynb # and follow the instrunctions: Select every cell and run it with Ctrl+d

Ctrl+c # kill Jupyter Notebook

exit # from xTerm
```