<div align="center">
<img src=".github/logo.png" alt="Quickemu" width="256" />

# Quickemu

**Quickly create and run optimised Windows, macOS and Linux virtual machines:**

**Made with 💝 for <img src=".github/tux.png" align="top" width="24" alt="Tux (Linux)"/> & <img src=".github/apple.png" align="top" width="24" alt="Apple (macOS)"/>**
</div>

<p align="center">
  &nbsp;<a href="https://wimpysworld.io/discord" target="_blank"><img alt="Discord" src="https://img.shields.io/discord/712850672223125565?style=for-the-badge&logo=discord&logoColor=%23ffffff&label=Discord&labelColor=%234253e8&color=%23e4e2e2"></a>&nbsp;
  &nbsp;<a href="https://fosstodon.org/@wimpy" target="_blank"><img alt="Mastodon" src="https://img.shields.io/badge/Mastodon-6468fa?style=for-the-badge&logo=mastodon&logoColor=%23ffffff"></a>&nbsp;
  &nbsp;<a href="https://twitter.com/m_wimpress" target="_blank"><img alt="Twitter" src="https://img.shields.io/badge/Twitter-303030?style=for-the-badge&logo=x&logoColor=%23ffffff"></a>&nbsp;
  &nbsp;<a href="https://linkedin.com/in/martinwimpress" target="_blank"><img alt="LinkedIn" src="https://img.shields.io/badge/LinkedIn-1667be?style=for-the-badge&logo=linkedin&logoColor=%23ffffff"></a>&nbsp;
</p>

# About this fork

