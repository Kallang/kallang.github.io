---
layout: post
title:  "About Internal Field Seperator"
date:   2014-08-18 23:49:11
categories: unix
---
  I met some problems during processing some files with names contains spaces, such as the following. If the file name contains space, the corresponding files will not be deleted, as the `rm -rf` treats the file name as several files.

    find . -name *.flac | xargs rm -rf

  I found that it is the IFS(internal field seperator) in bash leads to this.

# What Is IFS?

1. The IFS is a special shell variable.
2. You can change the value of IFS as per your requirments.
3. The Internal Field Separator (IFS) that is used for word splitting after expansion and to split lines into words with the read builtin command.
4. The default value is <space><tab><newline>. You can print it with the following command:(cat -etv <<<"$IFS")
5. IFS variable is commonly used with read command, parameter expansions and command substitution. You can `man bash` and search for IFS.

# IFS Effect On The Values of "$@" And "$*"


1. $@ and $* are special command line arguments shell variables.
2. The $@ holds list of all arguments passed to the script.
3. The $* holds list of all arguments passed to the script.


  Create a shell script called ifsargs.sh:

    #!/bin/bash
    # ifsargs.sh - Cmd args - positional parameter demo
    echo "Command-Line Arguments Demo"
    echo "*** All args displayed using \$@ positional parameter ***"
    echo $@
    echo "*** All args displayed using \$* positional parameter ***"
    echo $*

Then run it.

    chmod +x ifsargs.sh
    ./ifsargs.sh honda yamaha harley-davidson kawasaki

  We get the following outputs.

    Command-Line Arguments Demo
    *** All args displayed using $@ positional parameter ***
    honda yamaha harley-davidson kawasaki
    *** All args displayed using $* positional parameter ***
    honda yamaha harley-davidson kawasaki

  As you can see, the values of $@ and $* are the same. However the values of "$@" and "$*" are different. Edit the ifsargs.sh again as follows.

    #!/bin/bash
    # ifsargs.sh - Cmd args - positional parameter demo

    #### Set the IFS to | ####
    IFS='|'

    echo "Command-Line Arguments Demo"

    echo "*** All args displayed using \$@ positional parameter ***"
    echo "$@"        #*** double quote added ***#

    echo "*** All args displayed using \$* positional parameter ***"
    echo "$*"        #*** double quote added ***#

  Then run it again with the same arguments as above. We can get the following.

    Command-Line Arguments Demo
    *** All args displayed using $@ positional parameter ***
    honda yamaha harley-davidson kawasaki
    *** All args displayed using $* positional parameter ***
    honda|yamaha|harley-davidson|kawasaki

  So we can see that.

1. $@ expanded as "$1" "$2" "$3" ... "$n"
2. $* expanded as "$1y$2y$3y...$n", where y is the value of IFS variable i.e. "$*" is one long string and $IFS act as an separator or token delimiters.

  And we can solve the problem mentioned in the beginning by easily changing the IFS.
  
# References:

* [IFS][1]

[1]: https://library.linode.com/networking/socks-proxy
