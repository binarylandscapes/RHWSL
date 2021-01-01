# RHWSL (Red hat UBI on WSL)
Red hat redistributable Standard UBI on WSL (Windows 10 FCU or later)
based on [wsldl](https://github.com/yuk7/wsldl)

![screenshot](https://raw.githubusercontent.com/binarylandscapes/RHWSL/master/img/screenshot.png)

[![Build Zip Actions Status](https://github.com/binarylandscapes/RHWSL/workflows/Build zip CI/badge.svg)](https://github.com/binarylandscapes/RHWSL/actions)
[![Release Zip Actions Status](https://github.com/binarylandscapes/RHWSL/workflows/Release zip CI/badge.svg)](https://github.com/binarylandscapes/RHWSL/actions)
[![Github All Releases](https://img.shields.io/github/downloads/binarylandscapes/RHWSL/total.svg?style=flat-square)](https://github.com/binarylandscapes/RHWSL/releases)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)
![License](https://img.shields.io/github/license/binarylandscapes/RHWSL.svg?style=flat-square)

## [Download](https://github.com/binarylandscapes/RHWSL/releases)


## Requirements

* Windows 10 1903 April 2018 Update x64 or later.
* Windows Subsystem for Linux feature is enabled.

---
**IMPORTANT**

Be aware that if installing any WSL instance on Windows 10 1803+, your system automatically is configured with NTFS "Case Sensitive" enabled for any folder created by the WSL instance. This may have issues with Windows usage of files in those folders.

[Per-directory case sensitivity and WSL](https://blogs.msdn.microsoft.com/commandline/2018/02/28/per-directory-case-sensitivity-and-wsl/)

[Improved per-directory case sensitivity support in WSL](https://devblogs.microsoft.com/commandline/improved-per-directory-case-sensitivity-support-in-wsl/)

If you are currently on Windows 10 2004 for the Insiders Program and planning to use WSL 2 and get the following error message "HRESULT:0x800701bc", then you will need to update your WSL Linux Kernel per (https://docs.microsoft.com/en-us/windows/wsl/wsl2-kernel)

---

## References

* [Microsoft WSL Documentation](https://docs.microsoft.com/en-us/windows/wsl/about)

## Install

### 1. [Download](https://github.com/binarylandscapes/RHWSL/releases) RHWSL.zip

### 2. Extract zip file to a new RHWSL directory containing all files (Recommend C:\TEMP or Downloads folder)

### 3.Run RHWSL.exe to Extract rootfs and Register to WSL
Exe filename is using to the instance name to register.
If you rename it you can register with a diffrent name and have multiple installs.

### Subscription Manager
- The rootfs included in the release file is the redistributable Standard __"Universal Base Image"__.  
  __However, you can register as usual using subscription-manager and use the RHEL repositories.__
```sh
[root@<yourhost> RHWSL]# subscription-manager register
You are attempting to use a locale that is not installed.
Registering to: subscription.rhsm.redhat.com:443/subscription
Username: <yourusername>
Password: <yourpassword>
The system has been registered with ID: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
The registered system name is: <yourhost>
[root@<yourhost> RHWSL]# subscription-manager attach
You are attempting to use a locale that is not installed.
Installed Product Current Status:
Product Name: Red Hat Enterprise Linux for x86_64
Status:       Subscribed
```

## How-to-Use (for Installed Instance)

### exe Usage (Based off wsldl)

```cmd
Usage:

    <no args>
      - Open a new shell with your default settings.

    run <command line>
      - Run the given command line in that distro. Inherit current directory.

    runp <command line (includes windows path)>
      - Run the path translated command line in that distro.

    config [setting [value]]
      - `--default-user <user>`: Set the default user for this distro to <user>
      - `--default-uid <uid>`: Set the default user uid for this distro to <uid>
      - `--append-path <on|off>`: Switch of Append Windows PATH to $PATH
      - `--mount-drive <on|off>`: Switch of Mount drives
      - `--default-term <default|wt|flute>`: Set default terminal window

    get [setting]
      - `--default-uid`: Get the default user uid in this distro
      - `--append-path`: Get on/off status of Append Windows PATH to $PATH
      - `--mount-drive`: Get on/off status of Mount drives
      - `--wsl-version`: Get WSL Version 1/2 for this distro
      - `--default-term`: Get Default Terminal for this distro launcher
      - `--lxguid`: Get WSL GUID key for this distro

    backup [contents]
      - `--tgz`: Output backup.tar.gz to the current directory using tar command
      - `--reg`: Output settings registry file to the current directory

    clean
      - Uninstall the distro.

    help
      - Print this usage message.
```

#### Set "Windows Terminal" as default terminal

```cmd
<DistributionName>.exe config --default-term wt
```

### How to uninstall instance

```cmd
<DistributionName>.exe clean

```

### Helpful tips

* The commands `bash` or `wsl` will open your default distro of WSL as well

* If you forgot your password, `wsl --distribution <DistributionName> --user root` will open the distro as root. So you can use passwd <user> to reset. Then close all terminals and reopen Alpine normally under your user account with new password.

* If you need to virtually "reboot" the WSL distro or distros, as an Administrator open Services and restart the running LxssManager service. 


### WSL Command Line Reference

See [Microsoft WSL Reference Documentation](https://docs.microsoft.com/en-us/windows/wsl/reference)

```cmd
Usage: wsl.exe [Argument] [Options...] [CommandLine]
```

#### Arguments to run Linux binaries

If no command line is provided, wsl.exe launches the default shell.

`--exec, -e <CommandLine>`: Execute the specified command without using the default Linux shell.

`-- <CommandLine>`: Pass the remaining command line as is.

  Options:

  `--distribution, -d <DistributionName>`: Run the specified distribution.

  `--user, -u <UserName>`: Run as the specified user.

#### Arguments to manage Windows Subsystem for Linux

`--export <DistributionName> <FileName>`: Exports the distribution to a tar file.
                                          The filename can be - for standard output.

`--import <DistributionName> <InstallLocation> <FileName>`: Imports the specified tar file as a new distribution.
                                                            The filename can be - for standard input.

`--list, -l [Options]`: Lists distributions.

  Options:

  `--all`: List all distributions, including distributions that are currently
           being installed or uninstalled.

  `--running`: List only distributions that are currently running.

  `--verbose`: Lists which version of WSL for distributions.

  `--set-default, -s <DistributionName>`: Sets the distribution as the default.

  `--set-default-version <wslVersion>`: Sets the default WSL version for newly created distributions.

  `--set-version <DistributionName> <wslVersion>`: Sets the WSL version for distribution.

  `--terminate, -t <DistributionName>`: Terminates the distribution.

  `--unregister <DistributionName>`: Unregisters the distribution.

  `--upgrade <DistributionName>`: Upgrades the distribution to the WslFs file system format.

  `--help`: Display usage information.

## How-to-Build

CentWSL can build on GNU/Linux or WSL.

`curl`, `bsdtar`, `tar`(gnu) and `sudo` is required for build.

```shell
$ make
```
