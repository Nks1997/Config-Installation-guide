# Terraform Installation Guide (Ubuntu)

This document provides steps to install Terraform on Ubuntu using the official HashiCorp APT repository.  
This method is recommended for servers, CI/CD pipelines, and cloud virtual machines.

---

## Supported Versions
- Ubuntu 20.04
- Ubuntu 22.04
- Ubuntu 24.04

---

## Prerequisites

Update packages and install required dependencies:

```bash
sudo apt update
sudo apt install -y wget gnupg software-properties-common curl
```bash

---

## Install Terraform
Add the HashiCorp GPG key:

wget -O - https://apt.releases.hashicorp.com/gpg | \
sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

---

## Add the HashiCorp APT repository:

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com \
$(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) \
main" | sudo tee /etc/apt/sources.list.d/hashicorp.list

---

## Update APT and install Terraform:

sudo apt update
sudo apt install -y terraform

### Verify Installation
terraform version
Expected output:

Terraform v1.x.x
Optional: Install a Specific Version

### List available versions:
apt list -a terraform

### Install a specific version:
sudo apt install terraform=1.6.6-1

### Uninstall Terraform
sudo apt remove terraform
Remove repository and key (optional):

sudo rm /etc/apt/sources.list.d/hashicorp.list
sudo rm /usr/share/keyrings/hashicorp-archive-keyring.gpg
