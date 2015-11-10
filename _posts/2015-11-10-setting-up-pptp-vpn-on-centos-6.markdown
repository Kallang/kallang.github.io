---
layout: post
title:  "Setting up PPTP-VPN on CentOS 6"
date:   2014-11-10 09:47:06
categories: os
---

Setting up a PPTP VPN on Ubuntu is easy, how about setting up one on CentOS?

# Install pptpd & iptables

{% highlight bash %}
yum install -y perl
yum install -y vim
yum install -y ppp
wget http://poptop.sourceforge.net/yum/stable/packages/pptpd-1.4.0-1.el6.x86_64.rpm
rpm -Uhv pptpd-1.4.0-1.el6.x86_64.rpm
yum install iptables
{% endhighlight %}

# Change the Configurations

## Change `/etc/pptpd.conf`

Change the `localip`.

## Change `/etc/ppp/options.pptpd`

Change `ms-dns` to what you want.

## Change `/etc/ppp/chap-secrets`

Add one line just like the following

```
your_username pptpd your_password *
```

## Change `/etc/sysctl.conf`

Enable IP forwarding.

```
net.ipv4.ip_forward = 1
```

Use `sysctl -p` to make it effect.

## Setting up iptables & start the service

{% highlight bash %}
iptables -A INPUT -i eth0 -p tcp --dport 1723 -j ACCEPT
iptables -A INPUT -i eth0 -p gre -j ACCEPT
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
service pptpd restart
chkconfig pptpd on
{% endhighlight %}


