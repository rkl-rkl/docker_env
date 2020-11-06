FROM golang:1.14

MAINTAINER  renkeliang (renkeliang@foxmail.com)

ENV GOPATH /go/src

#  修改镜像源
# RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.tuna.tsinghua.edu.cn/g' /etc/apk/repositories

## vim
# RUN apt-get update \
#   && apt-get install -y vim \
#   && rm -rf /var/lib/apt/lists/*

# vim setting
# COPY vimrc $GOPATH

RUN go get -u github.com/derekparker/delve/cmd/dlv

WORKDIR $GOPATH

CMD ['bash']