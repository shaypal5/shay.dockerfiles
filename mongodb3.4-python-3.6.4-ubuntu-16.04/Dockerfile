FROM shaypal5/python-3.6.4-ubuntu-16.04:v0.0.2

LABEL maintainer="shaypal5"

#so the repository lists can be update
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6 && \
echo "deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-3.4.list && \
apt-get update && \
apt-get install -y mongodb-org && \
# service mongod start && \
# service mongod stop && \
mongoexport --version

# some extra stuff - actually not needed; everything already install by the image we depend on
# RUN apt-get install -y ca-certificates \
#         openssh-client \
#         git 

# We have to create a new mongodb systemd service file in the '/lib/systemd/system' directory
# ADD mongod.service /lib/systemd/system/
#
# RUN systemctl daemon-reload && \
# systemctl start mongod && \
# systemctl enable mongod && \
# mongoexport --version && \
# netstat -plntu 
