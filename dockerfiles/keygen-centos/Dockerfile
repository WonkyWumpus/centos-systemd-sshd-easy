FROM centos:7
RUN yum -y update && yum clean all

RUN yum -y install openssh-clients

CMD ssh-keygen -q -t rsa -N '' -f /tmp/rsa-key && cat /tmp/rsa-key.pub && cat /tmp/rsa-key
