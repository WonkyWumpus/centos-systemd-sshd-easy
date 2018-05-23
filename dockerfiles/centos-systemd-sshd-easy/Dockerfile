FROM centos:7
RUN yum -y update && yum clean all

ENV container docker
RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;

VOLUME [ "/sys/fs/cgroup" ]

RUN yum -y install openssh-server openssh-clients openssl
RUN yum -y install sudo net-tools vim curl

RUN yum -y clean all

RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

RUN mkdir /root/.ssh

EXPOSE 22
RUN yum -y install openssh openssh-server openssh-clients openssl-libs; systemctl enable sshd.service

ADD easy-network.service /etc/systemd/system/easy-network.service
ADD easy-network-config.sh /root/easy-network-config.sh
RUN chmod 755 /root/easy-network-config.sh; systemctl enable easy-network.service

CMD ["/usr/sbin/init"]
