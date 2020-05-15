FROM ubuntu:18.04

# Create the mastodon user
ARG UID=991
ARG GID=991
RUN apt update && \
	echo "Etc/UTC" > /etc/localtime && \
	apt install -y whois wget && \
	addgroup --gid $GID mastodon && \
	useradd -m -u $UID -g $GID -d /opt/mastodon mastodon && \
	echo "mastodon:`head /dev/urandom | tr -dc A-Za-z0-9 | head -c 24 | mkpasswd -s -m sha-256`" | chpasswd && \
	rm -rf /var/cache && \
	rm -rf /var/lib/apt/lists/*
 
# Add tini
ENV TINI_VERSION="v0.18.0"
ENV TINI_SUM="12d20136605531b09a2c2dac02ccee85e1b874eb322ef6baf7561cd93f93c855"
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN echo "$TINI_SUM tini" | sha256sum -c -
RUN chmod +x /tini

# Install Flodgatt
ENV FG_VER="v0.4.7"
ENV FG_SUM="e7ebbcb3ffe52d969bc0dff3fa1140f3bbb42006270054c5fe8a94925de738a9"
ADD --chown=mastodon:mastodon https://github.com/tootsuite/flodgatt/releases/download/$FG_VER/flodgatt /flodgatt 
RUN echo "$FG_SUM /flodgatt" | sha256sum -c -
RUN chmod +x /flodgatt

ENV BIND="0.0.0.0"

# Set the run user
USER mastodon

# Set the work dir and the container entry point
WORKDIR /
ENTRYPOINT ["/tini", "--"]
EXPOSE 4000