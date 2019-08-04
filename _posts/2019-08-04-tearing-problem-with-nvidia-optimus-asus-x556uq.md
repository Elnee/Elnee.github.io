---
title: "How to solve tearing problem with Nvidia OPTIMUS on Asus X566U(Q)"
categories:
  - Troubleshooting
tags:
  - Linux
  - Asus
  - Nvidia
---

To solve this problem you have to install Bumblebee, mesa, xf86-video-intel, nvidia proprietary or nouveau driver.

1. Add yourself to a bumblebee group:

	```sh
	sudo gpasswd -a user bumblebee
	```

	Also you have to enable Bumblebee service:

	```sh
	sudo systemctl enable bumblebeed.service
	```

2. Create file `20-intel.conf` in **/etc/X11/xorg.conf.d/** and write next strings:

	```
	Section "Device"
	  Identifier "Intel Graphics"
	  Driver "intel"
	  Option "AccelMethod" "sna"
	  Option "TearFree" "true"
	EndSection
	```

And here you go :)
