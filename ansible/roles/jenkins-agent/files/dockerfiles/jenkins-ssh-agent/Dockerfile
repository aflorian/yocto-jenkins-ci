FROM jenkins/ssh-agent:bullseye-jdk11@sha256:ccb105ee72c1399c8331d9a43917d99c2fddbd4216bd8c347d5a2e42358b7447

RUN apt-get update \
  && apt-get install -y --no-install-recommends \
    apt-transport-https \
    ca-certificates \
    curl \
    git \
    gnupg2 \
    software-properties-common

#
# Docker install
#
RUN apt-get update && apt-get install --no-install-recommends -y \
       apt-transport-https \
       ca-certificates \
       curl \
       gnupg2 \
       software-properties-common
RUN curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -
RUN apt-key fingerprint 0EBFCD88
RUN add-apt-repository \
       "deb [arch=amd64] https://download.docker.com/linux/debian \
       $(lsb_release -cs) \
       stable"
RUN apt-get update && apt-get install --no-install-recommends -y \
       docker-ce docker-ce-cli containerd.io

RUN usermod -a -G docker jenkins

#
# supervisord
#
RUN apt-get install --no-install-recommends -y supervisor
RUN mkdir -p /var/log/supervisor
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

#
# Entrypoint
#
COPY docker-entrypoint.sh /usr/bin/docker-entrypoint.sh
RUN chmod +x /usr/bin/docker-entrypoint.sh

#
# prepare mount point directory for artifacts and other persisted data
#
RUN mkdir -p /var/jenkins && chown jenkins:jenkins -R /var/jenkins

ENTRYPOINT ["/usr/bin/docker-entrypoint.sh"]
