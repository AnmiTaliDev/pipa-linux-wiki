# Linux Kernel Information and Status

There are at least 3 different 'close-to'-mainline kernels available for pipa.

## Kernel developers
The talented People involved in initial kernel upbringing, maintenance, porting ...  
List credit to: TheMojoMan @[xiaomi-pipa repo](https://github.com/TheMojoMan/xiaomi-pipa), in no particular order or degree of involvement.

- adomerle [github](https://github.com/adomerle) 
- vipaoL [github](https://github.com/vipaoL) 
- luka177 [github](https://github.com/luka177) 
- Dominik Sitarski [github](https://github.com/domin746826) 
- Danila Tikhonov [github](https://github.com/JIaxyga) 
- Teguh Sobirin [github](https://github.com/tjstyle) 
- lujianhua [github](https://github.com/lujianhua) 
- map220v [github](https://github.com/map220v) 
- maverickjb [github](https://github.com/maverickjb) 
- rmux [github](https://github.com/rmuxnet) 
- Users nixxiz, nikroks at pipa telegram groups 
- [postmarketOS Team](https://postmarketos.org) 

## Kernel Hardware Status
- all Kernels:

| Sleep | Speakers | Mic | WLAN | Bluetooth | Charging @10w | Battery Status | Hall | Display | Brightness | Touch | GPU | USB (Host/Client) | DP alt mode | UFS | Back Camera | Front Camera | Sensors | Xiaomi Keyboard | Pen | Hall Sensor
| ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- |
| ✅️ | ✅️ | ✅️ | ✅️ | ✅️ | ✅️ | ✅️ | ✅️ | ✅️ | ✅️ | ✅️ | ✅️ | ✅️ | ✅️ (Video only) | ✅️ | ⚠️ (sometimes) | ❌️ | ⚠️ (flaky) | ✅️ | ✅️ | ✅ |



## pipaDB Kernel
- source at [github.com/pipaDB/linux](https://github.com/pipaDB/linux)
- latest version: `7.0.8`
- 'close-to'-mainline kernel
- DP alt mode audio, front camera, 25w charging working



## pipa-mainline Kernel
- source at [github.com/pipa-mainline/linux](https://github.com/pipa-mainline/linux)
- latest version: `6.18.28`
- 'close-to'-mainline kernel



## postmarketOS Kernel
- source at [gitlab.postmarketos.org/postmarketOS/pmaports/-/tree/main/device/testing/linux-xiaomi-pipa](https://gitlab.postmarketos.org/postmarketOS/pmaports/-/tree/main/device/testing/linux-xiaomi-pipa)
- latest version: `7.0.6`
- mainline kernel with separate patches