FROM bitnami/minideb:jessie

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd -s /bin/bash -d /home/$USERNAME --uid $USER_UID --gid $USER_GID -m $USERNAME
RUN install_packages wget git sudo openssh-client ca-certificates tar
RUN echo "user ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/user && chmod 0440 /etc/sudoers.d/user
RUN update-ca-certificates
RUN cd /tmp && wget https://dl.google.com/go/go1.13.3.linux-amd64.tar.gz && tar -xvf go1.13.3.linux-amd64.tar.gz && mv go /usr/local
USER vscode
ENV GOROOT=/usr/local/go
ENV GOPATH=/home/$USERNAME/src
ENV PATH=$PATH:$GOROOT/bin:$GOPATH/bin
RUN mkdir -vp $GOPATH
RUN /bin/bash -c 'go get -u github.com/golang/protobuf/protoc-gen-go'
RUN /bin/bash -c 'GO111MODULE=on go get golang.org/x/tools/gopls@latest'
WORKDIR $GOPATH
