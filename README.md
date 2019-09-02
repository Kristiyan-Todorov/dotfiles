
# arpi4b

## archlinux distributions

* [armv7l]

## Point of boot

## root password

```bash
passwd root
```

### start sshd

```bash
systemctl start sshd.service
```

### hostname and time

```bash
echo arpi4b > /etc/hostname && ln -sf /usr/share/zoneinfo/Europe/Sofia /etc/localtime
```

### locales

```bash
echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen && echo "LANG=en_US.UTF-8" >> /etc/locale.conf && echo "LC_COLLATE=C" >> /etc/locale.conf && echo "LC_TIME=en_US.UTF-8" >> /etc/locale.conf && echo "LC_MESSAGES=C" >> /etc/locale.conf && locale-gen
```

### packages

```bash
pacman -S sudo ntp zsh git fzf docker powerline powerline-fonts
```

### updates

```bash
pacman -Syu
```

### wheel

```bash
echo "%wheel ALL=(ALL) ALL" >> /etc/sudoers
```

### usr

```bash
useradd -m -d /home/usr -G wheel -s /bin/zsh usr
```

### usr password

```bash
passwd usr
```

### Remove default user

```bash
userdel -r alarm
```

### enable ntpd

```bash
systemctl enable ntpd.service
```

### enable docker

```bash
systemctl enable docker.service
```

### enable sshd

```bash
systemctl enable sshd.service
```

## zsh

```bash
git clone https://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
```

## powerlevel9k

```bash
git clone https://github.com/Powerlevel9k/powerlevel9k.git ~/.oh-my-zsh/themes
```

-----

[armv7l]: <https://archlinuxarm.org/platforms/armv8/broadcom/raspberry-pi-4>
