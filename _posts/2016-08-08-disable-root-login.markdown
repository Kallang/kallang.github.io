---
layout: post
title:  "Disable root Login on Ubuntu"
date:   2016-08-08 10:06:02
categories: unix
---

Create the user, replacing `example_user` with your desired username. You’ll then be asked to assign the user a password:

	adduser example_user

Add the user to the sudo group so you’ll have administrative privileges:

	adduser example_user sudo

Since the root username is predictable and provides complete access to your system, providing unfettered access to this account over SSH is unwise. Edit the /etc/ssh/sshd_config file to modify the PermitRootLogin option as follows:

	PermitRootLogin no

Restart `sshd`

	service ssh restart

After that you should login with your new created `non-root` user.

References:

* [Securing Your Server][1]

[1]: https://www.linode.com/docs/security/securing-your-server
