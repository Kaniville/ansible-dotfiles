# 🌸 Ansible Dotfiles

ArchLinux configuration managed with Ansible.

![](src/screenshot.png)

## 🛁 Why

- Provides a quick and easy method of installing an operating system.
- Allow us to easily modify installed packages and applied settings.
- Focus on package installation via Flatpak and Distrobox to keep the system clean and simple.
- Thanks to the simplicity of the YAML language, it is very easy to understand & create additional tasks.

## 🚀 Installation

> ⛔ **Important variables are present in the `ansible/group_vars` directory. You need to edit them to customize your installation.**

First, follow the [ArchLinux installation guide](https://wiki.archlinux.org/title/Installation_guide) and chroot into your system.

Then, let's clone the repository into a directory (for example, `/mnt`):
```
# git clone https://github.com/steadywool/ansible-dotfiles.git /mnt/ansible-dotfiles
# cd /mnt/ansible-dotfiles
```

The installation will be done in 3 steps, for each step we will use a different tag.
Let's use first the **LIVE** tag to install the necessary configuration to start the system:
```
# ansible-playbook playbook.yml -t LIVE
```

Quit the chroot and start your new system.
Start the `NetworkManager.service` service and configure your connection with `nmtui`:
```
# systemctl start NetworkManager.service
# nmtui
```

> ⚠️  **Don't forget to modify the variables in `group_vars`, especially the user password !**

We will now use the **BOOT** tag:
```
# ansible-playbook playbook.yml -t BOOT
```

Then, enable/start `gdm.service`:
```
# systemctl enable gdm.service
# systemctl start gdm.service
```

You can now log in with your new user account.
To install your user configuration, edit the `05-homedir` role and use the **HOME** tag:
```
$ ansible-playbook playbook.yml -t HOME
```

> ⚠️  **If you execute this role as root, your configuration will be installed to the root user !**

## 🔧 Configuration

You can perform partially run of playbook using tags.

Available tags are:
- LIVE
- BOOT
- HOME
- 00-core
- 01-system
- 02-desktop
- 03-applications
- 04-access
- 05-homedir
- bootloader
- kernel
- hostname
- timezone
- locale
- packages
- sysctl
- udev
- services
- firewalld
- snapper
- sudo
- usbguard
- users
- subuid
- subgid
- flatpak
- dotfiles

> ⚠️  **This playbook does not update the system.**

## 📕 Exemples

Run the entire playbook:
```
$ ansible-playbook playbook.yml -K
```

> 📌 **The `-K` option is used to request the "sudo" password without being recognized by Ansible as root. We need it for tasks requiring privileges.**

Install every packages & enable/start Systemd services:
```
$ ansible-playbook playbook.yml -K -t packages,services
```

Executes tasks requiring no privileges:
```
$ ansible-playbook playbook.yml -t 05-homedir
```

> 📌 **The `HOME`, `dconf`, `flatpak` & `dotfiles` tags don't require privileges either.**
