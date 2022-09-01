Hardening Project (WIP)
=================

Contains sysctl config files, grub boot parameters and a custom Kernel configuration used to harden all my (debian) Linux installs.

* grub.d: contains grub configuration files adding extra boot parameters.
* sysctl.d: contains sysctl.conf files to tweak kernel settings.
* kernel.config: configuration file to build a custom hardened kernel (Debian specific, for now)
* kernel.diff: only shows the differences between standard Debian kernel and this one. 

Files in grub.d and sysctl.d are distro agnostic, though they may need to be adjusted depending on the distribution. e.g. Debian enables init_on_alloc within the kernel, so no need to pass it as a boot parameter. On distros that don't enable it, add it to grub.d/01_hardening.conf with _init_on_alloc=1_.

Usage
=====

Carefully read and understand what any of these options do before proceeding.

Sysctl & Boot Parameters
------------------------

* Copying sysctl configs

  ```
  cp /hardening-project/sysctl.d/{01-hardening.conf,02-hardening_net.conf} /etc/sysctl.d/
  ```

* Enable boot paramters

  ```
  cp /hardening-project/grub.d/01_hardening.cfg /etc/default/grub.d/
  update-grub
  ```
* Reboot. Done.

Kernel.config
-------------

The current kernel config is only meant for Debian Bullseye. It is built using LLVM/Clang with the [linux-source](https://packages.debian.org/bullseye/linux-source) package. A .deb package is coming soon (to be used with my [Debian live iso](https://github.com/I-LeCorbeau/debian-live-build/releases), and for those who are rather trusting and don't want to compile the kernel themselves).

Kernel building instructions are coming soon, but the gist: after installing the linux-source package and its build dependencies, install the llvm, clang and lld packages, then extract the kernel source (from /usr/src/) wherever you please, copy kernel.config to the extracted source dir as .config, run __make CC=clang LLVM=1 -j$(nproc) bindeb-pkg__.
