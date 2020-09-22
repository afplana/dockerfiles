FROM ubuntu:focal

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update -y &&  apt-get install -y --no-install-recommends \
    acl \
    apt-utils \
    apache2 \
    asciidoc \
    bzip2 \
    cdbs \
    curl \
    debhelper \
    debianutils \
    devscripts \
    docbook-xml \
    dpkg-dev \
    fakeroot \
    gawk \
    gcc \
    git \
    locales \
    make \
    mercurial \
    mysql-server \
    openssh-client \
    openssh-server \
    pass \
    reprepro \
    rsync \
    ruby \
    sshpass \
    subversion \
    python3-pip \
    sudo \
    tzdata \
    unzip \
    xsltproc \
    zip \
    && \
    apt-get clean

#RUN apt-get update && apt-add-repository universe && apt update && apt install python2
#RUN curl https://bootstrap.pypa.io/get-pip.py --output get-pip.py && python2 get-pip.py
#RUN pip install pip --upgrade
#RUN pip install --upgrade pycrypto cryptography

#ADD init-fake.conf /etc/init/fake-container-events.conf

#RUN apt-get update && apt-get install software-properties-common
RUN apt-add-repository ppa:ansible/ansible && sudo apt-get update
RUN apt-get -y install ansible

# remove some pointless services
RUN /usr/sbin/update-rc.d -f ondemand remove; \
	for f in \
		/etc/init/u*.conf \
		/etc/init/mounted-dev.conf \
		/etc/init/mounted-proc.conf \
		/etc/init/mounted-run.conf \
		/etc/init/mounted-tmp.conf \
		/etc/init/mounted-var.conf \
		/etc/init/hostname.conf \
		/etc/init/networking.conf \
		/etc/init/tty*.conf \
		/etc/init/plymouth*.conf \
		/etc/init/hwclock*.conf \
		/etc/init/module*.conf\
	; do \
		dpkg-divert --local --rename --add "$f"; \
	done; \
	echo '# /lib/init/fstab: cleared out for bare-bones Docker' > /lib/init/fstab

RUN rm /etc/apt/apt.conf.d/docker-clean
RUN mkdir /etc/ansible/
RUN /bin/echo -e "[local]\nlocalhost ansible_connection=local" > /etc/ansible/hosts
RUN locale-gen en_US.UTF-8
RUN ssh-keygen -q -t rsa -N '' -f /root/.ssh/id_rsa && \
    cp /root/.ssh/id_rsa.pub /root/.ssh/authorized_keys && \
    for key in /etc/ssh/ssh_host_*_key.pub; do echo "localhost $(cat ${key})" >> /root/.ssh/known_hosts; done
VOLUME /sys/fs/cgroup /run/lock /run /tmp
#RUN pip install coverage junit-xml
ENV container=docker
CMD ["/sbin/init"]
