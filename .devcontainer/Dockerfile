FROM bitnami/minideb:jessie

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID
USER vscode
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd -d /home/$USERNAME --uid $USER_UID --gid $USER_GID -m $USERNAME 
RUN install_packages golang-go git sudo openssh-client ca-certificates
RUN update-ca-certificates
ENV GOPATH=/home/$USERNAME/src
RUN mkdir -vp $GOPATH
RUN /bin/bash -c 'go get -u github.com/golang/protobuf/protoc-gen-go'
