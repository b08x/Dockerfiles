ARG BASE_IMAGE_TAG=base-devel-20240101.0.204074
FROM archlinux:$BASE_IMAGE_TAG

LABEL maintainer="Robert Pannick <rwpannick@gmail.com>"

RUN groupadd -g 1001 soundbot
RUN useradd -ms /bin/bash -u 1000 -g 1001 soundbot
RUN usermod -a -G wheel soundbot
RUN echo '%wheel ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

RUN pacman -Syu --noconfirm xorg-server \
		wget kitty which \
		i3-wm python-setuptools ttf-dejavu dmenu \
		git base-devel rofi i3status terminator \
		cmake x11vnc xorg-server-xvfb tk net-tools xf86-video-dummy

RUN pacman -Sy --noconfirm && \
		pacman-key --init && \
		pacman-key --recv-key 3056513887B78AEB --keyserver keyserver.ubuntu.com && \
		pacman-key --lsign-key 3056513887B78AEB && \
		pacman -U --noconfirm 'https://cdn-mirror.chaotic.cx/chaotic-aur/chaotic-keyring.pkg.tar.zst' && \
		pacman -U --noconfirm 'https://cdn-mirror.chaotic.cx/chaotic-aur/chaotic-mirrorlist.pkg.tar.zst'

COPY i3arch/etc/pacman.conf /etc/pacman.conf

RUN pacman -Sy --noconfirm && pacman -S --noconfirm paru

# RUN curl https://arch.osamc.de/proaudio/osamc.gpg | pacman-key --add - && \
#     pacman-key --finger 762AE5DB2B38786364BD81C4B9141BCC62D38EE5 && \
#     pacman-key --lsign-key 762AE5DB2B38786364BD81C4B9141BCC62D38EE5 && \
#     curl http://bender.syncopated.net/repo/syncopated.gpg | pacman-key --add - && \
#     pacman-key --finger 36A6ECD355DB42B296C0CEE2157CA2FC56ECC96A && \
#     pacman-key --lsign-key 36A6ECD355DB42B296C0CEE2157CA2FC56ECC96A

COPY i3arch/etc/i3/ /etc/i3/

USER soundbot
WORKDIR /home/soundbot

RUN paru -S --noconfirm --needed --cleanafter --batchinstall --useask \
		jack2-dbus pulseaudio-jack pulseaudio-alsa \
  	neovim neovim-symlinks \
		nfs-utils htop nss chromium xz mpg123 exa bat \
  	thunar thunar-volman gvfs && \
  	paru -Scc --noconfirm && \
  	rm -rf /home/syncopated/.cache/paru && \
  	sudo rm -rf /var/lib/cache/pacman

CMD i3
