# ansible-dropbear-initramfs

`dropbear-initramfs` is installed to enable remote unlocking of an encrypted root filesystem via SSH during early boot. This works by integrating the lightweight Dropbear SSH server into the initramfs, allowing SSH access before the system disk is mounted. Once connected, a passphrase can be provided to unlock the LUKS-encrypted disk and continue the boot process.

## testet on


## Content

- [What the playbook does](#what-the-playbook-does)
- [Info](#info)
- [Usage](#usage)
- [Konfiguration](#konfiguration)

## What the playbook does

    - disables dropbear SSH password login
    - installs SSH public key for authentication
    - uses customizable port for SSH
    - optional - takes networkinterface down before OS starts
    - keeps SSH host identity unique by copying and converting openSSH hostkey to a `dropbear-initramfs` compatible format
    - static IPv4 configuration for initramfs

## Info

    - only use reserved IP addresses to avoid address conflicts due to DHCP
    - os far only IPv4 is supported
    - IP address configuration only effects initramfs and not the actual OS
    - SSH dropbear login happens as root `ssh root@<host> -p <your custom ssh port>`

## Usage

    1. Customize `inventories/hosts.yml`
    2. Copy `roles/dropbear_initramfs/defaults/main.yml` to `inventories/host_vars/your-hostname.yml` and provide values for all undefined variables.

```sh
# Run ansible containerized on your client
docker run -ti --rm -v ~/.ssh:/root/.ssh -v ~/.aws:/root/.aws -v $(pwd):/apps -w /apps alpine/ansible ansible-playbook -i inventory/hosts.yml playbooks/dropbear-initramfs-setup.yml --limit=<hostname>