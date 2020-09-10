FROM rocker/r-apt:bionic
LABEL MAINTAINER="Pedro Rodrigues <pir.pedro@gmail.com>"

# Options for setup script
ARG INSTALL_ZSH="true"
ARG UPGRADE_PACKAGES="true"
ARG USERNAME=app
ARG USER_UID=1000
ARG USER_GID=$USER_UID
ARG BOOTSTRAP_SCRIPT=common-debian.sh

# Install needed packages and setup non-root user. Use a separate RUN statement to add your own dependencies.
COPY ${BOOTSTRAP_SCRIPT} /tmp/library-scripts/
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && /bin/bash /tmp/library-scripts/${BOOTSTRAP_SCRIPT} "${INSTALL_ZSH}" "${USERNAME}" "${USER_UID}" "${USER_GID}" "${UPGRADE_PACKAGES}" \
    && apt-get -y install libzip-dev python3 python3-pip libapt-pkg-dev \
    && pip3 install -U wheel radian \
    && apt-get autoremove -y && apt-get clean -y && rm -rf /var/lib/apt/lists/* /tmp/library-scripts

# [Optional] Uncomment this section to install additional OS packages.
# RUN apt-get update \
#     && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends <your-package-list-here>