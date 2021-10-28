# Docker Image - SteamCDM Image for game servers
# German location, default volume "server", default user "steam"
#
# Forked from https://github.com/steamcmd/docker
#
# https://github.com/thelamescriptkiddiemax/docker_steamcmd
#
#-----------------------------------------------------------------------------------------------------------------------------------------------------------

# Base Image
FROM ubuntu:20.04

#-----------------------------------------------------------------------------------------------------------------------------------------------------------
# Label
LABEL image.authors="Max"
LABEL description="SteamCDM Image for game servers"
LABEL "other"="https://github.com/thelamescriptkiddiemax/docker_steamcmd"
#-----------------------------------------------------------------------------------------------------------------------------------------------------------

# Skip any interactive post-install steps
ARG DEBIAN_FRONTEND=noninteractive

#-----------------------------------------------------------------------------------------------------------------------------------------------------------

# Insert Steam prompt answers
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
RUN echo steam steam/question select "I AGREE" | debconf-set-selections \
 && echo steam steam/license note '' | debconf-set-selections

# Update the repository and install SteamCMD

RUN dpkg --add-architecture i386 \
 && apt-get update -y \
 && apt-get install -y --no-install-recommends ca-certificates locales steamcmd libsdl2-2.0-0:i386 \
 && rm -rf /var/lib/apt/lists/*

# Create symlink for executable
RUN ln -s /usr/games/steamcmd /usr/bin/steamcmd

# Add unicode support
RUN locale-gen de_DE.UTF-8
ENV LANG 'de_DE.UTF-8'
ENV LANGUAGE 'de_DE:de'

#-----------------------------------------------------------------------------------------------------------------------------------------------------------

# Create Steam-User
RUN useradd -m steam
RUN usermod -aG sudo steam
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

# Switch to Steam-User
USER steam
WORKDIR /home/steam

#-----------------------------------------------------------------------------------------------------------------------------------------------------------

# Create volume for server files
RUN mkdir /home/steam/server
VOLUME ["/home/steam/server"]

#-----------------------------------------------------------------------------------------------------------------------------------------------------------

# Update SteamCMD and verify latest version
RUN steamcmd +quit

# Bash-Shell at container start
CMD ["/bin/bash"]
