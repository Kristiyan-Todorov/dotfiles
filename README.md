# arch install

## initial

set root pwd

```sh
passwd
```

wifi connect

```sh
iwctl station ... connect ...
```

---

## ssh

make partitions

```sh
parted -a optimal /dev/nvme0n1 mktable gpt && \
parted -a optimal /dev/nvme0n1 mkpart primary 0% 512MiB && \
parted -a optimal /dev/nvme0n1 name 1 boot && \
parted -a optimal /dev/nvme0n1 set 1 esp on && \
parted -a optimal /dev/nvme0n1 mkpart primary 512MiB 75GiB && \
parted -a optimal /dev/nvme0n1 name 2 root && \
parted -a optimal /dev/nvme0n1 mkpart primary 75GiB 100% && \
parted -a optimal /dev/nvme0n1 name 3 home && \
mkfs.fat -F32 /dev/nvme0n1p1 && \
mkfs.ext4 /dev/nvme0n1p2 && \
mkfs.ext4 /dev/nvme0n1p3 && \
mount /dev/nvme0n1p2 /mnt && \
mount /dev/nvme0n1p1 /mnt/boot/ && mount /dev/nvme0n1p3 /mnt/home/
```

pacstrap

```sh
pacstrap /mnt base base-devel intel-ucode openssh zsh git fzf dhcp sudo vim iw wpa_supplicant dialog i3 i3status i3lock rofi curl vim nano mousepad \
libinput xf86-input-synaptics networkmanager networkmanager-openconnect networkmanager-openvpn networkmanager-pptp networkmanager-vpnc \
lightdm lightdm-gtk-greeter gnome-keyring htop acpi alsa-tools zip p7zip clipnotify lightdm-gtk-greeter-settings linux linux-firmware \
lxappearance ncdu arandr xorg-xrandr dunst ranger \
chromium xorg-server alsa-utils xorg-fonts-100dpi ttf-font-awesome otf-font-awesome adobe-source-code-pro-font freetype2 xorg-fonts-type1 network-manager-applet \
ntp docker powerline powerline-fonts feh dmenu terminator nemo bluez bluez-utils blueman nemo bolt pasystray pulseaudio-bluetooth gsimplecal \
nodejs-lts-gallium npm kubectl libreoffice-still flameshot
```

fstab

```sh
genfstab -U /mnt >> /mnt/etc/fstab
```

chroot

```sh
arch-chroot /mnt
```

change shell

```sh
chsh -s /bin/zsh root
```

lightdm

```sh
systemctl enable lightdm.service
```

wheel

```sh
echo "%wheel ALL=(ALL) ALL" >> /etc/sudoers
```

remove dmesg restriction

```sh
echo "kernel.dmesg_restrict = 0" > /etc/sysctl.d/10-local.conf
```

locales

```sh
echo arch-dell > /etc/hostname && ln -sf /usr/share/zoneinfo/Europe/Sofia /etc/localtime
```

```sh
echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen && echo "LANG=en_US.UTF-8" >> /etc/locale.conf && echo "LC_COLLATE=C" >> /etc/locale.conf && echo "LC_TIME=en_US.UTF-8" >> /etc/locale.conf && echo "LC_MESSAGES=C" >> /etc/locale.conf && locale-gen
```

add user

```sh
useradd -m -d /home/kris -G wheel -s /bin/zsh kris
```

add boot entries

```sh
#/boot/loader/entries/arch.conf

title   Arch Linux
linux   /vmlinuz-linux
initrd  /intel-ucode.img
initrd  /initramfs-linux.img
options root=UUID=046f8f69-cd97-4276-afd4-6c9a6de744fa rw quiet splash nowatchdog
```

```sh
bootctl install && bootctl update
```

```sh
mkinitcpio -p linux
```

mousepad gestures

```sh
# /etc/X11/xorg.conf.d/70-synaptics.conf

Section "InputClass"
    Identifier "touchpad"
    Driver "synaptics"
    MatchIsTouchpad "on"
        Option "TapButton1" "1"
        Option "TapButton2" "3"
        Option "TapButton3" "2"
        Option "VertEdgeScroll" "on"
        Option "VertTwoFingerScroll" "on"
        Option "HorizEdgeScroll" "on"
        Option "HorizTwoFingerScroll" "on"
        Option "CircularScrolling" "on"
        Option "CircScrollTrigger" "2"
        Option "EmulateTwoFingerMinZ" "40"
        Option "EmulateTwoFingerMinW" "8"
        Option "CoastingSpeed" "0"
        Option "FingerLow" "30"
        Option "FingerHigh" "50"
        Option "MaxTapTime" "125"
EndSection
```

enable network manager

```sh
systemctl enable NetworkManager.service
```

enable ntp

```sh
systemctl enable ntpd.service
```

---

## post

install trizen

```sh
git clone https://aur.archlinux.org/trizen.git
cd trizen
makepkg -si
```

install packages

```sh
trizen -S ly bumblebee-status slack-desktop skypeforlinux-stable-bin valentina-studio lens --noedit --noconfirm
```

enable ly

```sh
systemctl enable ly.service
```
