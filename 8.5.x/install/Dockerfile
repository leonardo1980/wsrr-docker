FROM rodrigotsuru/wasnd-docker:8.5.5
MAINTAINER Rodrigo Tsuru <caixapostal@gmail.com>
RUN yum install -y curl \
unzip \
tar \
libaio.i686 \
libaio.x86_64  \
pam.i686 \
pam.x86_64 \
libstdc++.i686 \
libstdc++.x86_64 \
compat-libstdc++-33.i686 \
compat-libstdc++-33.x86_64 \
glibc.i686 \
glibc.x86_64 \
libgcc.i686 \
libgcc.x86_64 \
sg3_utils \
kernel-devel \
gcc-c++ \
wget \
file \
ksh.x86_64 \
patch \
ntpd \
gcc \
python-setuptools
RUN easy_install supervisor
COPY configs/supervisord.conf /etc/supervisord.conf
RUN cd /tmp && \
curl -O -L https://googledrive.com/host/0B1TfHhOuw5TLMWw2YkNITVhWWjQ/WEBSPHERE_SERVICE_REGISTRY_AND_RE.zip && \
unzip WEBSPHERE_SERVICE_REGISTRY_AND_RE.zip -d /tmp/wsrr && \
rm WEBSPHERE_SERVICE_REGISTRY_AND_RE.zip
COPY install.xml /tmp/install.xml
RUN cd /tmp/wsrr && \
/opt/IBM/InstallationManager/eclipse/tools/imcl input /tmp/install.xml -acceptLicense -showProgress -masterPasswordFile /tmp/masterpw.txt -secureStorageFile /tmp/secure.dat && \
rm -Rf /tmp/wsrr
RUN cd /tmp && \
curl -O -L https://googledrive.com/host/0B1TfHhOuw5TLMWw2YkNITVhWWjQ/v10.5_linuxx64_expc.tar.gz && \
tar xvzf v10.5_linuxx64_expc.tar.gz && \
rm v10.5_linuxx64_expc.tar.gz
COPY db2server.rsp /tmp/db2server.rsp
RUN cd /tmp/expc && \
./db2prereqcheck -v 10.5.0.5 && \
( ./db2setup -r /tmp/db2server.rsp && \
cat /tmp/db2setup.log || cat /tmp/db2setup.log ) && \
/opt/ibm/db2/V10.5/bin/db2val -o && \
cd && \
rm -Rf /tmp/expc && \
rm /tmp/db2server.rsp
