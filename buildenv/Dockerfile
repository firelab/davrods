FROM centos/httpd
RUN rpm --import https://packages.irods.org/irods-signing-key.asc && \
    yum install -y epel-release && \
    curl https://packages.irods.org/renci-irods.yum.repo > /etc/yum.repos.d/renci-irods.yum.repo && \
    curl -L https://github.com/UtrechtUniversity/davrods/releases/download/4.2.1_1.3.0/davrods-4.2.1_1.3.0-1.rpm >/tmp/davrods-4.2.1_1.3.0-1.rpm&& \
    yum install -y /tmp/davrods-4.2.1_1.3.0-1.rpm  \
                 mod_ssl \
                 irods-icommands-4.2.1 && \
    yum clean all && rm -rf /var/cache/yum && \
    rm /etc/httpd/conf.d/{welcome.conf,userdir.conf} && \
    rm /tmp/davrods-4.2.1_1.3.0-1.rpm

COPY anchors/* /etc/pki/ca-trust/source/anchors/
RUN update-ca-trust

COPY certs/* /etc/pki/tls/certs/
COPY private/* /etc/pki/tls/private/
COPY config/davrods-* /etc/httpd/conf.d/
COPY config/irods_environment.json /etc/httpd/irods/
EXPOSE 443
