# Documentation

## Development

### Setup phase

Started with installing KVM and virt-manager on Arch Linux for virtualization.

#### Lists

- Packages installed: `qemu`, `virt-manager`, `virt-viewer`, `libvirt`, `edk2-ovmf`, `bridge-utils`, `dnsmasq`
- Enabled and started `libvirtd` service
- Added user to `libvirt` group
- Verified virtualization support

#### Commands used

* Update system and install virtualization packages
  ```bash
  sudo pacman -Syu qemu virt-manager virt-viewer libvirt edk2-ovmf bridge-utils dnsmasq

