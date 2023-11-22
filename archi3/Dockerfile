ARG BASE_IMAGE_TAG=base-devel
FROM archlinux:$BASE_IMAGE_TAG

LABEL maintainer="Robert Pannick <rwpannick@gmail.com>"

RUN pacman -Syu --noconfirm xorg-server \
  wget alacritty which \
  i3-wm python-setuptools ttf-dejavu dmenu \
  git base-devel rofi i3status terminator aria2 \
  base-devel cmake

COPY etc/pacman.conf /etc/pacman.conf
COPY etc/makepkg.conf /etc/makepkg.conf

RUN pacman-key --init && \
    curl https://arch.osamc.de/proaudio/osamc.gpg | pacman-key --add - && \
    pacman-key --finger 762AE5DB2B38786364BD81C4B9141BCC62D38EE5 && \
    pacman-key --lsign-key 762AE5DB2B38786364BD81C4B9141BCC62D38EE5 && \
    curl http://syncopated.hopto.org/syncopated.gpg | pacman-key --add - && \
    pacman-key --finger 36A6ECD355DB42B296C0CEE2157CA2FC56ECC96A && \
    pacman-key --lsign-key 36A6ECD355DB42B296C0CEE2157CA2FC56ECC96A && \
    pacman -Syu --noconfirm paru-bin

COPY etc/i3/ /etc/i3/

RUN groupadd -g 1001 syncopated
RUN useradd -ms /bin/bash -u 1001 -g 1001 syncopated
RUN usermod -a -G wheel syncopated
RUN echo '%wheel ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

USER syncopated
WORKDIR /home/syncopated

RUN paru -S --noconfirm --needed --cleanafter --useask \
  jack2-dbus pulseaudio-jack pulseaudio-alsa \
  neovim neovim-symlinks \
  nfs-utils htop nss chromium xz mpg123 exa bat \
  thunar thunar-volman gvfs && \
  paru -Scc --noconfirm && \
  rm -rf /home/syncopated/.cache/paru && \
  sudo rm -rf /var/lib/cache/pacman

CMD i3
