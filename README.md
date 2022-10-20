# bash script to pack openwrt ipkg file by hand

An IPK package is very simple. It's like a .deb debian package, as in that is has both data and control files packaged up into an archive. The data will be extracted onto the filesystem where the package is installed, the control files are used for dependency management and to execute pre and post install actions.

In my case, the postinst script is used to start the service (the binary we're packaging up). The prerm script is used to stop the service and disable it before uninstalling the package. The postinst script is used to check if the serial number matches the machine.

An ipk is an archive (either tar or ar or gzip) containing two archives (control.tar.gz & data.tar.gz) and a debian-binary file with the contents 2.0:

# Folder structure & Data

   The following folder structure is used for the package build. There is a main folder named packages, which has a subfolder for each machine based on the machines serial number. Under the machine folder there is a folder named after the package we're building (examplepackage), which has a control and data folder. The data folder contains the files that will be extracted on the filesystem and the control folder contains the pre and post scripts and some package information.

```bash
packages/serialnumber/
|-- ipkbuild
|   `-- example_package
|       |-- control
|       |   |-- control
|       |   |-- postinst
|       |   |-- preinst
|       |   `-- prerm
|       |-- data
|       |   |-- usr
|       |   |   `-- bin
|       |   |       `-- my_binary
|       |   `-- lib
|       |       `-- systemd
|       |           `-- system
|       |               `-- example_package.service
|       `-- debian-binary
`-- example_package_1.3.3.7_varam335x.ipk
```

# how to pack the folder into *.ipk

```bash
$ cd package/example_package
$ mkipkg -d . -D ../
$ ls *.ipk
$ example_package_1.3.3.7_aarch64-cortex-a53.ipk
```
