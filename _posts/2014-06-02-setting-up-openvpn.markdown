---
layout: post
title:  "Setting up a PPTP Server on Ubuntu 14.04"
date:   2014-06-02 01:28:02
categories: unix
---

1. Install PPTP service.

	sudo apt-get install pptpd

2. Modify `/etc/pptpd.conf`, uncomment the two lines at the end of the file.

	localip 192.168.0.1
	remoteip 192.168.0.234-238,192.168.0.245

3. Modify `/etc/ppp/chap-secrets` to setup the username and password.

    your_username pptp your_password *

* `your_username` is the username for your VPN.
* `pptp` is the service name for your VPN service.
* `your_password` is the password for your VPN.
* `*` means the IP for each client is randomly allocated.

4. Modify `/etc/ppp/options`

	ms-dns 106.187.36.20
	ms-dns 106.187.34.20
	logfile /var/log/pptpd.log

  The two DNS server will be used on each client, if it is not configured, it should be configured on each client. `logfile` will be used for PPTP service's logging, also you can comment `logwtmp` in `/etc/pptpd.conf` to disable logging.

5. Restart PPTP service.

    /etc/init.d/pptpd restart

  And now you can connect to the PPTP service from clients, but clients can not access the Internet as there are no routing rules.

6. Enable IP forwarding by modifying `/etc/sysctl.conf`

	net.ipv4.ip_forward=1
  Execute `sysctl -p` to make it take effect.

7. Install `iptables` and add two forwarding rules:

	apt-get intall iptables
	iptables -A FORWARD -s 192.168.0.0/24 -j ACCEPT
	iptables -t nat -A POSTROUTING -s 192.168.0.0/24 -o eth0 -j MASQUERADE

8. Save the forwarding rules.
  As the rules added using iptables will be lost after system or network restart, we should save them and restore them after restart.
  * Save forwarding rules:
  
	iptables-save > /etc/iptables-rules

  * Create a new file `/etc/network/if-up.d/iptables` with the following content:

  iptables-restore < /etc/iptables-rules

  Execute `chmod +x /etc/network/if-up.d/iptables` to make it executable. Now the forwarding rules will be loaded each time network card boots.

