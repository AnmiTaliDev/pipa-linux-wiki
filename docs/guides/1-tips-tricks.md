# Tips, Tricks and known Quirks & Issues

## Known Quirks & Issues
??? warning "Gyro and ambient light sensors don't work or break after waking the System"
	This is a known issue with how the sensors are turned off and on again when suspending and resuming.  
	Potential fixes include:  

	- making sure you have the `pipa-sensors`, `iio-sensor-proxy` & `hexagonrpcd-sdsp` packages installed
	- restarting sensor services in the following order (example for systemd):
		1. `systemctl restart iio-sensor-proxy.service`
		2. `systemctl restart hexagonrpcd-sdsp.service`
		3. `systemctl restart iio-sensor-proxy.service`
	- creating a simple system service that restarts the services on every system wake (example for systemd, create file `/usr/lib/systemd/system-sleep/pipa-sensor-restart`):  
	``` sh
	#!/bin/sh
	case "$1" in
  		post)
    		/usr/bin/systemd-run \
      		--unit=pipa-sensor-restart --no-block \
     		/bin/sh -c '
        	sleep 2
        	/usr/bin/systemctl restart iio-sensor-proxy.service
        	/usr/bin/systemctl restart hexagonrpcd-sdsp.service
        	/usr/bin/systemctl restart iio-sensor-proxy.service
      		'
  		;;
	esac
	```

??? warning "Netflix, Spotify, etc dont work because of missing Widevine CDM Module"
	Google still don't package their Widevine CDM for ARM64 Linux.  

	You can either install it manually, or use [Asahi Linux' Widevine Installer](https://github.com/AsahiLinux/widevine-installer).  
	This will only work for Firefox and Chromium-based Browsers installed as system Packages, not for Flatpaks.

??? quote "Bluelight Filter/Night Light doesn't work in Gnome for lack of a Color Profile"
    ![Gnome Knows Best](../gnome-knows-best.jpeg)

??? warning "Performance seems strangely bad and/or Power Profiles on my Desktop Environment are missing"
	Install `tuned` & `tuned-ppd`.

??? failure "GTK/libadwaita apps are artifacting heavily on Kernel versions newer than 6.19.0"
	 This is a known GTK/GSK issue. A temporary fix is to force GSK to use its legacy renderer: 
	``` bash
	echo "GSK_RENDERER=gl" | sudo tee -a /etc/environment
	``` 
	See [postmarketOS wiki](https://wiki.postmarketos.org/wiki/Xiaomi_Pad_6_(xiaomi-pipa)#GTK_apps_flicker) for more info.

??? warning "Docker complains about missing iptables on certain Kernel versions"
	This is a problem related to the legacy `iptables` kernel module, mostly on Fedora.
	``` bash
	sudo alternatives --set iptables /usr/bin/iptables-nft
	sudo systemctl restart docker
	``` 

## Tips & Tricks
### Unbricking your pipa using EDL mode
You can try unbricking your device with Emergency Download Mode using rmuxnet's guide on the [pipaDB Github](https://github.com/PipaDB/Info/blob/main/edl.md).  

### Choosing a Desktop Environment or Window Manager  
...  


### Gnome Desktop Environment 
#### Recommended Shell extensions
- [Screen Rotation Extension](https://github.com/shyzus/gnome-shell-extension-screen-autorotate) to toggle auto-rotate or set it up as manual Rotation toggle
- [GJS OSK extension](https://github.com/Vishram1123/gjs-osk) to make the Gnome OSK usable (if your enter key gets stuck, remove it)
- [TouchUP extension](https://github.com/mityax/gnome-extension-touchup) to make the Gnome Shell more usable on a Touchscreen
