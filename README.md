# pfsense-vpn-notifications
Receive Email and Telegram notifications when a client connects/disconnects to/from a pfsense ipsec or openvpn server


### SSH

- SSH to your pfsense firewall. Upload files to /root/
- chmod 755
- If files are not saved at /root/ change all the paths in the scripts. e.g.:
``` 
openvpnconnect.sh
...
php /root/notify.php [...] --> php /root/vpn_notify/notify.php [...]
...
```

```
 4 -rwxr-xr-x   1 root  wheel   1999 Jul 17  2019 ipsec.php
 4 -rwxr-xr-x   1 root  wheel    198 Jul 17  2019 notify.php
 4 -rwxr-xr-x   1 root  wheel    308 Jul 17  2019 openvpnconnect.sh
 4 -rwxr-xr-x   1 root  wheel    498 Jul 17  2019 openvpndisconnect.sh
```

### Open VPN
For open VPN:

- Login to GUI
- VPN
- Edit your OpenVPN Server
- Under Advanced Configuration, Custom options, add the following lines and adjust paths if necessary:

```
client-connect /root/openvpnconnect.sh;
client-disconnect /root/openvpndisconnect.sh; 
```

### ipsec
Create cron to run every minute or so. This will not give exact times for connect/disconnect but within a 60 second window.

- Login to GUI
- Services
- Cron

```*	*	*	*	*	root	/usr/local/bin/php /root/ipsec.php > /dev/null 2>&1```

### Notifications

Setup your SMTP server and Telegram Notifications. The above files utilize inbuilt pfsense php functions to send notifications via the send_smtp_message and notify_via_telegram function. 

- Login to GUI
- System
- Advanced
- Notifications Tab
