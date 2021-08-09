FROM jenkins/jenkins:lts

ARG dockerGroupID

USER root
RUN apt-get update
RUN apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
RUN curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
# pi3 = armhf, pi4 = arm64
RUN echo \
  "deb [arch=armhf signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null
RUN apt-get update && apt-get install -y docker-ce-cli containerd.io

RUN groupadd docker -g ${dockerGroupID}
RUN usermod -aG docker jenkins
RUN newgrp docker
USER jenkins

# Install plugins
COPY --chown=jenkins:jenkins plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN jenkins-plugin-cli -f /usr/share/jenkins/ref/plugins.txt

# Load configuration as code
COPY ./jenkins-config.yaml /usr/jenkins-config.yaml
ENV CASC_JENKINS_CONFIG=/usr/jenkins-config.yaml

EXPOSE 8080
