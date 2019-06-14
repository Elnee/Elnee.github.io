---
title: "How to easily configure Canon LBP-1120 printer on Arch Linux"
categories:
	- Arch Linux
tags:
	- Arch
	- LBP1120
	- Linux
---

Install packages: `capt-sr`c and `cups` 
Then follow simple instruction, <> used for templating:

1. Add yourself to lp group: 

	```sh
	sudo gpasswd -a <your_name> lp
	```

	Relogin to your session.

2. Connect printer to your computer, turn it on, start/restart CUPS: 

	```sh
	sudo systemctl restart org.cups.cupsd.service
	```

3. Find correct `.ppd` file for your printer in **/usr/share/cups/model/** directory. 
In my case that was **CNCUPSLBP1120CAPTK.ppd**

4. Then execute: 

	```sh
	/usr/bin/lpadmin -p <name_of_printer> -m <name_of_corresponding_ppd_file> -v ccp://localhost:59678 -E
	```

5. Now you can easily `cd /dev/usb/` and `ls`. Just find name of your **lp*** device. In my case it was **lp2**. 

**Attention!** Note that you have to provide full path! For example: **/dev/usb/lp2**
{: .notice--warning}

6. Execute: 
	
	```sh
	ccpdadmin -p <name_of_printer> -o <udev_device>
	```

7. And enable ccpd daemon: 

	```sh
	sudo systemctl enable ccpd.service
	```