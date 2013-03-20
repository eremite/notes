# Notes on bash (and linux)

## If you forget where you are:

    hostname

## For disk usage analysis:

    sudo du -sh ./*
    df -h

## Count the number of files in a directory.

    find . -type f | wc -l

## Sum the numbers in the first | delimited column in list.txt

    cut list.txt -d'|' -f1 | awk '{s+=$1} END {print s}'

## Rename extensions

    for i in *.php; do mv "$i" "${i/.php}".html; done

## Ubuntu stores its mail

    var/mail/username

## Show what distro you're using:

    lsb_release -a

## Show the latest exit code. 1 is true, 0 is false

    echo $?

## Bash shells and config files.

    A login shell is when you get the little introductory text like when using ssh. Opening up a console via gnome-terminal locally is not a login shell
    Pretty much everything is treated as an interactive shell, even scp.
    "When bash is invoked as an interactive login shell ... it first reads and executes commands from the file /etc/profile, if that file exists. After reading that file, it looks for ~/.bash_profile, ~/.bash_login, and ~/.profile, in that order, and reads and executes commands from the first one that exists and is readable."
    "When an interactive shell that is not a login shell is started, bash reads and executes commands from /etc/bash.bashrc and  ~/.bashrc, if these files  exist."

## To set up ssh bash_completion

    /etc/bash_completion.d/ssh

## Run something so that logging out won't kill it

    nohup bundle exec rake db:set_unit_names >> log/unit_names.log 2>&1 &
    
## Remove Old Kernels

    ls /boot/ | grep vmlinuz | sed 's@vmlinuz-@linux-image-@g' | grep -v `uname -r`
    sudo aptitude remove $old_kernel

## Redirect a single page to ssl in a vhosts file 

    Redirect /payments https://www.mokisystems.com/payments

## tar and gzip a directory

    tar -czf filename.tar.gz directory

## Renew dhcp lease

    sudo dhclient -r && sudo dhclient

## Set up a new server

    vim ~/.ssh/config
    SERVER=new_server
    ssh $SERVER
    adduser daniel
    vim /etc/group # add to moki group
    visudo # add to sudoers file ^K^U^U ^X
    exit
    ssh-copy-id $SERVER #make sure /home/daniel is not group writable.
    sync_server_dotfiles $SERVER

## Edit iptables

    sudo vim /etc/iptables.rules
    sudo iptables-restore < /etc/iptables.rules