This is [ilikehamradio/quickemu](https://github.com/ilikehamradio/quickemu),
a fork of [quickemu-project/quickemu](https://github.com/quickemu-project/quickemu).
It differs from upstream in two ways:

- `quickget` uses **`zsync2`** when classic `zsync` isn't installed. Arch-based
  distros without Arch's `[extra]` repo (such as Artix) only have `zsync2`,
  from the AUR.
- An Arch package in [`packaging/arch/`](packaging/arch/PKGBUILD) depends on
  `zsync2` instead of `zsync`.

A GitHub Action checks upstream every 6 hours and merges any new changes into
this fork automatically. If a merge ever conflicts, it opens an issue instead.

## Install on Arch / Artix / Manjaro

Requires [paru](https://github.com/Morganamilo/paru) (or another AUR helper)
for the `zsync2` dependency. Run these in a terminal:

```shell
git clone https://github.com/ilikehamradio/quickemu.git
cd quickemu
paru -Bi packaging/arch
```

Artix users without the Arch repos enabled: install `swtpm` and `xorg-xrandr`
from Artix's `[world]` repo first if paru can't find them:

```shell
sudo pacman -S --needed swtpm xorg-xrandr
```

## Update

The fork on GitHub updates itself, but your installed copy doesn't. To pull
the latest version and reinstall:

```shell
cd quickemu
git pull
paru -Bi packaging/arch
```

## Other distros

On other distros, follow the upstream install instructions in the
[documentation](#documentation) below. Everything else works as it does
upstream.

# Introduction

**Quickemu** is a wrapper for the excellent [QEMU](https://www.qemu.org/) that
automatically *"does the right thing"* when creating virtual machines. No
requirement for exhaustive configuration options. You decide what operating
system you want to run and Quickemu takes care of the rest 🤖

- `quickget` **automatically downloads the upstream OS** and creates the configuration 📀
- `quickemu` enumerates your hardware and launches the virtual machine with the **optimum configuration best suited to your computer** ⚡️

The original objective of the project was to [enable quick testing of Linux
distributions](https://github.com/quickemu-project/quickemu/wiki/02-Create-Linux-virtual-machines)
where the virtual machines and their configuration can be stored anywhere (such
as external USB storage or your home directory) and no elevated permissions are
required to run the virtual machines.

**Today, Quickemu includes comprehensive support for [macOS](https://github.com/quickemu-project/quickemu/wiki/03-Create-macOS-virtual-machines),
[Windows](https://github.com/quickemu-project/quickemu/wiki/04-Create-Windows-virtual-machines)**, most of the BSDs, novel non-Linux operating systems such as FreeDOS, Haiku, KolibriOS, OpenIndiana, ReactOS, and more.

# Features

- Host support for **Linux and macOS**
- **macOS** Sequoia, Sonoma, Ventura, Monterey, Big Sur, Catalina & Mojave
- **Windows** 10 and 11 including TPM 2.0
- **Windows Server** 2022 2019 2016
- **ARM64 guest support** for running aarch64 VMs (native on ARM hosts, emulated on x86_64)
- [Ubuntu](https://ubuntu.com/desktop) and all the **[official Ubuntu
  flavours](https://ubuntu.com/download/flavours)**
- **Nearly 1000 operating system editions are supported!**
- Full SPICE support including host/guest clipboard sharing
- VirtIO-webdavd file sharing for Linux and Windows guests
- VirtIO-9p file sharing for Linux and macOS guests
- [QEMU Guest Agent
  support](https://wiki.qemu.org/Features/GuestAgent); provides access
  to a system-level agent via standard QMP commands
- Samba file sharing for Linux, macOS and Windows guests (*if `smbd`
  is installed on the host*)
- VirGL acceleration
- USB device pass-through
- Smartcard pass-through
- Automatic SSH port forwarding to guests
- Network port forwarding
- Full duplex audio
- Braille support
- EFI (with or without SecureBoot) and Legacy BIOS boot

## As featured on [Linux Matters](https://linuxmatters.sh) podcast!

The presenters of Linux Matters 🐧🎙️ are the creators of each of the principal Quickemu projects. We discussed Quickemu's 2024 reboot in [Episode 30 - Quickemu Rising From the Bashes](https://linuxmatters.sh/30). <!-- and in [Episode 32 - Quick, quicker, quickest](https://linuxmatters.sh/32) [Martin](https://github.com/flexiondotorg) unveils macOS host support for [**Quickemu**](https://github.com/quickemu-project/quickemu), [Mark](https://github.com/marxjohnson) explains the origins of the [**Quickgui**](https://github.com/quickemu-project/quickgui) desktop app and upcoming improvements, and [Alan](https://github.com/popey) debuts [**Quicktest**](https://github.com/quickemu-project/quicktest); a framework for automatically testing operating systems via Quickemu -->

<div align="center">

<a href="https://linuxmatters.sh" target="_blank"><img src="https://github.com/wimpysworld/nix-config/raw/main/.github/screenshots/linuxmatters.png" alt="Linux Matters Podcast"/></a>
<br />
<em>Linux Matters Podcast</em>

</div>

# Quick start

[Once Quickemu is installed](https://github.com/quickemu-project/quickemu/wiki/01-Installation), there are two simple steps to create and run a virtual machine:

- `quickget` automatically downloads the ISO image for the operating system you want to run and creates a configuration file for the virtual machine.

``` shell
quickget nixos unstable minimal
```

- `quickemu` starts the virtual machine using the configuration file created by `quickget`.

``` shell
quickemu --vm nixos-unstable-minimal.conf
```

Execute `quickget` (with no arguments) to see a list of all the supported operating systems.

## Demo

<div align="center">

<a href="https://asciinema.org/a/658148?autoplay=1" target="_blank"><img src="https://asciinema.org/a/658148.svg" /></a>

</div>

# Documentation

The wiki describes how to get up and running with Quickemu and also covers more advanced configuration and usage.

- [**Installation**](https://github.com/quickemu-project/quickemu/wiki/01-Installation) 💾
- [**Create Linux virtual machines**](https://github.com/quickemu-project/quickemu/wiki/02-Create-Linux-virtual-machines) 🐧
- [**Create macOS virtual machines**](https://github.com/quickemu-project/quickemu/wiki/03-Create-macOS-virtual-machines) 🍏
- [**Create Windows virtual machines**](https://github.com/quickemu-project/quickemu/wiki/04-Create-Windows-virtual-machines) 🪟
- [**Advanced quickemu configuration**](https://github.com/quickemu-project/quickemu/wiki/05-Advanced-quickemu-configuration) 🔧
- [**Advanced quickget features**](https://github.com/quickemu-project/quickemu/wiki/06-Advanced-quickget-features) 🤓
- [**Alternative frontends**](https://github.com/quickemu-project/quickemu/wiki/07-Alternative-frontends) 🧑‍💻
- [**References**](https://github.com/quickemu-project/quickemu/wiki/08-References) 📚️
