#
# Copyright (c) 2020 Red Hat, Inc.
# This program and the accompanying materials are made
# available under the terms of the Eclipse Public License 2.0
# which is available at https://www.eclipse.org/legal/epl-2.0/
#
# SPDX-License-Identifier: EPL-2.0
#

# https://catalog.redhat.com/software/containers/ubi8/nodejs-12/5d3fff015a13461f5fb8635a?tag=1-70
FROM registry.access.redhat.com/ubi8/nodejs-12:1-70

USER 0
# Set permissions on /etc/passwd and /home to allow arbitrary users to write
COPY --chown=0:0 entrypoint.sh /
RUN mkdir -p /projects && \
    mkdir -p /home/user && \
    chgrp -R 0 /home && \
    chmod -R g=u /etc/passwd /etc/group /home && \
    chmod +x /entrypoint.sh
COPY bashrc /home/user/.bashrc

WORKDIR /work

RUN dnf -y install git python36 python3-six python3-pip platform-python-pip \
    curl wget bzip2 gdb make cmake gcc gcc-c++
RUN /usr/bin/python3 -m pip install --upgrade pip
RUN pip install ipykernel jupyter;

RUN git clone https://github.com/sunix/vscode-python && \
    cd vscode-python && \
    rm -rf ./.git && \
    npm install -g vsce gulp-cli && npm install --unsafe-perm=true --allow-root && \
    npm run package


USER 10001
ENV HOME=/home/user
WORKDIR /projects
ENTRYPOINT [ "/entrypoint.sh" ]
CMD ["tail", "-f", "/dev/null"]

