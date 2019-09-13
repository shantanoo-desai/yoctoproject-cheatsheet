# Yocto Project Cheatsheet

A summary of all things one might need to work with [Yocto Project](https://yoctoproject.org).
One stop repository for Yocto since there are so many variables and CLIs to work with.


## Configuration

All variables that can be added to files like `bblayers.conf` or `local.conf` or other configuration files in Yocto.

| Purpose                       | File         | Variable(s)                                                                                |
|:------------------------------|:-------------|--------------------------------------------------------------------------------------------|
| __Add__ `systemd`             | `local.conf` | `DISTRO_FEATURES_append = " systemd"` <br> `VIRTUAL-RUNTIME_init_manager = "systemd"`      |
| __Remove__ `sysvinit`         | `local.conf` | `DISTRO_FEATURES_BACKFILL_CONSIDERED = "sysvinit"` <br> `VIRTUAL-RUNTIME_initscripts = ""` |
| __Add__ `strace`              | `local.conf` | `CORE_IMAGE_EXTRA_INSTALL += "strace"`                                                     |
| `root` __Login w/o Password__ | `local.conf` | `EXTRA_IMAGE_FEATURES ?= "debug-tweaks"`                                                   |
| __Add__ `gcc`                 | `local.conf` | `EXTRA_IMAGE_FEATURES ?= "tools-sdk"`                                                      |
| __Add__ `dropbear-ssh-server` | `local.conf` | `EXTRA_IMAGE_FEATURES ?= "ssh-server-dropbear"`                                            |
| __Add__ `virtualization`      | `local.conf` | `DISTRO_FEATURES_append = " virtualization"`                                               |

## Commands

All hints for CLIs like `bitbake`, `bitbake-layers`, `devtool` etc. used within common Yocto Project development

| Purpose                       | Command(s)                                                                       |
|:------------------------------|----------------------------------------------------------------------------------|
| __Populate Classic SDK__      |   `bitbake -c populate_sdk <IMAGE_NAME>`                                         |
| __Populate Extensible SDK__   |   `bitbake -c populate_sdk_ext <IMAGE_NAME>`                                     |

## Misc.

All miscellaneous resources once can find for understanding the Yocto Project and working with it.

### Documentation


### Repositories

* [yocto-research by @davisRoman](https://github.com/davisRoman/yocto-research)
* [meta-diet layer, part of Blog by Johan Thelin (@e8johan)](https://github.com/e8johan/meta-diet)


### Blogs
* Blogs by Johan Thelin (@e8johan)
    - [Yocto part I – baseline boot time](http://www.thelins.se/johan/blog/2014/06/yocto-part-i-baseline-boot-time/)
    - [Yocto part II – baseline image size](http://www.thelins.se/johan/blog/2014/06/yocto-part-ii-baseline-image-size/)
    - [Yocto part III – a custom meta layer](http://www.thelins.se/johan/blog/2014/06/yocto-part-iii-a-custom-meta-layer/)
    - [Yocto part IV – going on a diet](http://www.thelins.se/johan/blog/2014/06/yocto-part-iv-going-on-a-diet/)
* Blog by Khem Raj (@kraj)
    - [Bake 64-bit Raspberry Pi 3 images with Yocto, Openembedded](https://himvis.com/bake-64-bit-raspberrypi3-images-with-yoctoopenembedded/)


### Videos

* [Yocto Project YouTube Channel](https://www.youtube.com/user/TheYoctoProject)
* [Live Coding with Yocto Project Playlist](https://www.youtube.com/playlist?list=PLD4M5FoHz-TxMfBFrDKfIS_GLY25Qsfyj)