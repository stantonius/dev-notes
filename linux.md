# My Linux notes

This could be massive - however below are some of the key things I want to make note of that have confused me.

One thing that took me a long time to troubleshoot - Linux doesn't do well with spaces in directory names. So get rid of these

## Commands

* `touch filename` - creates a file (ie. like Dockerfile)
* `chmod - R 777 directory` - **recursively** modifies permissions for directory and all below

## Permissions

There are 3 official buckets of users that Linux grants directory and file permissions to: **User**, **Group**, and **Other**

Q: how do you distinguish what bucket *you* are in when viewing a file?
Q: how can you list all files including ones you do not have access to? Or this this just possible with admin/root user? 

* **User** is the owner of the file/directory
* **Group**: ...there is something about the default group names take on the name of the user? Need to figure this out

### Permission Structure

Easy to verify with `ls -l (optional) file`

<p align="center">
<img src="https://cdn.thegeekdiary.com/wp-content/uploads/2017/11/Files-permissions-and-ownership-basics-in-Linux.png" width=400>
</p>

### Permission Updates

Using the **`chmod`** command you can update file and directory permissions in two ways:

* Using letters:

    * `chmod a+x filename` - *adds* (`+`) *executable* (`x`) permissions to *all* (`a`)
    * `chmod go-wx filename` - *removes* (`-`) *write and executable* (`wx`) permissions to *group and other* (`go`)
    * Note the format - first goes the users (`u`, `g`, `o`, or `a`), then add or remove (`+` or `-`), and finally the permissions (`r`, `w`, `x`)


* Using numbers (much more common)

    * `chmod 640 filename` - changes the User to (`rw`), the Group to (`r`), and others have no permissions

<p align="center">
<img src="https://helpdeskgeek.com/wp-content/pictures/2017/02/octal-linux-permissions.png.webp" width=300>
<img src="https://helpdeskgeek.com/wp-content/pictures/2017/02/octal-permissions-explained.png.webp" width=300>
</p>


## Structure

Unlike Windows, everything in Linux is based off the top level `/` directory. Each directory below it is like `C:`, 

<p align="center">
<img src="https://averagelinuxuser.com/assets/images/posts/2018-11-15-linux-root-folders-explained/26-1.jpeg" width=500>
</p>

Everything is a file in Linux. 

Below is from an awesome tutorial [here](https://averagelinuxuser.com/linux-root-folders-explained/)

* **`/bin`** - contains *binary* files (executables - really fast) that boot Linux. Without this, the system won't start
* `/boot` - contains files and drivers for the distribution you are running
* `/dev` - list all the devices (including harddrives, CPU, USB devices) connected to Linux, represented as files (everything is a file)
* `/etc`
* **`/home`** - home directory **for each user** conyaining "normal" files like documents, pictures, etc.
    * `/home/craig/` is the home folder for user craig and contains craig's private data (images, documents, etc.)
    * this is the **default folder** when you open a terminal
* `/lib` - libraries (set of functions) required by programs in `/bin`
* `/media` and `/mnt` - automatic and manual mounting of media devices (CR-ROM, USB)
* **`/opt`** - non essential, optional software (ie. Dropbox). However **conda install packages here** in `/opt/conda/bin/`
* `/proc`
* **`/root`** - same as the `/home` directory except **for the root user**. Note this is note the same as root `/` - this is for the root *user*
    * This is the **default account** if you **log in as root**
    * Contains the private data of the *root user* (again images, documents, etc)
* `/run` - temporary files used early in the boot
* `/sbin` - same as `/bin` but has additional files for super user (administrator)
* `/srv` - services installed on your system (ie. a web server like NGINX)
* `/tmp` - temporary folders cleaned on reboot
* **`/usr`** - the largest folder after `/home`, this contains **all the programs used by regular uses** (hence this is where you go to find anaconda virtual environments)
    * `/usr/bin` - programs installed by distro (thousands)
    * `/usr/lib` - contains libraries for `/usr/bin`
    * `/usr/local`
    * `/usr/share` - most useful; contains shared data used by `/usr/bin` (themes, icons, wallpapers, sound files, config files)
        * `/usr/share/doc` - contains *documentation* for programs installed on the system
* `/var` - files that change all the time, like **logs** (`/var/log`)

## Scripts

Creating shell scripts can make using Linux very speedy. 

* **`echo`** simply outputs the strings it is passed

A note on `/bin/bash` vs `/bin/sh` - they are basically the same. `/bash` is a shell and `/sh` is a *symlink* to the main system shell. `bash` is more common today because it is used in distributions like Ubuntu, however given the symlink nature of `/sh`, using it can ensure the script runs on any UNIX system (some don't use `bash`). For our case, we will use `bash` since we are using Ubuntu

* An executable script should have `.sh` extension. 

* It should also have `#!/bin/bash` at the top - this identifies the file as an executable
    * Below this, you add **any commands you would normally type in the command line**
```
#!/bin/bash

hugo

git add .

git commit -m "Updates"

git push
```

* You may need to update the file permissions to make it executable `chmod u+x deploy.sh`

### Script generation

You can do this using vim if you have a multiline script. However if you want to do something quick (ie. repetitive task like launching Jupyter) you can do the following:

`echo -e '#!/bin/bash\ndocker exec -it kidney-cv jupyter lab --ip=0.0.0.0 --no-browser --NotebookApp.token="" --NotebookApp.password=""'>run_lab.sh`
* the `-e` is important as it reads the newline `\n` (might be just Ubuntu 18.02 as I'm not seeing this issue past 2016/17 in forums)
* the `>` creates a new `run_lab.sh` file every time. You could use double arrows `>>` to *add* to a file if it exists (or creates if it doesn't) 
* you call this script by being in the *same directory*: `bash ./run_lab.sh` or `sh ./run_lab.sh`
    * note that the `./` beforehand is needed in combination with `#!/bin/bash`, but this makes the scripts more available across distros