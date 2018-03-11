FROM circleci/python:3.6.4-jessie-node

LABEL maintainer="shaypal5@gmail.com"

RUN sudo apt-get update \
	&& sudo apt-get install -y --no-install-recommends \
		ca-certificates \
		jq \
		numactl \
	&& sudo rm -rf /var/lib/apt/lists/*

# grab gosu for easy step-down from root (https://github.com/tianon/gosu/releases)
ENV GOSU_VERSION 1.10
# grab "js-yaml" for parsing mongod's YAML config files (https://github.com/nodeca/js-yaml/releases)
ENV JSYAML_VERSION 3.10.0

RUN sudo set -ex; \
	\
	sudo apt-get update; \
	sudo apt-get install -y --no-install-recommends \
		wget \
	; \
	sudo rm -rf /var/lib/apt/lists/*; \
	\
	sudo dpkgArch="$(dpkg --print-architecture | awk -F- '{ print $NF }')"; \
	sudo wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$dpkgArch"; \
	sudo wget -O /usr/local/bin/gosu.asc "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$dpkgArch.asc"; \
	sudo export GNUPGHOME="$(mktemp -d)"; \
	sudo gpg --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4; \
	sudo gpg --batch --verify /usr/local/bin/gosu.asc /usr/local/bin/gosu; \
	sudo rm -r "$GNUPGHOME" /usr/local/bin/gosu.asc; \
	sudo chmod +x /usr/local/bin/gosu; \
	sudo gosu nobody true; \
	\
	sudo wget -O /js-yaml.js "https://github.com/nodeca/js-yaml/raw/${JSYAML_VERSION}/dist/js-yaml.js"; \
# TODO some sort of download verification here
	\
	sudo apt-get purge -y --auto-remove wget

ENV GPG_KEYS \
# pub   4096R/A15703C6 2016-01-11 [expires: 2018-01-10]
#       Key fingerprint = 0C49 F373 0359 A145 1858  5931 BC71 1F9B A157 03C6
# uid                  MongoDB 3.4 Release Signing Key <packaging@mongodb.com>
	0C49F3730359A14518585931BC711F9BA15703C6
# https://docs.mongodb.com/manual/tutorial/verify-mongodb-packages/#download-then-import-the-key-file
RUN sudo set -ex; \
	sudo export GNUPGHOME="$(mktemp -d)"; \
	for key in $GPG_KEYS; do \
		sudo gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$key"; \
	done; \
	sudo gpg --export $GPG_KEYS > /etc/apt/trusted.gpg.d/mongodb.gpg; \
	sudo rm -r "$GNUPGHOME"; \
	sudo apt-key list

# Allow build-time overrides (eg. to build image with MongoDB Enterprise version)
# Options for MONGO_PACKAGE: mongodb-org OR mongodb-enterprise
# Options for MONGO_REPO: repo.mongodb.org OR repo.mongodb.com
# Example: docker build --build-arg MONGO_PACKAGE=mongodb-enterprise --build-arg MONGO_REPO=repo.mongodb.com .
ARG MONGO_PACKAGE=mongodb-org
ARG MONGO_REPO=repo.mongodb.org
ENV MONGO_PACKAGE=${MONGO_PACKAGE} MONGO_REPO=${MONGO_REPO}

ENV MONGO_MAJOR 3.4
ENV MONGO_VERSION 3.4.13

RUN sudo echo "deb http://$MONGO_REPO/apt/debian jessie/${MONGO_PACKAGE%-unstable}/$MONGO_MAJOR main" | sudo tee "/etc/apt/sources.list.d/${MONGO_PACKAGE%-unstable}.list"

RUN sudo apt-get update \
	&& sudo apt-get install -y --force-yes \
		${MONGO_PACKAGE}-shell=$MONGO_VERSION \
		${MONGO_PACKAGE}-tools=$MONGO_VERSION \
	&& sudo rm -rf /var/lib/apt/lists/* \
	&& sudo rm -rf /var/lib/mongodb \
    && mongoexport --version
