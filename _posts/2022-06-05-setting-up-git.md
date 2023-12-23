---
layout: post
title: Setting Up Git
slug: setting-up-git
date: 2022-06-05T12:00:00.000Z
image: /assets/img/posts/SettingUpGit.png
tags:
  - Git
  - Version Control
categories:
  - Technology
  - Development
---

# Setting Up Git

## TL;DR

```bash
# Install Git
sudo apt-get install git

# Configure Git
git config --global user.name "Your Name"
git config --global user.email "youremail@domain.com"

# Generate SSH keys
ssh-keygen -t ed25519 -C "youremail@example.com"
> Accept the default location of the key
> Enter a password for the SSH key (optional)

# Display the public key
cat ~/.ssh/id_ed25519.pub
# Copy this output

# Add the public key to GitHub
# Go to GitHub: Settings > SSH and GPG Keys > New SSH Key

# Create a test repository in GitHub

# Push a change to GitHub
echo "# test" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:yourusername/test.git
git push -u origin main
```

## Prerequisite

Before setting up Git, ensure that you have prepared the WSL Ubuntu distribution on your PC.

## Install Git

To install Git on your WSL Ubuntu distribution, run the following command:

```bash
sudo apt install git
```

## Configuration

### SSH Key Setup

1. Generate a new set of SSH keys using the following command:

```bash
ssh-keygen -t ed25519 -C "youremail@example.com"
```

* Accept the default location of the key.
* You may choose to enter a password for the SSH key (optional).

2. Display the public key using the following command:

```bash
cat ~/.ssh/id_ed25519.pub
```

* Copy the output.

3. Follow the instructions in [GitHub's documentation](https://docs.github.com/en/authentication/connecting-to-github-with-ssh) to add the generated public key to your GitHub account.

### Creating a Test Repository on GitHub

1. Visit GitHub and log in to your account.
2. Navigate to your account's settings.
3. In the left sidebar, go to "SSH and GPG Keys."
4. Add a new SSH key by clicking "New SSH Key."

### Pushing a Change to GitHub

To test your Git setup, create a test repository on GitHub:

1. In your GitHub account, click the "New" button to create a new repository.
2. Follow the instructions provided after creating the repository to push a change into it:

```bash
echo "# test" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:yourusername/test.git
git push -u origin main
```
