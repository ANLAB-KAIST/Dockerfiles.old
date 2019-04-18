# Based on Sharelatex Base Image (sharelatex/sharelatex-base)

# I prefer debian for the base.
FROM debian:latest

ENV TEX_YEAR 2019

# prepare working dir
RUN mkdir /install
WORKDIR /install

# Bootstrap apt
RUN apt update -y
RUN apt dist-upgrade -y
RUN apt install -y apt-transport-https

# I removed unnesseary packages for web servers.
RUN apt install -y build-essential wget time aspell aspell-*

# Download latest tlnet
RUN wget -q http://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz

# Unzip the archive
RUN tar xzf install-tl-unx.tar.gz

# Extract the version name
RUN ls -f | sed -r 's/(install-tl-([0-9]{4})[0-9]+)?(.*)/\2/g' | tr -d '\n' > TEX_YEAR
RUN ls -f | sed -r 's/(install-tl-([0-9]{4})[0-9]+)?(.*)/\1/g' | tr -d '\n' > TEX_DIR

# Since there is no way to setup ENV inside a container, we just check.
RUN test ${TEX_YEAR} == `cat TEX_YEAR`

RUN echo "selected_scheme scheme-full" > texlive.profile
RUN TEX_DIR="`cat TEX_DIR`" && perl ${TEX_DIR}/install-tl -profile texlive.profile

# Clean dirs
WORKDIR /
RUN rm -rf /install
RUN apt autoremove -y
RUN apt autoclean -y

ENV PATH "/usr/local/texlive/${TEX_YEAR}/bin/x86_64-linux/:${PATH}"