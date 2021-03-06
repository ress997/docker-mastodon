FROM ubuntu:18.04

# Use bash for the SHELL
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN echo "Etc/UTC" > /etc/localtime && \
	apt-get update && \
	apt-get --no-install-recommends -y install apt-utils wget whois ca-certificates && \
	rm -rf /var/cache && \
	rm -rf /var/lib/apt/lists/*

# Create the mastodon user
ARG UID=991
ARG GID=991
RUN addgroup --gid ${GID} mastodon && \
	useradd -m -u ${UID} -g ${GID} -d /opt/mastodon mastodon && \
	echo "mastodon:$(head /dev/urandom | tr -dc A-Za-z0-9 | head -c 24 | mkpasswd -s -m sha-256)" | chpasswd

# Install tini
ENV TINI_VERSION="v0.18.0"
ENV TINI_SUM="12d20136605531b09a2c2dac02ccee85e1b874eb322ef6baf7561cd93f93c855"
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN echo "${TINI_SUM} tini" | sha256sum -c - && \
	chmod +x /tini

# Install Flodgatt
ENV FG_VER="v0.9.9"
ENV FG_SUM="9fa9d89eac0d3858bb8d2c4ae14c51e5056f8f07b408c0662e8385d0394ef46a"
ADD --chown=mastodon:mastodon https://github.com/tootsuite/flodgatt/releases/download/${FG_VER}/flodgatt /flodgatt
RUN echo "${FG_SUM} /flodgatt" | sha256sum -c - && \
	chmod +x /flodgatt

ENV BIND="0.0.0.0"

# Set the run user
USER mastodon

# Set the work dir and the container entry point
WORKDIR /
ENTRYPOINT ["/tini", "--"]
EXPOSE 4000
