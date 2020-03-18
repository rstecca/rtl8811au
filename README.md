# Note by rstecca
This repo is forked from https://github.com/sloretz/rtl8811au but includes a **fix** for the [issue#6](https://github.com/sloretz/rtl8811au/issues/6) that causes `implicit declaration of function ‘is_compat_task’ [-Werror=implicit-function-declaration] if(is_compat_task())` when calling `make -j 8`

**I have no intention of maintaining this. All it is is a quick hotfix that worked for me on Debian 9 for my 8811au chipset WiFi dongle. I haven't tested with other distros but it might work.**

Follow instructions given in the [Building](#Building) section then run `modprobe 8821au`

Other useful commands:   
`lsmod | grep 8821au` to check whether te module has been installed correctly   
`lsusb` to list all usb devices and find infos about your WiFi dongle   
`lsusb -v -s 001:002` to obtain more infos about the device with address with bus=001 and device=002 (find these numbers with lsusb first)

## Random Disconnections
Random disconnections could be due to power saving been turned on for the device. You can check this is your case by inspecting the kern.log file with `sudo tail /var/log/kern.log`. If, in your listing, you can find the line `Jan 18 10:14:23 PCNAME kernel: [  384.189391] RTL871X: rtw_set_ps_mode(enp0s20f0u2) Enter 802.11 power save - WIFI-TRAFFIC_IDLE` than you have confirmed this is your problem.

If you dig the internet for a solution you'll mostly find command sequences that rely on `iw`. Unfortunately `iw` here isn't helpful since rtl is not a wlan device so `iw` will not see it.

Someone claims the solution is to edit/create a file called /etc/NetworkManager/conf.d/default-wifi-powersave-on.conf and enter the lines

```
[connection]
wifi.powersave = 2
```

but I tested this and it does not solve the issue. At least under Debian 9.  
**I am currently looking for a solution so if you have one please refer to [issue #1](https://github.com/rstecca/rtl8811au/issues/1)** 

---

**_THE FOLLOWING IS THE ORIGINAL TEXT FROM THE AUTHOR OF THIS VERSION OF rtl8811au_**

---

# rtl8811au driver for recent version of ubuntu

This repo contains a driver originally called `rtl8821AU_linux_v4.3.14_13455.20150212_BTCOEX20150128-51` with additional patches to get it to build on Ubuntu xenial 16.04.

I originally got the source code off a cd that came with [this usb wifi adapter](https://www.amazon.com/Heiyo-Network-600Mbps-802-11ac-Wireless/dp/B01N2NJFPG).

I tested on Ubuntu 16.04.2 LTS with kernel 4.4.0-64-generic using a "Heiyo Network Wi-Fi Dongle 802.11ac"


## Building

```
sudo make clean
make -j 8
sudo make install
```

## License
The driver license appears to be GPLv2

# Similar repositories
The repository [diederikdehaas/rtl8812AU](https://github.com/diederikdehaas/rtl8812AU) appears to have newer and older versions of the RealTek driver.
It also has a version supporting dkms.
