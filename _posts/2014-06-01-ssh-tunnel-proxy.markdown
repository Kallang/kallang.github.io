---
layout: post
title:  "Setting up an SSH Tunnel and Using it as Firefox Proxy"
date:   2014-06-01 21:28:02
categories: unix
---
Firstly, we should have a server with SSH service running. Set up a user with shell disabled.


    useradd /home/socksproxy -m -g socksproxy -s /bin/false socksproxy
    useradd -d /home/socksproxy -m -g socksproxy -s /bin/false socksproxy
    passwd socksproxy

Secondly, use the following command to setup the proxy.

    ssh -qTfnN -D 8080 socksproxy@your_server_ip
    
* `-q` Quiet mode.
* `-T` Disable pseudo-tty allocation.
* `-t` Disable pseudo-tty allocation.
* `-f` Requests ssh to go to background just before command execution.
* `-N` Do not execute a remote command.  This is useful for just forwarding ports (protocol version 2 only).
* `-n` Redirects stdin from /dev/null (actually, prevents reading from stdin).  This must be used when ssh is run in the background.


Thirdly, open Firefox and set up a socks v5 proxy with host your_server_ip and port 8080. Now you can have secure web traffic through Firefox, but the DNS traffic is still in the clear. As Firefox does the DNS lookups locally by defaut, we should change that.

* Type `about:config` in your browser's and press Enter to open Firefox preferences.
* Type `type network.proxy.socks` to filter the settings we want out. Set `network.proxy.socks_remote_dns` to `true` by double clicking it.
* Restart Firefox.

For linode users, if you got the following result during visiting google,
	
	We're sorry...
	
	... but your computer or network may be sending automated queries. 
	To protect our users, we can't process your request right now.
	See Google Help for more information.

You should disable your ipv6 by adding the following lines in `/etc/sysctl.conf`.

	net.ipv6.conf.all.disable_ipv6 = 1 
	net.ipv6.conf.default.disable_ipv6 = 1 
	net.ipv6.conf.lo.disable_ipv6 = 1



References:

* [Setting up an SSH Tunnel with Your Linode for Safe Browsing][1]
* [Tunnel DNS Lookups in SOCKS Proxy on Firefox][2]

[1]: https://library.linode.com/networking/socks-proxy
[2]: http://jaamee.wordpress.com/2009/08/16/tunnel-dns-lookups-in-socks-proxy-on-firefox/
