# ILA Debug

Enable `ILA_DEBUG` in Kconfig Menu. 
Add your ILA core.


# Building in Docker Containers

When using the Xilinx Vivado Toolchain in a Docker Container, issues may arise if the container does not feature a proper `init` process.
The subprocess management for individual build steps like Synthesis seems to rely on orphaned processes being joined by `init`.
If this does not happen, the entire build can freeze.

Therefore you should take care to start Docker containers intended for building `oc-accel` with `--init` [(documentation)](https://docs.docker.com/engine/reference/run/#specify-an-init-process) or include `init: true` in a `docker-compose.yml` [(documentation)](https://docs.docker.com/compose/compose-file/#init).


# Known issues and TODO

Features to be supported: 

* Interrupt
* Multiple-process supported

Cleanup work:

* Renaming SNAP to OCAC; oc-snap to oc-accel

Enrich example lists: 

* Multiple-process example
* Ethernet example
* HBM example

