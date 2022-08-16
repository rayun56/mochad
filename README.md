# MOCHAD

`mochad` is a Linux TCP gateway daemon for the X10 CM15A and CM19A RF (radio frequency) and
PL (power line) controllers. 


## Why this fork

Changes were needed to the [Neil Cherry fork](https://github.com/linuxha/mochad) to compile and install `mochad` on recent 64 bit versions of Linux with the `systemd` init system. Specifically these have been tested on 

- x86_64 GNU/Linux : Mint 20.1 Ubuntu 20.04 LTS (Focal) Linux 5.4.0-124

- aarch64 GNU/LInux : Armbian 22.05.3 Ubuntu 22.04.1 LTS (Jammy) Linux 5.10.123-meson64 

## Changes

### Source

`mochad` could not be built because of the following linking errors

    /usr/bin/ld: decode.o:/home/hestia/mochad-master/global.h:31: multiple definition of `RfToRf16'; mochad.o:/home/hestia/mochad-master/global.h:31: first defined here
    /usr/bin/ld: decode.o:/home/hestia/mochad-master/global.h:29: multiple definition of `RfToPl16'; mochad.o:/home/hestia/mochad-master/global.h:29: first defined here
    /usr/bin/ld: decode.o:/home/hestia/mochad-master/global.h:26: multiple definition of `PollTimeOut'; mochad.o:/home/hestia/mochad-master/global.h:26: first defined here
    /usr/bin/ld: decode.o:/home/hestia/mochad-master/global.h:25: multiple definition of `Cm19a'; mochad.o:/home/hestia/mochad-master/global.h:25: first defined here

The problem was solved by declaring those variables as `extern` in `global.h` and defining `PollTimeOut` in `global.c`.

### Configuration

The `mochad` service was not installed properly in `systemd` and it would stop functioning with a 

    usb_claim_interface failed -6

error. 

The solution was to bring back the installation provided by the original author nnauka in [mochad-0.1.17](https://sourceforge.net/projects/mochad/files/).


## Installation of `mochad`

### Prerequisite

    sudo apt install libusb-1.0-0-dev

### Getting the source

The source code on GitHub can be obtained with any one of the usual methods. Notably

#### by cloning the repository

```
  $ git clone https://github.com/sigmdel/mochad.git
  $ cd mochad
```

#### by downloading the archive

```
  $ wget https://github.com/sigmdel/mochad/archive/refs/heads/master.zip
  $ unzip master.zip
  $ cd mochad-master
```    

### Compiling the source

From within the directory containing the source

```
    $ ./autogen.sh
    $ make
```

While it should be, make sure `autogen.sh` is an executable

```
    $ chmod +x autogen.sh 
```
if needed.    

### Installing the package

```
    $ sudo make install
```

Again within the directory containing the source.

### Installed files


```
   /etc/local/bin/mochad
   /etc/udev/rules/91-usb-x10-controllers.rules
```

In `systemd` the service file is also installed

```
   /etc/systemd/system/mochad.service 
```

Once the installation is completed, all the other downloaded and generated files can be deleted if desired.

## More information

The [original README](README) text file contains much more information.

## History and Attributions

The last version of`mochad` by its original author, mmauka, is version 0.1.17 [published on Sourceforge in 2016-06-10](https://sourceforge.net/projects/mochad/files/).  That version will no longer compile on Ubuntu 20.04 because of an outdated `configure` script. 

In 2010, Boris Jonica (bjonica) created a GitHub repository forking version 0.1.14. A year later, an `autogen` script was added which generates a `configure` script which will allow compilation under recent Linux distributions. This is actually a contribution by Nicholas Humfrey (njh). The [bjonica/mochad repository](https://github.com/bjonica/mochad) has been updated to incorporate version 0.1.17 of `mochad` by mmauka except for the files needed in `system.d` for some reason.

Neil Cherry (linuxha) forked the bjonica repository and released what is labelled as version 0.1.18. It adds optional support for IPV6 although this is not enabled by default (see the IPV6 macro at the start of `mochad.c`). Again the `mochad.service` file and associated `udev` rules needed in `systemd` were not included.

The source of the `usb_claim_interface failed -6` problem in using `mochad` with `systemd` was identified by [Vicente](https://sourceforge.net/p/mochad/discussion/1320002/thread/1bef8f39/#8250) back in 2016-04-07. Patrick Kuijvenhoven (petski) provided a working `systemd.service` file in a [2016-04-15](https://github.com/petski/mochad/tree/c46f2f789d795d6140e24ee9b25656e180d23bd8) commit of a fork of the SourceForge code. There were no `udev` rules and his [2016-04-15 post](https://sourceforge.net/p/mochad/discussion/1320002/thread/1bef8f39/#1f85) on SourceForge has a line deleting the previous `udev` rules. Presumably, the service file was to be enabled and hence loaded at boot time. 

As already stated, version 0.1.17 by mmauka already had the solution which was for the most part reintroduced in this fork. The version of `mochad.service` found in this fork comes from Andreas' [2021-09-07 post](https://sourceforge.net/p/mochad/discussion/1320002/thread/764dd1ce44/#76e9) on the grounds that the unit file looks more sophisticated to me... but then what do I know?

## License

GNU General Public License version 3.0 (GPLv3) according to the [original project page on SourceForge](https://sourceforge.net/projects/mochad/).
