---
layout: post
title: Setting Up Visual Studio Code (VSCode)
slug: setting-up-vscode
date: 2022-06-03T10:30:00.000Z
tags:
  - Visual Studio Code
  - Development Tools
categories:
  - Technology
  - Development
---

# Setting Up Visual Studio Code (VSCode)

## Prerequisite

Before setting up VSCode, make sure you have already set up WSL and installed the Ubuntu 20.04 version.

## Introduction

A good code editor can significantly enhance your development experience. Visual Studio Code (VSCode) offers a wide range of features that can benefit developers working with various programming languages. In this guide, we'll focus on specific features that are particularly useful for Ansible practitioners.

Here are some of the key features of VSCode that cater to Ansible use cases:

- Excellent Git integration
- GitHub integration
- YAML support
- Python support
- Integration with WSL/WSL2
- Built-in terminal

Additionally, GitHub has a special integration with VSCode that allows you to open any project in a web-based version of the editor by replacing the prefix "www" in the GitHub address with "dev." This feature opens up new possibilities for development on devices like iPads and Android tablets, which were previously challenging to work with.

## Installation

If you haven't already installed VSCode, you can download the latest version from the official website:

[Download Visual Studio Code](https://code.visualstudio.com)

Once downloaded, run the installer and follow the installation dialog, accepting the default settings.

## Configuration

After installing VSCode, it's essential to set up a few key extensions to enhance your development experience. Install the following extensions:

- Remote WSL: Allows you to work seamlessly with your Windows Subsystem for Linux (WSL) environment.
- YAML: Provides syntax highlighting and validation for YAML files commonly used in Ansible.
- Prettier: A code formatter that helps maintain consistent code style.
- Ansible: Offers enhanced support for Ansible, including code snippets and debugging.
- Jinja2: Provides support for Jinja2 templating, commonly used in Ansible playbooks.

To install these extensions, open VSCode, go to the Extensions sidebar, and search for each extension by name. Click the "Install" button for each one.

> For detailed instructions on setting up WSL to work with Visual Studio Code, refer to the following guide:
[Get started using Visual Studio Code with WSL](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-vscode)

