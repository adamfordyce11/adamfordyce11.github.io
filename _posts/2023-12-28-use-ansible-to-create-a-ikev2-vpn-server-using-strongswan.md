---
layout: post
draft: true
title: Use Ansible to create a IKEv2 VPN server using StrongSwan
slug: ansible-create-ikev2-vpn-server-strongswan
date: 2023-12-28T14:02:45.000Z
image: /assets/img/posts/VPNSetupStrongSwan.png
tags:
  - IKEv2
  - VPN
  - Ansible
  - StrongSwan
  - PKI
categories:
  - How To Guide
  - Security
---

# Using Ansible to Create an IKEv2 VPN Server

## Why do I want/need to do this?

Recently I have found the need and desire to have control over my own and family members devices to help stop malware and other malicious activities. Some family members have fell victim to some social engineering techniques and to some spam adverts that have ended up charging their cards.

I would like to make use of the always on feature on the iOS for an IKEv2 VPN connection, this is advised on the UKs [National Cyber Security Center's pages](https://www.ncsc.gov.uk/collection/device-security-guidance/platform-guides/ios-and-ipados) for securing iOS devices.

I have not had to setup an IKEv2 VPN before so after some quick searching I have discovered that on the [Digital Ocean's blog pages](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-ikev2-vpn-server-with-strongswan-on-ubuntu-22-04) there is an entry that details how to set this up on an Ubuntu instance. The blog post goes into great detail and explains how to setup the server manually using a pki package.

I would like to set it up using Ansible automatically from a cloud development environment (CDE) such as GitPod or perhaps even by incorporating GitHub Workspaces.

About the PKI, I would prefer to use OpenSSL directly, while it may be a little more complex to setup I have some familiarity with it and there are some Ansible modules readily available that will allow me to:

- Create the root CA key
- Create the root CA certificate
- Sign the root CA certificate with the key
- Generate client certificates from the root certificate

There is an interesting article on [GitHub member JRald's blog post](https://gquintana.github.io/2020/11/28/Build-your-own-CA-with-Ansible.html) that shows the steps required to do this using Ansible

Once the VPN server is up and running in order to have some control over the traffic the clients are accessing, Pi-Hole will be used as the DNS solution which will effectively block requests to any of the blacklisted destinations that show up.

## Manual Steps

> Note these steps are extracted from the [Digital Ocean blog post](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-ikev2-vpn-server-with-strongswan-on-ubuntu-22-04)

### Prerequisites

- An Ubuntu 22.04 server is required
- The IP address of the server is required
- The ssh private key for accessing the server should be available
-

> Note, please see the following guide on setting up a basic AWS environment which will provide

### Define the Ansible Playbook

Prepare the Ansible playbook, the inventory can be created later on

```yaml
---
- name: Configure VPN server with IKEv2 VPN using StrongSwan
  hosts: vpnservers
  gather_facts: true
  become: true
  tasks:
```

### Step 1 - Installing StrongSwan

```bash
sudo apt update
sudo apt install strongswan strongswan-pki libcharon-extra-plugins libcharon-extauth-plugins libstrongswan-extra-plugins
```

#### Converting the manual steps over to Ansible to update the system and to install the required packages

```yaml
    - name: Update Packages
      ansible.builtin.apt:
        name: "*"
        state: latest

    - name: Install required packages
      ansible.builtin.apt:
        name: "{{ packages }}"
        state: latest
      vars:
        packages:
          - strongswan
          - strongswan-pki
          - libcharon-extra-plugins
          - libcharon-extauth-plugins
          - libstrongswan-extra-plugins
```

> The packages can be added to the inventory or added as top level playbook vars which would make the task definition a little cleaner and would keep the required packages in a separate and more central location.

### Step 2 - Creating a Certificate Authority

```bash
mkdir -p ~/pki/{cacerts,certs,private}
chmod 700 ~/pki
# Generate the root key
pki --gen --type rsa --size 4096 --outform pem > ~/pki/private/ca-key.pem
# Generate the root CA certificate, valid for 10 years (3650 days)
pki --self --ca --lifetime 3650 --in ~/pki/private/ca-key.pem --type rsa --dn "CN=VPN root CA" --outform pem > ~/pki/cacerts/ca-cert.pem

```

Converting this to Ansible would work well inside a block, later on this could be conditionally executed.

#### Ansible output

```yaml
    - name: Create a Certificate Autority
      block:

        - name: Create required directories
          ansible.builtin.file:
            path: "{{ item.path }}"
            state: directory
            mode: "{{ item.mode }}"
          loop:
            - path: ~/pki
              mode: '0700'
            - path: ~/pki/cacerts
              mode: '0644'
            - path: ~/pki/certs
              mode: '0644'
            - path: ~/pki/private
              mode: '0644'

        - name: Generate the root CA key used to sign the root Certificate Authority certificate
          ansible.builtin.command:
            cmd: pki --gen --type rsa --size 4096 --outform pem > ~/pki/private/ca-key.pem
            creates: ~/pki/private/ca-key.pem

        - name: Generate the root Certificate Authority using the root key
          ansible.builtin.command:
            cmd: pki --self --ca --lifetime 3650 --gen --type rsa --size 4096 --outform pem > ~/pki/private/ca-key.pem
            creates: ~/pki/private/ca-key.pem


```

> This is a like for like equivalent of what is in the Digital Ocean blog post, later on this will be updated to use openssl directly.

### Step 3 - Generating a Certificate for the VPN server

```bash
pki --gen --type rsa --size 4096 --outform pem > ~/pki/private/server-key.pem
```

#### Ansible generating a certificate for the VPN server

```yaml
        # Still inside the block ...

        - name: Generate a Certificate for the VPN server
          ansible.builtin.command:
            cmd: pki --gen --type rsa --size 4096 --outform pem > ~/pki/private/server-key.pem
            creates: ~/pki/private/server-key.pem

        - name: Create and sign the VPN server certificate with the certificate authority's key
          ansible.builtin.command:
            cmd: pki --pub --in ~/pki/private/server-key.pem | pki --issue --lifetime 3650 --cacert ~/pki/private/ca-key.pem --cakey ~/pki/private/ca-key.pem --dn "C=UK, O=VPN Server, CN=VPN Server" --san vpn.example.com --flag serverAuth --flag ikeIntermediate --outform pem > ~/pki/certs/server-cert.pem
            creates: ~/pki/certs/server-cert.pem

```

> Note: The creates indicates that the command that is executed should create the file specified at the given path. If this file already exists the command will not be executed upon multiple successive iterations of the playbook. For this reason if any of the PKI steps fail, a rescue section should be added to be block to tidy up the partially created certs and keys in the event of creation failure.

> Note: if using an IP address instead of a DNS name, you will need to specify multiple --san entries.

```bash
--dn "CN=IP address" --san @IP_address --san IP_address
```

#### The pertinent arguments for creating the server certificate

| Argument | Description |
| --- | --- |
| --flag serverAuth | Option is used to indicate the certificate will be used explicitly for server authentication, before the encrypted tunnel is established. |
| --flag ikeIntermediate | This option is used to support older macOS clients |
| --cakey | This is the root certificates CA key |
| --cacert | This is the root certificates CA certificate |
| --outform | This is saying to output the resulting certificate into a [PEM file](https://serverfault.com/questions/9708/what-is-a-pem-file-and-how-does-it-differ-from-other-openssl-generated-key-file) |

#### Ansible rescue section in the event the certificates are not generated correctly

```yaml
      rescue:
        - name: Determine if the cert files exist
          ansible.builtin.stat:
            path: "{{ item }}"
          register: cert_files
          loop:
            - ~/pki/private/ca-key.pem
            - ~/pki/cacerts/ca-cert.pem
            - ~/pki/private/server-key.pem
            - ~/pki/certs/server-cert.pem

        - name: Remove the ~/pki directory
          ansible.builtin.file:
            path: "{{ item.stat.path }}"
            state: absent
          loop: "{{ cert_files.results }}"
          loop_control:
            label: "{{ item.stat.path }}"
          when: item.stat.exists == True
```

The playbook tasks within the block section will attempt to create the required directories and generate the root CA key and certificate. In the event of failure of any of the PKI setup commands, if any of the files already exist, the rescue section will check if the files exist and remove them if they do. This is useful if you need to re-run the playbook after a failure.

The final part to preparing the certificate authority is to copy the SSL/TLS files into the appropriate location for StrongSwan to access.

[What is the difference between SSL and TLS?](https://aws.amazon.com/compare/the-difference-between-ssl-and-tls/#:~:text=However%2C%20SSL%20is%20an%20older,to%20support%20encrypted%20communication%20channels.)

```bash
sudo cp -r ~/pki/* /etc/ipsec.d/
```

#### Ansible steps to copy the generated certificates to the remote server

```yaml
    - name: Copy all of the TLS/SSL files into the /etc/ipsec.d/ directory
      ansible.builtin.copy:
        src: "{{ item.src }}"
        dest: "{{ item.dest }}"
        owner: root
        group: root
        mode: "{{ item.mode }}"
      loop:
        - src: ~/pki/cacerts/ca-cert.pem
          dest: /etc/ipsec.d/cacerts/ca-cert.pem
          mode: '0644'
        - src: ~/pki/certs/server-cert.pem
          dest: /etc/ipsec.d/certs/server-cert.pem
          mode: '0644'
        - src: ~/pki/private/server-key.pem
          dest: /etc/ipsec.d/private/server-key.pem
          mode: '0600'
```

### Step 4 - Configure StrongSwan

[StrongSwan is an OpenSource modular and portable IPsec based VPN solution.](https://strongswan.org/)