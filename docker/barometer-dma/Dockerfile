FROM centos:7

# install prerequisites
RUN yum update -y &&\
    yum install -y which sudo git libvirt-devel && \
    rpm --import https://mirror.go-repo.io/centos/RPM-GPG-KEY-GO-REPO && \
    curl -s https://mirror.go-repo.io/centos/go-repo.repo | tee /etc/yum.repos.d/go-repo.repo && \
    yum install -y golang

# set environment variables
ENV DOCKER y
ENV repos_dir /root/go/src/github.com/opnfv

# get repo code and build
WORKDIR ${repos_dir}
RUN git clone https://gerrit.opnfv.org/gerrit/barometer
WORKDIR ${repos_dir}/barometer/src/dma
RUN go build ./cmd/server && \
    go build ./cmd/threshold && \
    go build ./cmd/infofetch

RUN cp server threshold infofetch /
WORKDIR /
