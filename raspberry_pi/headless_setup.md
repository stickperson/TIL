# Headless Raspberry Pi Setup

I have a lot of raspberry pi's sitting around. Sometimes I flash new images and want to get install the same packages. I don't want to learn ansible (no idea if that would slow down the pi... probably), so here's a bunch of steps to get things up and running.

## Before initial boot
- download the image
- use something like Etcher or `dd` to flash the image
- `cd /Volumes/boot` and run `touch ssh`
- in the same directory, create a `wpa_supplicant.conf` file

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
country=US

network={
     ssid="<ssid>"
     psk="<pd>"
     key_mgmt=WPA-PSK
}
```

## After booting
- `sudo apt-get update && sudo apt-get upgrade && sudo apt-get dist-upgrade`
- `sudo apt-get install -y tmux vim git`
- add `gpu_mem=16` to `/boot/config.txt` since this will be headless
- follow [these instructions](https://github.com/robbyrussell/oh-my-zsh/wiki/Installing-ZSH) to install zsh/oh-my-zsh
- clone dotiles/copy over
- install any necessary zsh-plugins/themes etc.
- follow [these instructions](https://weechat.org/download/debian/) to setup weechat

## Weechat setup
TODO: write a small script to replace some values in the configuration files (usernames etc) with env variables.

At some point I'll have config files in the `dotfiles` repo. Until then just `scp` all configs over except `sec.conf`.

- `/unset weechat.network.gnutls_ca_file`
- `/secure passphrase <passphrase>` to store things in `sec.conf`
- `/secure set freenode_password xxxxxxx`