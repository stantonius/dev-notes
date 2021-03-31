# Craig's Windows Setup Notes

## Install Checklist

I'm not sure when I will do this again, so I am taking notes

* Python
    * Use **Miniconda** for Python install and dependencies - don't install Python on its own
* Flutter
    * Go to the official FLutter page and download the latest zip file (for me I always use the beta channel)
    * Extract the zipped file to a folder (I create a Development\Flutter folder for this purpose)
    * Then edit the environmental variables to *point to the Flutter `bin` folder*
* Git
    * Install the latest Windows version from the [official site ](https://git-scm.com/) via the .exe file
    * Run `git init` in the folder you want to add a remote repo
* NodeJS
    * Download the .exe file and install it and all dependencies. It installs a lot - including Python, Visual Studio, and Chocolatey
* Firebase CLI
    * Can use npm to install it in one step


## Common Commands

Unzip
* `Compress-Archive -LiteralPath <PathToFolder> -DestinationPath <PathToDestination>`
    * You dont need the tags above either, just the paths