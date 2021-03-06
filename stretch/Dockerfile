FROM bitnami/minideb@sha256:8a70d36105a46bb020a36e17a4fcbdac95d03032e57d3ee52d3f13151ba9c350
LABEL maintainer "Bitnami <containers@bitnami.com>"

RUN install_packages curl ca-certificates sudo locales procps libaio1 gnupg dirmngr && \
  update-locale LANG=C.UTF-8 LC_MESSAGES=POSIX && \
  locale-gen en_US.UTF-8 && \
  DEBIAN_FRONTEND=noninteractive dpkg-reconfigure locales && \
  useradd -ms /bin/bash bitnami && \
  mkdir -p /opt/bitnami && chown bitnami:bitnami /opt/bitnami && \
  sed -i -e 's/\s*Defaults\s*secure_path\s*=/# Defaults secure_path=/' /etc/sudoers && \
  echo "bitnami ALL=NOPASSWD: ALL" >> /etc/sudoers

ENV NAMI_VERSION 0.0.6-0

RUN cd /tmp && \
  curl -sSLO https://nami-prod.s3.amazonaws.com/tools/nami/releases/nami-$NAMI_VERSION-linux-x64.tar.gz && \
  echo "08c474bf7b866879cd709f7c3efaebec43cfc2eb39c974fd83c18eb19821129a  nami-$NAMI_VERSION-linux-x64.tar.gz" | sha256sum -c - && \
  mkdir -p /opt/bitnami/nami && \
  tar xzf nami-$NAMI_VERSION-linux-x64.tar.gz --strip 1 -C /opt/bitnami/nami && \
  rm nami-$NAMI_VERSION-linux-x64.tar.gz

ENV TINI_VERSION v0.13.2

RUN cd /tmp && \
  gpg --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 595E85A6B1B4779EA4DAAEC70B588DFF0527A9B7 && \
  gpg --fingerprint 595E85A6B1B4779EA4DAAEC70B588DFF0527A9B7 | grep -q "6380 DC42 8747 F6C3 93FE  ACA5 9A84 159D 7001 A4E5" && \
  curl -sSL https://github.com/krallin/tini/releases/download/$TINI_VERSION/tini.asc -o tini.asc && \
  curl -sSL https://github.com/krallin/tini/releases/download/$TINI_VERSION/tini -o /usr/local/bin/tini && \
  gpg --verify tini.asc /usr/local/bin/tini && \
  chmod +x /usr/local/bin/tini && \
  rm tini.asc

ENV GOSU_VERSION=1.10 \
    GOSU_GPG_KEY=B42F6819007F00F88E364FD4036A9C25BF357DD4

RUN cd /tmp && \
  gpg --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys $GOSU_GPG_KEY && \
  gpg --fingerprint $GOSU_GPG_KEY | grep -q "B42F 6819 007F 00F8 8E36  4FD4 036A 9C25 BF35 7DD4" && \
  curl -sSL https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-amd64.asc -o gosu.asc && \
  curl -sSL https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-amd64 -o /usr/local/bin/gosu && \
  gpg --verify gosu.asc /usr/local/bin/gosu && \
  chmod +x /usr/local/bin/gosu && \
  rm gosu.asc

ENV PATH=/opt/bitnami/nami/bin:$PATH
ENV BITNAMI_IMAGE_VERSION=stretch-r0

COPY rootfs /
ENTRYPOINT ["/entrypoint.sh"]
