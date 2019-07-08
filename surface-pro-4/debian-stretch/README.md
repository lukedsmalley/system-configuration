# Configuration of Debian Stretch on the Surface Pro 4
Last Updated 7/6/2019.

## Operating System Installation

USB image used: [Debian 9.8 CD Image with Non-free Firmware](https://cdimage.debian.org/cdimage/unofficial/non-free/cd-including-firmware/archive/9.8.0+nonfree/amd64/iso-cd/firmware-9.8.0-amd64-netinst.iso)

The touchpad does not work during installation, so a USB hub with the installation drive and a USB mouse must be used.

GRUB installs perfectly fine under Stretch [unlike Jessie](https://github.com/jimdigriz/debian-mssp4#installing-debian), but the EFI partition can somehow be out of space and cause it to fail. This can be prevented by executing a shell in the installer before installation and cleaning up `efivars`:

```sh
mkdir /tmp/efivars
mount -t efivarfs none /tmp/efivars
rm /tmp/efivars/dump-type0-*
umount /tmp/efivars

rm /sys/fs/pstore/dmesg-efi-*
```

The WLAN driver used is the `usb8xxx` driver from `firmware-libertas`, since the wireless card is a [Marvell 88W8897](https://github.com/jimdigriz/debian-mssp4#what-is-working).

Desktop chosen in `tasksel` is Cinnamon, because it's the only one that scales automatically to the Surface's Resolution/DPI.

## Software Installation

### Lock package version for `firmware-libertas`
```sh
sudo apt-mark hold firmware-libertas
```

### Add repository keys
```sh
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
sudo apt install dirmngr
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 931FF8E79F0876134EDDBDCCA87FF9DF48BF1C90
```

### Copy archived configuration
```sh
sudo cp -rf ./root/*/ /
```

### Install applications from repositories
```sh
sudo apt install apt-transport-https
sudo apt install curl fonts-firacode git gparted htop lightdm-gtk-greeter-settings spotify-client
sudo apt install --no-install-recommends yarn
```

### Install Firefox
Download the Firefox tarball from https://www.mozilla.org/en-US/firefox/ and extract it.
```sh
sudo tar -C /opt -xf ~/Downloads/firefox*.tar.bz2
```
Add a launcher shortcut to `/opt/firefox/firefox` with the icon `/opt/firefox/browser/chrome/icons/default/default128.png`.

### Install Node.js
Manually download the runtime tarball from https://nodejs.org, then extract it.
```sh
sudo tar -xf ./node*.tar.gz
sudo cp -rf ./node*/*/ /usr/local
```
The following applications have to be manually downloaded:
* [VSCode](https://code.visualstudio.com/)

### Install applications from manually-downloaded Debian packages
```sh
sudo apt install ./<app>.deb
```
The following applications have to be manually downloaded:
* [VSCode](https://code.visualstudio.com/)
* [Discord](https://discordapp.com)

### Todo
/etc/lightdm.conf for autologin
backlight
Never sleep on cover close
Dark theme
Git credentials
Open windows maximized
Open shell in desktop
