# Oc_find_card debug option tools in POWER server environment

oc_find_card is a basic tool allowing to explore which card have been properly discovered as OpenCAPI (OC)  at server startup

If called with a request for all card with -A ALL option it provides results like this :

```
user@P9server:sudo ~/oc-accel/software/tools/oc_find_card -v -AALL
oc_find_card version is 2.6

OC-AD9V3 card has been detected in OPENCAPI card position: 5
 Device ID    is                                                : 0x062b
 Sub device   is                                                : 0x060f
 Image loaded is self defined as                                : factory
 Virtual Card PCI location is                                   : 0005:00:00.1
 Card PCI physical slot is                                      : Not Applicable

OC-AD9H7 card has been detected in OPENCAPI card position: 6
 Device ID    is                                                : 0x062b
 Sub device   is                                                : 0x0666
 Image loaded is self defined as                                : factory
 Virtual Card PCI location is                                   : 0006:00:00.1
 Card PCI physical slot is                                      : Not Applicable

OC-BW250SOC card has been detected in OPENCAPI card position: 7
 Device ID    is                                                : 0x062b
 Sub device   is                                                : 0x066a
 Image loaded is self defined as                                : factory
 Virtual Card PCI location is                                   : 0007:00:00.1
 Card PCI physical slot is                                      : Not Applicable

Total 3 cards detected 

```

If called with the debug (-d) option, it will provide extra system information and will collect reports concerning OpenCAPI information.

It can be useful if a card shows unexpected behavior.

It will also create a tar.gz file in the current dir to store valuable information when contacting support team.

