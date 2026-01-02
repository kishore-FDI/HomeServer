# Documentation

## 1. Purpose

Establish a secure, private home server environment for file and password storage.
Initial implementation uses a virtualized server hosted on a personal laptop to validate architecture and hardening before migrating to dedicated hardware.

---

## 2. Host Environment

### 2.1 Operating System

* **Host OS:** Arch Linux
* **Role:** Virtualization host only
* **Security posture:** Fully updated, no server workloads on host

---

## 3. Virtualization Stack (Host)

### 3.1 Installed Packages

| Package        | Purpose                                   |
| -------------- | ----------------------------------------- |
| `qemu`         | Hardware-assisted virtualization          |
| `virt-manager` | VM lifecycle management                   |
| `virt-viewer`  | Console access (used only during install) |
| `libvirt`      | Hypervisor abstraction and daemon         |
| `edk2-ovmf`    | UEFI firmware for VMs                     |
| `bridge-utils` | Network bridging                          |
| `dnsmasq`      | DHCP/DNS for virtual networks             |

---

### 3.2 Host Setup Steps

1. **System update**

   ```bash
   sudo pacman -Syu
   ```

2. **Install virtualization stack**

   ```bash
   sudo pacman -S qemu virt-manager virt-viewer libvirt edk2-ovmf bridge-utils dnsmasq
   ```

3. **Enable libvirt daemon**

   ```bash
   sudo systemctl enable --now libvirtd
   ```

4. **Grant non-root VM management access**

   ```bash
   sudo usermod -aG libvirt kishore
   ```

5. **Verify hardware virtualization**

   ```bash
   lsmod | grep kvm
   ```

---

## 4. Virtual Machine (Server)

### 4.1 VM Purpose

* Acts as the future home server blueprint
* Isolated from host except via SSH
* No desktop, no GUI, no user applications

---

### 4.2 Guest OS

* **Distribution:** Ubuntu Server (LTS)
* **Install profile:** Minimal
* **Firmware:** UEFI (OVMF)
* **Access:** SSH only

---

### 4.3 VM Configuration

| Component  | Configuration                        |
| ---------- | ------------------------------------ |
| CPUs       | Fixed allocation                     |
| Memory     | Fixed allocation                     |
| Disk 1     | OS disk (encrypted)                  |
| Disk 2     | Data disk (encrypted, mounted later) |
| Networking | Host-only or bridged                 |
| Console    | Disabled after install               |

---

## 5. Guest OS Hardening

### 5.1 Account Security

* Single administrative user
* Root account locked

  ```bash
  sudo passwd -l root
  ```

---

### 5.2 SSH Hardening

**Authentication**

* SSH key only
* Password authentication disabled
* Root login disabled

**Other Controls**

* Non-default SSH port
* Limited authentication retries
* No X11 forwarding
* No port forwarding

---

### 5.3 Firewall

* Default deny inbound
* Default allow outbound
* SSH port explicitly allowed

---

### 5.4 Intrusion Protection

* Fail2ban enabled
* SSH jail active
* Aggressive retry limits

---

### 5.5 Kernel and Network Hardening

* ASLR enabled
* ICMP redirects disabled
* Reverse path filtering enabled
* TCP SYN cookies enabled

---

### 5.6 Service Minimization

* Audited enabled services
* Disabled all unused daemons
* No exposed network services beyond SSH

---

### 5.7 Logging and Auditing

* `auditd` installed and enabled
* Persistent system logs retained

---

### 5.8 Filesystem Protections

* `noexec`, `nosuid`, `nodev` applied where applicable
* Secure default umask enforced

---

