FROM bitnami/minideb:jessie AS download
RUN install_packages wget ca-certificates openssh-client
RUN update-ca-certificates
RUN cd /tmp && wget -q https://bazel.build/bazel-release.pub.gpg 
RUN cd /tmp && wget -q https://dl.google.com/go/go1.13.3.linux-amd64.tar.gz && tar -xf go1.13.3.linux-amd64.tar.gz
RUN ls /tmp


FROM bitnami/minideb:jessie

# Install inital packages
RUN install_packages git sudo openssh-client ca-certificates apt-transport-https

# Install bazel
COPY --from=download /tmp/bazel-release.pub.gpg /tmp
RUN cat /tmp/bazel-release.pub.gpg | apt-key add -
RUN echo "deb [arch=amd64] https://storage.googleapis.com/bazel-apt stable jdk1.8" | tee /etc/apt/sources.list.d/bazel.list
RUN install_packages bazel

# Create vscode user
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd -s /bin/bash -d /home/$USERNAME --uid $USER_UID --gid $USER_GID -m $USERNAME
RUN echo "user ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/user && chmod 0440 /etc/sudoers.d/user

# Install certificates for Go to be happy
RUN update-ca-certificates

# Install Go
COPY --from=download /tmp/go /usr/local/go

# vscode does much better when the go libraries are installed by the vscode user
USER vscode
ENV GOROOT=/usr/local/go
ENV GOPATH=/home/$USERNAME/src
ENV PATH=$PATH:$GOROOT/bin:$GOPATH/bin
RUN mkdir -vp $GOPATH

RUN /bin/bash -c 'go get -u github.com/golang/protobuf/protoc-gen-go > /dev/null'
RUN /bin/bash -c 'GO111MODULE=on go get golang.org/x/tools/gopls@latest > /dev/null'
RUN /bin/bash -c 'GO111MODULE=on go get github.com/bazelbuild/buildtools/buildifier@latest > /dev/null'
WORKDIR ${GOPATH}
