# Downgrade Linux Kernel

Occasionally a linux kernel update will break the functionality of something on your machine, such as causing the [GPU to hang](https://community.frame.work/t/solved-linux-kernel-5-15-issues/11794/3).

Though such situations are usually temporary, a decent workaround is to downgrade the linux kernel to an older version.

By default, Pacman, the Arch Linux package manager, stores a local cache of downloaded packages in `/var/cache/pacman/pkg`. This is usually sufficient for locating an older, working version of the linux kernel.

List all package files in long listing format, filter for file names containing "linux", and then filter for file names that don't contain "sig":
```zsh
$ ls -al /var/cache/pacman/pkg | grep linux | grep -v sig
```

In case there isn't a working version of the linux kernel in local cache, navigate to the [Arch Linux archives](https://archive.archlinux.org/packages/l/linux/) and download one.

Once you have obtained a suitable target version, use pacman to downgrade the linux package:

```zsh
$ sudo pacman -U linux-5.14.16.arch1-1-x86_64.pkg.tar.zst
# Though it's technically a downgrade in this case, pacman still uses the upgrade flag.
```

Update the GRUB config so that it loads the correct linux kernel during boot:
```zsh
$ sudo grub-mkconfig -o /boot/grub/grub.cfg
```

If you don't mind manually ignoring the linux package every time that you update your packages, you can use:
```bash
$ sudo pacman -Syu --ignore linux
```
or
```bash
$ yay -Syu --ignore linux
```

If you desire a more permanent or elegant solution, then you might want to configure `pacman` to automatically ignore it. Edit /etc/pacman.conf as a superuser:
```zsh
$ sudo nano /etc/pacman.conf
```
And add the following line:
```
IgnorePkg = linux
```

That line can always be removed in the future once a new kernel version is released that fixes the broken functionality.
