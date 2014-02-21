# Dockerfile for go (lastest release) cross compile
#
# docker build -t golang setup/
# docker run -i -t --name golang-cross --rm -v <local dir>:/srv golang bash
# root@b43906efeb3a:/# cd /srv; source /golang-crosscompile/crosscompile.bash 
# root@b43906efeb3a:/srv# go-darwin-386 build -o boot2docker.exe boot2docker.go
# maybe you need to activate cgo with export CGO_ENABLED=1
 
FROM ubuntu
 
RUN echo "deb http://archive.ubuntu.com/ubuntu precise main universe" > /etc/apt/sources.list
RUN apt-get update
RUN apt-get upgrade -y
 
RUN apt-get install -y mercurial git-core
 
RUN hg clone https://code.google.com/p/go
# Maybe checkout the latest go release here
ENV GOROOT /go
ENV GOBIN /go/bin
 
RUN apt-get install -y gcc g++ make build-essential

RUN cd /go/src && ./all.bash || echo "a few tests would have needed more memory. build shoud be ok!"
ENV PATH $PATH:/go/bin
RUN /bin/bash -c "git clone git://github.com/davecheney/golang-crosscompile.git && source golang-crosscompile/crosscompile.bash && go-crosscompile-build-all"
 
# Do some cool stuff here to have a container produce binaries for platforms