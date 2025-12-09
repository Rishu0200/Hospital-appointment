
FROM jenkins/jenkins:lts

USER root

# Install prerequisites
RUN apt-get update && \
    apt-get install -y ca-certificates curl gnupg lsb-release && \
    mkdir -p /etc/apt/keyrings

# Add Docker's official GPG key (new method)
RUN curl -fsSL https://download.docker.com/linux/debian/gpg \
    | gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Add Docker repository
RUN echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" \
  > /etc/apt/sources.list.d/docker.list

# Install Docker CE, CLI and containerd
RUN apt-get update && \
    apt-get install -y docker-ce docker-ce-cli containerd.io && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Add jenkins user to docker group
RUN groupadd -f docker && \
    usermod -aG docker jenkins

# Create directory for Docker-in-Docker (optional)
RUN mkdir -p /var/lib/docker
VOLUME /var/lib/docker

USER jenkins
