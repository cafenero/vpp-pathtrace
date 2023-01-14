FROM ubuntu:20.04

# Default settings
ENV DEBIAN_FRONTEND noninteractive
ENV TZ=Asia/Tokyo

# Essential packages
RUN apt-get update && apt-get install -y sudo python3 python3-pip git

# use systemd
RUN apt-get -y install init

# Add sde and related files
ADD bash_history /root/.bash_history
ADD termshark /root/.config/termshark/

# Install additional packages
RUN apt-get clean && rm -rf /var/lib/apt/lists/* && pip3 install scapy
RUN apt-get update && apt-get install -y jq zsh vim tree curl

# golang for termshark
RUN apt-get -y install software-properties-common
RUN add-apt-repository -y ppa:longsleep/golang-backports
RUN apt-get update && apt-get -y install golang-go tshark

# termshark
RUN git clone https://github.com/gcla/termshark && cd termshark && go install ./...
ENV PATH $PATH:/root/go/bin

# install network related packages
RUN apt-get update
RUN apt-get -y install iputils-ping net-tools dnsutils iproute2 pciutils

# # install VPP
RUN curl -s https://packagecloud.io/install/repositories/fdio/2210/script.deb.sh | bash
RUN apt-get update
RUN apt-get -y install vpp vpp-plugin-core vpp-plugin-dpdk python3-vpp-api vpp-dbg vpp-dev
ADD setup.conf /root/setup.conf
ADD startup.conf /etc/vpp/startup.conf


RUN apt-get -y install tcpdump

WORKDIR /root/

ENTRYPOINT [ "/sbin/init" ]
