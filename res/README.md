# Patch mochad 0.1.17

Instead of downloading this repository, one could download the latest version of `mochad` by the original author mmauka, and update `global.h` and `global.c` with the two patch files in this directory. Here is a long `bash` command that will download the source and patches, and apply the latter.


```
wget -O mochad-0.1.17.tar.gz https://sourceforge.net/projects/mochad/files/mochad-0.1.17.tar.gz/download ; \
tar xzf mochad-0.1.17.tar.gz ; \
cd mochad-0.1.17 ; \
wget https://raw.githubusercontent.com/sigmdel/mochad/master/res/patch-global.c.diff ; \
patch global.c patch-global.c.diff ; \
wget https://raw.githubusercontent.com/sigmdel/mochad/master/res/patch-global.h.diff ; \
patch global.h patch-global.h.diff 
```


Carry on with `./configure`, `make`, `sudo make install` to complete the installation, but do not forget to install the prerequisite library, `libusb-1.0-0-dev`, before compiling the source code. Note that there is no need to install `autoconf` in Raspberry Pi OS because the `configure` script is used here instead of the `autogen.sh` script.
