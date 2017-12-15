FROM centos/httpd
RUN curl -L ftp://ftp.renci.org/pub/irods/releases/4.1.11/centos7/irods-resource-4.1.11-centos7-x86_64.rpm > /tmp/irods-resource-4.1.11-centos7-x86_64.rpm && \
    curl -L https://github.com/UtrechtUniversity/davrods/releases/download/4.1_1.3.0/davrods-4.1_1.3.0-1.el7.centos.x86_64.rpm > /tmp/davrods-4.1_1.3.0-1.el7.centos.x86_64.rpm && \
    curl -L ftp://ftp.renci.org/pub/irods/releases/4.1.11/centos7/irods-runtime-4.1.11-centos7-x86_64.rpm > /tmp/irods-runtime-4.1.11-centos7-x86_64.rpm && \
    curl -L ftp://ftp.renci.org/pub/irods/releases/4.1.11/centos7/irods-icommands-4.1.11-centos7-x86_64.rpm > /tmp/irods-icommands-4.1.11-centos7-x86_64.rpm && \
    yum install -y /tmp/davrods-4.1_1.3.0-1.el7.centos.x86_64.rpm  \
                 /tmp/irods-runtime-4.1.11-centos7-x86_64.rpm \
                 /tmp/irods-icommands-4.1.11-centos7-x86_64.rpm \
                 mod_ssl && \
    yum clean all && rm -rf /var/cache/yum && \
    rm /etc/httpd/conf.d/{ssl.conf,welcome.conf,userdir.conf} && \
    rm /tmp/irods-resource-4.1.11-centos7-x86_64.rpm \
       /tmp/davrods-4.1_1.3.0-1.el7.centos.x86_64.rpm  \
       /tmp/irods-icommands-4.1.11-centos7-x86_64.rpm \
       /tmp/irods-runtime-4.1.11-centos7-x86_64.rpm

COPY anchors/* /etc/pki/ca-trust/source/anchors/
RUN update-ca-trust

COPY certs/* /etc/pki/tls/certs/
COPY private/* /etc/pki/tls/private/
COPY config/davrods-* /etc/httpd/conf.d/
COPY config/irods_environment.json /etc/httpd/irods/
