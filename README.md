# Arch Linux ARM Ansible Scripts

I always wanted to be able to customize and add to Arch Linux ARM embedded installs.

The goal here is to have a set of playbooks that generates an SD card that's supposed to boot and already be usable for a particular application (ex: the alarm setup for my [guitar](https://hypertriangle.com/~alex/guitar/)).

## Dependencies

* [Ansible](https://docs.ansible.com/ansible/latest/)
* [qemu-user-static](https://wiki.archlinux.org/title/QEMU#Chrooting_into_arm/arm64_environment_from_x86_64) in order to chroot from x86 into an arm system.
    * Arch packages:
        * [binfmt-qemu-static](binfmt-qemu-static)
        * [qemu-user-static](https://aur.archlinux.org/packages/qemu-user-static/)
* [arch-chroot](https://man.archlinux.org/man/arch-chroot.8) in $PATH from arch package [arch-install-scripts](https://archlinux.org/packages/extra/any/arch-install-scripts/).
    * Note for other distros: This is a very self contained file, so feel free to just grab/unpack it from the [arch package file](https://archlinux.org/packages/extra/any/arch-install-scripts/download) with a tar invocation.

## How to run

Optional: Add your own .play.yml files for more custom applications.

```bash
alarm-ansible% sudo ansible-playbook *.play.yml -v
```

Then copy results of build/chroot to an sd card.
TODO: partioning and writing to sd cards.
