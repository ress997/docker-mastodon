FROM rust as build-dep

WORKDIR /usr/src/ragequit
RUN git clone --depth 1 https://github.com/tootsuite/ragequit /usr/src/ragequit
RUN cargo build --release

FROM ubuntu:18.04

COPY --from=build-dep /usr/src/ragequit/target/release/ragequit /opt/mastodon/

# Create the mastodon user
ARG UID=991
ARG GID=991
RUN apt update && \
	echo "Etc/UTC" > /etc/localtime && \
	apt install -y whois wget && \
	addgroup --gid $GID mastodon && \
	useradd -m -u $UID -g $GID -d /opt/mastodon mastodon && \
	echo "mastodon:`head /dev/urandom | tr -dc A-Za-z0-9 | head -c 24 | mkpasswd -s -m sha-256`" | chpasswd && \
	ln -s /opt/mastodon /mastodon && \
	rm -rf /var/cache && \
	rm -rf /var/lib/apt/lists/*

# Add tini
ENV TINI_VERSION="0.18.0"
ENV TINI_SUM="12d20136605531b09a2c2dac02ccee85e1b874eb322ef6baf7561cd93f93c855"
ADD https://github.com/krallin/tini/releases/download/v${TINI_VERSION}/tini /tini
RUN echo "$TINI_SUM tini" | sha256sum -c -
RUN chmod +x /tini

# Set the run user
USER mastodon

# Set the work dir and the container entry point
WORKDIR /opt/mastodon
ENTRYPOINT ["/tini", "--"]
