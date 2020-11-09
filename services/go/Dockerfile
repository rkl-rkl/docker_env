ARG GO_VERSION
FROM ${GO_VERSION}

ARG GOPROXY
ARG MAINTAINER

MAINTAINER  ${MAINTAINER}

#  修改镜像源代理
RUN go env -w GOPROXY=${GOPROXY}/goproxy/,direct

# 指定项目目录
ENV GOPATH /go/src

# RUN go get -u github.com/derekparker/delve/cmd/dlv

# RUN go get github.com/tools/godep

WORKDIR $GOPATH

# CMD ['bash']