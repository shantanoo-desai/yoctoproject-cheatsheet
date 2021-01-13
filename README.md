# Yocto Project Cheatsheet

A summary of all things one might need to work with [Yocto Project](https://yoctoproject.org).
One stop repository for Yocto since there are so many variables and CLIs to work with.

## Contributing

Please feel free to add / adapt the list accordingly via PRs. As OE/Yocto Project still has a steep learning-curve,
a better way would be to share resources with the community.

## Commands

All hints for CLIs like `bitbake`, `bitbake-layers`, `devtool` etc. used within common Yocto Project development

| Purpose                       | Command(s)                                                                       |
|:------------------------------|----------------------------------------------------------------------------------|
| __Populate Classic SDK__      |   `bitbake -c populate_sdk <IMAGE_NAME>`                                         |
| __Populate Extensible SDK__   |   `bitbake -c populate_sdk_ext <IMAGE_NAME>`                                     |
| __Start build from scratch__  |   `cd $BUILD_DIR && rm -Rf tmp sstate-cache`                                     |
| __Supported HW Boards__       |   `ls sources/meta-<hardware>*/recipes*/conf/machine/*.conf`                     |
| __Supported Images__          |   `ls sources/meta-<hardware>*/recipes*/images/*.bb`                             |
| __Single PR Server instance__ |   `bitbake-prserv --host <server_ip> --port <port> --start`                      |
| __Find a package in a layer__ |   `cd sources && find --name "*busybox*"`                                        |
| __Find a recipe in a layer__  |   `cd sources && find --name "*busybox*.bb*"`                                    |
| __Search recipe__             |   `bitbake-layers show-recipes "gdb*"`                                           |
| __Find dependency cache__     |   `devtool search <RegEx>`                                                       |
| __Dump global env & find__    |   `bitbake -e`<code>&#124;</code>`grep -w DISTRO_FEATURES`                       |
| __Locate source directory__   |   `bitbake -e <recipe>`<code>&#124;</code>`grep ^S=`                             |
| __Locate working directory__  |   `bitbake -e <recipe>`<code>&#124;</code>`grep ^WORKDIR=`                       |
| `devshell`                    |   `bitbake -c devshell <target>`                                                 |
| `devpyshell`                  |   `bitbake -c devpyshell <target>`                                               |
| __List tasks for a recipe__   |   `bitbake -c listtasks <target>`                                                |
| __Force a build__             |   `bitbake -f <target>`                                                          |
| __Force-run a specific task__ |   `bitbake -c compile -f <target>`                                               |
| __Current/given pkg version__ |   `bitbake --show-versions`                                                      |
| __Verbose output__            |   `bitbake -v <target>`                                                          |
| __Display debug information__ |   `bitbake -DDD <target>`                                                        |
| __Send error report__         |   `send-error-report ${LOG_DIR}/error-report/error-report_${TSTAMP}`             |
| __Create a new layer__        |   `yocto-layer create <layer_name>`                                              |
| __Add custom layer__          |   `bitbake-layers add-layer /path/to/your_meta-layer`                            |
| __Remove custom layer__       |   `bitbake-layers remove-layer /path/to/your_meta-layer`                         |
| __Find all recipe layers__    |   `bitbake-layers show-recipes`                                                  |
| __Find all clashing recipe__  |   `bitbake-layers show-overlayed`                                                |
| __Show all `bbappend` files__ |   `bitbake-layers show-appends`                                                  |
| __Flatten all Layers__        |   `bitbake-layers flatten <output_dir>`                                          |
| __Show recipe cross-depends__ |   `bitbake-layers show-cross-depends`                                            |
| __List layer dependencies from OE Index__ |   `bitbake-layers layerindex-show-depends <layer_name>`              |
| __Fetch & add layer using OE Index__      |   `bitbake-layers layerindex-fetch <layer name>`                     |




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
| Persistent Bitbake server     | `local.conf` | `BB_SERVER_TIMEOUT= "n" # n in seconds`                                                    |
| __Remove__ build packages     | `local.conf` | `INHERIT += "rm_work"`                                                                     |
| Exclude packages from cleaning| `local.conf` | `RM_WORK_EXCLUDE += "recipe_name"`                                                         |
| Avoid `-dbg` packages         | `local.conf` | `INHIBIT_PACKAGE_DEBUG_SPLIT = "1"`                                                        |
| Accept FSL's EULA license     | `local.conf` | `ACCEPT_FSL_EULA = "1"`                                                                    |
| Download Directory            | `local.conf` | `DL_DIR ?= "${BSP_DIR}/downloads/"`                                                        |
| Configuring a Pre-mirror      | `local.conf` | `INHERIT += "own-mirrors"` <br> `SOURCE_MIRROR_URL = "http://example.com/source-mirror"`   |
| *tarballs* of git repos.      | `local.conf` | `BB_GENERATE_MIRROR_TARBALLS = "1"`                                                        |
| Build using Pre-mirrors only  | `local.conf` | `BB_FETCH_PREMIRRORONLY = "1"`                                                             |
| _Add_ Package Mgmt. System    | `local.conf` | `EXTRA_IMAGE_FEATURES += "package-management"`                                             |
| Simple `PR` server            | `local.conf` | `PRSERV_HOST = "<server_ip>:<port>"`                                                       |
| Enable build history          | `local.conf` | `INHERIT += "buildhistory"`                                                                |
| Store build history in repo.  | `local.conf` | `BUILDHISTORY_COMMIT = "1"`                                                                |
| Track image, pkg, SDKs change | `local.conf` | `BUILDHISTORY_FEATURES = "image" # package or sdk`                                         |
| Track a file for buildhistory | `local.conf` | `BUILDHISTORY_IMAGE_FILES += "/path/to/file"`                                              |
| Terminal for `dev(py)shell`   | `local.conf` | `OE_TERMINAL = "screen"`                                                                   |
| Submit Failed build error     | `local.conf` | `INHERIT += "report-error"`                                                                |
| Number of parallel tasks      | `local.conf` | `BB_NUMBER_THREADS ?= "${@oe.utils.cpu_count()}"`                                          |
| Value of `-j` in `make`       | `local.conf` | `PARALLEL_MAKE ?= "-j ${@oe.utils.cpu_count()}"`                                           |
| Add Real-Time Kernel          | `local.conf` | `PREFERRED_PROVIDER_virtual/kernel = "<RT_Kernel_Image_name_from_Meta-Layer>"`             |


## Misc.

All miscellaneous resources once can find for understanding the Yocto Project and working with it.

### Documentation / Websites

* [`repo` Command Reference](https://source.android.com/setup/develop/repo)
* [OpenEmbedded Layers Index](http://layers.openembedded.org)
* [OpenEmbedded Errors Index](http://errors.openembedded.org)

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
* [Paul Barker's YouTube Channel/ Rust with Yocto Project](https://www.youtube.com/channel/UCvnVQTiuS9-1dxZI-SJGBRA)
