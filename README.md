# Arch Linux ARM Ansible Scripts

I always wanted to be able to customize and add to Arch Linux ARM embedded installs.

The goal here is to have a set of playbooks that generates an SD card that's
supposed to boot and already be usable for a particular application (ex: the
alarm setup for my [guitar](https://hypertriangle.com/~alex/guitar/), or
[MiSTerArch](https://github.com/MiSTerArch)).

## Dependencies

* [Ansible](https://docs.ansible.com/ansible/latest/)
* qemu-user-static in order to
  [chroot from x86 into an arm system](https://wiki.archlinux.org/title/QEMU#Chrooting_into_arm/arm64_environment_from_x86_64).
* [arch-chroot](https://man.archlinux.org/man/arch-chroot.8) in $PATH from
  package [arch-install-scripts](https://archlinux.org/packages/extra/any/arch-install-scripts/).

### Arch Linux

```bash
pikaur -S ansible qemu-user-static binfmt-qemu-static arch-install-scripts
```

### Debian / glinux

```bash
sudo apt-get install ansible qemu-user-static arch-install-scripts arch-install-scripts
```

## How to run

Define your inventory (at least for the machine settings), see
inventory_template.yml for more details on how or where to put them.

```bash
alarm/ansible% sudo ansible-playbook -i inventory.yml *.play*.yml -v
```

### Tags

After the first successful run, you can probably narrow the `--tags` you're
running, some might be desired to be ran standalone. You can also `--skip-tags`.
`--list-tags` will give you a complete list of how plays are tagged.
Some tags of interest:

* `chroot_aquire` - creates chroot. It won't clobber it if already there,
  but should be skipped in order to save time if it is.
* `chroot_inventory` - if you already have a chroot setup, this is required
  (add as the first task in the arguments) to add the "chroot" host to the ram
  based inventory so other tasks can operate on it.
* `update` - various updates: arch packages

## GPG Checking

These `chroot_aquire` tasks will check validity of the archlinuxarm archive
(that it can only download over http) using GPG. The signature accepted is
stored, checked in this folder, as `archlinuxarm.gpg`.

To verify it, compare `gpg archlinuxarm.gpg` output with
https://archlinuxarm.org/about/package-signing:

	% gpg archlinuxarm.gpg
	gpg: WARNING: no command supplied.  Trying to guess what you mean ...
	pub   rsa4096 2014-01-18 [SC]
	68B3537F39A313B3E574D06777193F152BDBE6A6
	uid           Arch Linux ARM Build System <builder@archlinuxarm.org>
	sub   rsa4096 2014-01-18 [E]
