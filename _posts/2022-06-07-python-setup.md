---
layout: post
title: Setting Up Python for Web Applications
slug: setting-up-python
date: 2022-06-07T12:00:00.000Z
image: /assets/img/posts/SettingUpPython.png
tags:
  - Python
  - Ansible
  - Virtual Environment
categories:
  - Technology
  - Development
---

# Setting up Python

## Installation

To begin, set up your project directory and install Python along with its essential packages.

```bash
mkdir -p $HOME/projects/webapp
cd $HOME/projects/webapp
sudo apt update
sudo apt install python3 python3-pip python3-virtualenv
```

## Configuration

This section covers the steps to configure a virtual environment for working with Ansible 2.9.7, the version supported in RHEL8. We'll use a simpler approach instead of the latest Ansible execution environments.

### Create and Activate a New Virtual Environment

A virtual environment ensures a consistent workspace.

```bash
virtualenv venv
source venv/bin/activate
```

### Install Ansible and Other Packages

With the virtual environment active, install the specified versions of Ansible and other necessary packages.

```bash
pip3 install ansible==2.9.7
pip3 install pre-commit
pip3 install jinja2
```

> Note: Additional dependent packages will automatically be installed.

### View Installed Packages

List all installed packages - useful for updating Python requirements.txt in Git repositories.

```bash
pip3 freeze
```

Deactivate the Virtual Environment
When done, deactivate the environment.

```bash
deactivate
```

### Further Notes

* While virtual environments are best practice, they're not mandatory.
* Python is primarily a tool here for installing project dependencies.