# ansible-dropbear-initramfs

`dropbear-initramfs` is installed to enable remote unlocking of an encrypted root filesystem via SSH during early boot. This works by integrating the lightweight Dropbear SSH server into the initramfs, allowing SSH access before the system disk is mounted. Once connected, a passphrase can be provided to unlock the LUKS-encrypted disk and continue the boot process.

## testet on

- Ubuntu Server

## Inhaltsverzeichnis

- [What the playbook does](#what-the-playbook-does)
- [Usage](#usage)
- [Konfiguration](#konfiguration)

## What the playbook does

    - disables dropbear ssh password login
    - optional - takes networkinterface down before OS starts
    - uses openSSH hostkey to generate dropbear hostkey --> keeps ssh hostidentity
    - static IPv4 configuration for initramfs

## Usage

Voraussetzungen:
- Node.js >= 18 / Python >= 3.10 / etc.
- Abhängigkeiten, die vorab installiert werden müssen

```sh
# Run ansible containerized on your client
docker run -ti --rm -v ~/.ssh:/root/.ssh -v ~/.aws:/root/.aws -v $(pwd):/apps -w /apps alpine/ansible ansible-playbook -i inventory/hosts.yml playbooks/dropbear-initramfs-setup.yml --limit=<hostname>