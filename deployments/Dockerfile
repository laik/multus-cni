# This Dockerfile is used to build the image available on DockerHub
# Build the manager binary
FROM golang:1.14 as builder

WORKDIR /usr/src/multus-cni
# Copy the Go Modules manifests
ENV GO111MODULE=on
ENV GOPROXY=https://goproxy.cn,https://goproxy.io,https://mirrors.aliyun.com/goproxy/,https://athens.azurefd.net,direct

COPY go.mod go.mod
COPY go.sum go.sum
# cache deps before building and copying source so that we don't need to re-download as much
# and so that source changes don't invalidate our downloaded layer
RUN go mod download

# Add everything
ADD . .

# Build
RUN ./hack/build-go.sh
    
FROM centos:centos7
COPY --from=builder /usr/src/multus-cni/bin/multus /usr/src/multus-cni/bin/multus
WORKDIR /

ADD ./images/entrypoint.sh /
ENTRYPOINT ["/entrypoint.sh"]
