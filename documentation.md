# Documentation

## Development

### Setup phase

Started with installing KVM and virt-manager on Arch Linux for virtualization and creating a hardened Ubuntu Server VM accessed only via SSH keys.

#### Packages installed (host)

- `qemu`
- `virt-manager`
- `virt-viewer`
- `libvirt`
- `edk2-ovmf`
- `bridge-utils`
- `dnsmasq`

#### Host configuration steps

- Updated system
- Installed virtualization stack
- Enabled and started libvirt daemon
- Added user to `libvirt` group
- Verified hardware virtualization support

#### Commands used (host)

```bash
sudo pacman -Syu
sudo pacman -S qemu virt-manager virt-viewer libvirt edk2-ovmf bridge-utils dnsmasq

sudo systemctl enable --now libvirtd
sudo usermod -aG libvirt kishore

lsmod | grep kvm
