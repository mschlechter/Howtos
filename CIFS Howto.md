# CIFS Howto

Mounting CIFS shares from Ubuntu.

##Step 1: Install cifs-utils

    sudo apt install cifs-utils

##Step 2: Create a credentials file

The format of the file is:

    username=value
    password=value
    domain=value (optional)

Store this file as /etc/cifs_credentials (as root)

##Step 3: Create a mount point

    sudo mkdir /mnt/backup

##Step 4: Test if mounting works

    sudo mount -t cifs //192.168.2.110/backup /mnt/backup -o credentials=/etc/cifs_credentials

This should mount the share. Now try to unmount it again:

    sudo umount /mnt/backup

##Step 5: Create an entry in /etc/fstab

Add the following line to /etc/fstab:

    //192.168.2.110/backup  /mnt/backup cifs    noauto,credentials=/etc/cifs_credentials   0   0

You can now easily mount your CIFS share using the mount point:

    sudo mount /mnt/backup

I have added the noauto option to prevent Linux from automatically mounting the
Windows share. The share should only be mounted when needed.

That's all :-)
