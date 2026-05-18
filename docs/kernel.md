# Linux Kernel Information and Status

## Kernel developers
The talented People involved in initial kernel upbringing, maintenance, porting ...  
List credit to: [TheMojoMan/xiaomi-pipa](https://github.com/TheMojoMan/xiaomi-pipa).  
In no particular order or degree of involvement, completeness not guaranteed.  
Please open a PR if you're missing!

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

| Sleep | Speakers | Mic | WLAN | Bluetooth | Charging @10w | Battery Status | Hall Sensor | Display | Brightness | Touch | GPU | USB (Host/Client) | DP alt mode | UFS | Back Camera | Front Camera | Sensors | Xiaomi Keyboard | Pen |
| ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- |
| ✅️ | ✅️ | ✅️ | ✅️ | ✅️ | ✅️ | ✅️ | ✅️ | ✅️ | ✅️ | ✅️ | ✅️ | ✅️ | ✅️ (Video only) | ✅️ | ⚠️ (sometimes) | ❌️ | ⚠️ (flaky) | ✅️ (pmOS: ❌️) | ✅️ (pmOS: ❌️) | 

## Actively maintained Kernels
All of those are based on the original port of the mainline kernel.

### pipaDB Kernel
- source at [github.com/pipaDB/linux](https://github.com/pipaDB/linux)
- latest version: `7.0.8`
- 'close-to'-mainline
- DP alt mode audio, front camera, 25w charging working on newest branches



### pipa-mainline Kernel
- source at [github.com/pipa-mainline/linux](https://github.com/pipa-mainline/linux)
- latest version: `6.18.28`
- 'close-to'-mainline



### postmarketOS Kernel
- source at [gitlab.postmarketos.org/postmarketOS/pmaports/-/tree/main/device/testing/linux-xiaomi-pipa](https://gitlab.postmarketos.org/postmarketOS/pmaports/-/tree/main/device/testing/linux-xiaomi-pipa)
- latest version: `7.0.6`
- mainline kernel with separate patches