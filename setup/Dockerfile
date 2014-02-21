# Dockerfile for go (lastest release) cross compile
#
# docker build -t golang setup/
# docker run -i -t --name golang --rm -v <local dir>:/srv golang bash
# root@xx:/# cd /srv
# root@xx:/srv# make
 
FROM ubuntu:13.10
MAINTAINER Andreas Heissenberger <andreas@heissenberger.at> (@aheissenberger)

# Packaged dependencies
RUN	apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -yq \
	automake \
	build-essential \
	curl \
	ca-certificates \
	git \
	mercurial \
	--no-install-recommends

# Install Go
RUN	curl -s https://go.googlecode.com/files/go1.2.src.tar.gz | tar -v -C /usr/local -xz
ENV	PATH	/usr/local/go/bin:$PATH
ENV	GOPATH	/go:/go/src/github.com/dotcloud/docker/vendor
RUN	cd /usr/local/go/src && ./make.bash --no-clean 2>&1

# Compile Go for cross compilation
ENV	DOCKER_CROSSPLATFORMS	linux/386 linux/arm darwin/amd64 darwin/386 windows/386 windows/amd64
# (set an explicit GOARM of 5 for maximum compatibility)
ENV	GOARM	5
RUN	cd /usr/local/go/src && bash -xc 'for platform in $DOCKER_CROSSPLATFORMS; do GOOS=${platform%/*} GOARCH=${platform##*/} ./make.bash --no-clean 2>&1; done'
 
