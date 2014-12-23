---
layout: post
title:  "Setting up a PPTP Server on Ubuntu 14.04"
date:   2014-06-02 01:28:02
categories: unix
---

# 1. Install PPTP service.

{% highlight bash %}
sudo apt-get install pptpd
{% endhighlight %}

# 2. Modify `/etc/pptpd.conf`, uncomment the two lines at the end of the file.

{% highlight bash %}
localip 192.168.0.1
remoteip 192.168.0.50-238,192.168.0.245
{% endhighlight %}

# 3. Modify `/etc/ppp/chap-secrets` to setup the username and password.

{% highlight bash %}
your_username pptp your_password *
{% endhighlight %}

* `your_username` is the username for your VPN service.
* `pptp` is the service name for your VPN service.
* `your_password` is the password for your VPN service.
* `*` means the IP for each client is randomly allocated.

# 4. Modify `/etc/ppp/options`
{% highlight bash %}
ms-dns 106.187.36.20
ms-dns 106.187.34.20
logfile /var/log/pptpd.log
{% endhighlight %}

  The two DNS servers will be used on each client. If it is not configured, each client should setup one for itself. `logfile` will be used for PPTP service's logging, also you can comment `logwtmp` in `/etc/pptpd.conf` to disable logging.

# 5. Restart PPTP service.
{% highlight bash %}
/etc/init.d/pptpd restart
{% endhighlight %}

  And now you can connect to the PPTP service, but connected clients can not access the Internet as there are no routing rules.

# 6. Enable IP forwarding by modifying `/etc/sysctl.conf`

{% highlight bash %}
net.ipv4.ip_forward=1
{% endhighlight %}
  Execute `sysctl -p` to make it take effect.

# 7. Install `iptables` and add two forwarding rules:

{% highlight bash %}
apt-get install iptables
iptables -A FORWARD -s 192.168.0.0/24 -j ACCEPT
iptables -t nat -A POSTROUTING -s 192.168.0.0/24 -o eth0 -j MASQUERADE
{% endhighlight %}


# 8. Save the forwarding rules.
  As the rules added using iptables will be lost after system or network restart, we should save them and restore them after restart.

  * Save forwarding rules:
{% highlight bash %}
iptables-save > /etc/iptables-rules
{% endhighlight %}  

  * Create a new file `/etc/network/if-up.d/iptables` with the following content:

{% highlight bash %}
#!/bin/bash
 iptables-restore < /etc/iptables-rules
{% endhighlight %}  


  Execute `chmod +x /etc/network/if-up.d/iptables` to make it executable. Now the forwarding rules will be loaded each time network adapter boots.
