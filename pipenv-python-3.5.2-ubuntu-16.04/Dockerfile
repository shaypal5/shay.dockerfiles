FROM ubuntu:16.04

LABEL maintainer="t2lf1s0hxgovr89odd5oqh71mo3ltj"

#so the repository lists can be update
RUN apt-get update -y

#needed so Python will be able to copy Pip
RUN apt-get install -y libssl-dev openssl && \
#install wget, make, and gcc
apt-get install -y wget && \
apt-get install -y make && \
apt-get install -y gcc && \
apt-get install -y git && \
apt-get install -y libsqlite3-dev && \
#so the www.python.org certificate will be recognized
apt-get install -y ca-certificates

#get source for Python3.5
RUN wget https://www.python.org/ftp/python/3.5.2/Python-3.5.2.tgz

#decompress and extract
RUN tar xzvf Python-3.5.2.tgz

#build Python3.5 from source
RUN cd Python-3.5.2 && \
./configure && \
make && \
make install && \
#create alias so typing python always pulls up python3.5
echo "alias python=python3" >> ~/.bashrc && \
alias python="python3"

# install pip
RUN apt-get install -y python3-pip && \
#create alias so typing pip always pulls up pip3
echo "alias pip=pip3" >> ~/.bashrc && \
alias pip="pip3" && \
pip3 install --upgrade pip && \
pip3 -V

# install pyenv
RUN pip3 install pipenv

#clean up environment
RUN rm -rf Python-3.5.2 Python-3.5.2.tgz
