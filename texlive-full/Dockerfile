# Based on Sharelatex Base Image (sharelatex/sharelatex-base)

# I prefer debian for the base.
FROM debian:latest

ENV INSTALL_YEAR 2018

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
RUN mkdir install-tl-unx
RUN tar xzf install-tl-unx.tar.gz -C install-tl-unx --strip-components=1

WORKDIR /install/install-tl-unx

# Extract the version
RUN perl install-tl --version | sed -r 's/(.* version ([0-9]+))?(.*)/\2/g' | tr -d '\n' > TEX_YEAR

# Since there is no way to setup ENV inside a container, we just check.
RUN echo "${INSTALL_YEAR}" > TEX_YEAR_CMP
RUN diff -w -q TEX_YEAR_CMP TEX_YEAR

RUN echo "selected_scheme scheme-full" > texlive.profile
RUN perl install-tl -profile texlive.profile

# Clean dirs
WORKDIR /
RUN rm -rf /install
RUN apt autoremove -y
RUN apt autoclean -y

ENV PATH "/usr/local/texlive/${INSTALL_YEAR}/bin/x86_64-linux:${PATH}"