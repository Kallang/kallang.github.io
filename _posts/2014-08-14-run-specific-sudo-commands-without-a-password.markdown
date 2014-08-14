---
layout: post
title:  "Run Specific Sudo Commands without A Password"
date:   2014-08-14 13:00:00
categories: unix
---
  Sometimes we need to run some sudo commands in a shell without a password input, for example when we need to reboot if we found some service is down in a periodic checking shell script as follow.

    #!/bin/bash

    output="output.txt"

    failure_count=0
    while True
    do
        echo "Checking git server status"
        rm -rf $output
        curl http://192.168.1.120 --connect-timeout 4 -o $output
        if [ -e $output ]
        then
            echo "webpage file exists."
            failure_count=0
        else
            echo "webpage file does not exist."
            failure_count=$(($failure_count+1));
            echo "failure count: $failure_count"
        fi
        if [ $failure_count -gt 3 ]
        then
            echo "boot the git server."
            echo `date` >> log.txt
            sudo reboot
            failure_count=0
            sleep 300
        fi
        sleep 60
    done

  If the password is required during `sudo reboot`, the script won't work. In this case we can use `NOPASSWD` directive in `/etc/sudoers` file.
  If you `user_name` is your user, we can add the following line to `/etc/sudoers` with command `sudo visudo -f /etc/sudoers`.

    user_name ALL=(ALL) NOPASSWD: /sbin/poweroff, /sbin/reboot, /sbin/shutdown

  Now it's done.
