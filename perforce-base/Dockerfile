ARG P4_BASEIMAGE=ubuntu:jammy
FROM $P4_BASEIMAGE

ENV container docker

# See: https://hub.docker.com/_/ubuntu
RUN apt-get update && apt-get install -y locales && rm -rf /var/lib/apt/lists/* \
	&& localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8

ENV LANG en_US.utf8

RUN \
  apt-get update && \
  DEBIAN_FRONTEND=noninteractive apt dist-upgrade -y && \
  DEBIAN_FRONTEND=noninteractive apt install -y wget curl unzip vim gnupg zip lsb-release iputils-ping && \
  wget -qO - https://package.perforce.com/perforce.pubkey | apt-key add - && \
  echo "${P4_PUBLIC_REPO}\n" > /etc/apt/sources.list.d/perforce.list

RUN \
  apt-get update && \
  apt-get upgrade -y && \
  apt-get autoremove -y && \
  apt-get clean -y
  DEBIAN_FRONTEND=noninteractive apt-get install -y -f helix-p4d

#ARG GOSU_VERSION=1.11
#ARG S6_OVERLAY_VERSION=1.22.1.0
#ARG TINI_VERSION=0.18.0
#ARG SYSTEMCTL_GITSHA1=73b5aff2ba6abfd254d236f1df22ff4971d44660

#RUN sed -ir '/\[updates\].*/a enabled=1' /etc/yum.repos.d/CentOS-Base.repo && \
#    yum install -y epel-release cronie-anacron tar gzip curl openssl which sudo initscripts sysvinit at && \
#    echo -ne '[perforce]\nname=Perforce\nbaseurl=http://package.perforce.com/yum/rhel/7/x86_64\nenabled=1\ngpgcheck=1\n' > /etc/yum.repos.d/perforce.repo && \
#    rpm --import https://package.perforce.com/perforce.pubkey && \
#    yum clean all --enablerepo='*' && \
#    rm -rf /var/cache/yum

#RUN curl -fsSL https://raw.githubusercontent.com/gdraheim/docker-systemctl-replacement/${SYSTEMCTL_GITSHA1}/files/docker/systemctl.py -o /bin/systemctl && \
#    curl -fsSL https://github.com/just-containers/s6-overlay/releases/download/v${S6_OVERLAY_VERSION}/s6-overlay-amd64.tar.gz | tar zxf - -C / --keep-directory-symlink --exclude ./usr/bin/execlineb && \
#    curl -fsSL https://github.com/krallin/tini/releases/download/v${TINI_VERSION}/tini-amd64 -o /usr/bin/tini && \
#    curl -fsSL https://github.com/tianon/gosu/releases/download/${GOSU_VERSION}/gosu-amd64 -o /usr/bin/gosu && \
#    chmod +x /bin/systemctl /usr/bin/gosu /usr/bin/tini

ENTRYPOINT ["/init"]
