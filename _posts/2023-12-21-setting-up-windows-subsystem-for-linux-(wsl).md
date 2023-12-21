---
layout: post
title: Setting Up Windows Subsystem for Linux (WSL)
slug: setting-up-wsl
date: 2022-06-01T14:02:45.000Z
tags:
  - WSL
  - Linux
  - DevOps
categories:
  - Technology
  - Windows
---

# Setting Up Windows Subsystem for Linux (WSL)

## TL;DR

```powershell
# Open a Windows command prompt as Administrator
wsl --install
wsl --list --online
wsl --set-default-version 1
wsl --install -d Ubuntu-20.04
# Wait for Ubuntu-20.04 to be downloaded...
# Reboot PC

# Ensure the WSL kernel is uptodate
wsl --update
wsl --shutdown

# Open Ubuntu from the Start Menu, this will Install the downloaded Ubuntu into WSL and Register it with the OS before asking you to set a password.
```

## Introduction

When it comes to DevOps tooling, many tools work seamlessly in a Linux-style environment. If your laptop runs on Windows, you can gain access to a Linux environment using the Windows Subsystem for Linux (WSL).

Once WSL is activated, you can install a Linux distribution on top of it. The choice of Linux distribution often comes down to personal preference, but Ubuntu is popular, and there is a wealth of community content available on using Ubuntu with WSL on the internet.

> Should I use WSL or WSL2? [What are the differences between WSL1 and WSL2?](https://docs.microsoft.com/en-us/windows/wsl/compare-versions) - Both environments will work - WSL1 is set by default and offers the functionality needed to work with Ansible.

## Installation

To set up WSL on your Windows machine, follow these steps:

1. Open a Windows command prompt as an Administrator.
2. Install WSL.
3. Download your preferred Linux distribution.
4. Restart your computer.
5. Install your chosen Linux distribution (e.g., Ubuntu 20.04).

For detailed instructions, refer to:

* [Install Linux on Windows with WSL](https://docs.microsoft.com/en-us/windows/wsl/install)
* [WSL2 Installation Steps](https://pureinfotech.com/install-windows-subsystem-linux-2-windows-10/)

## Configuration

Once WSL is enabled, installed, and activated, you're ready to start using it on your PC.

For a comprehensive guide on configuring your WSL development environment, refer to [Best practices for setting up a WSL development environment](https://docs.microsoft.com/en-us/windows/wsl/setup/environment). This guide covers the following steps:

* Setting up WSL
* Configuring Visual Studio Code (VSCode)
* Integrating WSL with VSCode

Additionally, consider exploring the Windows Terminal, a powerful tool that offers exciting features like working with your linked Azure account and providing a similar experience to the cloud shell. It also allows multiple concurrent terminal windows, which can be very useful.

Happy Linux development on Windows with WSL!