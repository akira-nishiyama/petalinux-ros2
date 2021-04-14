# petalinux-ros2
This repository provides Repo manifests to setup the Yocto build system for petalinux with ros2.

# Setup
Install Repo.

```{class="prettyprint lang-sh"}
curl -k https://storage.googleapis.com/git-repo-downloads/repo > repo
chmod a+x repo
sudo mv repo /usr/local/bin/
```

fetch all repos and setup yocto environment.

```{class="prettyprint lang-sh"}
cd <petalinux-project-path>
repo init -u http://github.com/akira-nishiyama/petalinux-ros2-manifests -b feature-rel-v2020.2
repo sync
source setupsdk
```

amend local.conf to point xsa file.
(QB_TAP_OPT variable is option. used in runqemu command.)

```{class="prettyprint lang-none"}
HDF_EXT_ultra96v2-zynqmp = "xsa"
HDF_BASE_ultra96v2-zynqmp = "file://"
HDF_PATH_ultra96v2-zynqmp = "<xsa-file-path>/<yourdesign.xsa>"
QB_TAP_OPT_forcevariable = "-netdev tap,id=net0,ifname=tap0"
```

If you don't have xsa file, use [this](https://github.com/akira-nishiyama/ultra96v2_4z) project.
To build xsa, run below command.

```{class="prettyprint lang-sh"}
cd
git clone https://github.com/akira-nishiyama/vivado_cmake_helper.git
gic clone https://github.com/akira-nishiyama/ultra96v2_4z
source <path>/<to>/Vivado/2019.2/settings.sh
source vivado_cmake_helper/setup.sh
cd ultra96v2_4z
mkdir build
cd build
cmake .. -DCMAKE_INSTALL_PREFIX=<xsa-file-path>
make
make install
```

# Build

run below command.
```{class="prettyprint lang-sh"}
cd <petalinux-project-path>/build
MACHINE=ultra96v2-zynqmp bitbake petalinux-image-minimal-ultra96v2hw
```
