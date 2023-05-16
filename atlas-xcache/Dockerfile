################
# atlas-xcache #
################

FROM xcache AS atlas-xcache
LABEL maintainer OSG Software <help@opensciencegrid.org>

# Specify the base Yum repository to get the necessary RPMs
ARG BASE_YUM_REPO=testing

ENV XC_IMAGE_NAME atlas-xcache

RUN yum install -y --enablerepo=osg-contrib \
        atlas-xcache && \
    yum install -y python3 python3-psutil python3-requests && \
    yum clean all --enablerepo=* && rm -rf /var/cache/

COPY atlas-xcache/sbin/* /usr/local/sbin/
COPY atlas-xcache/10-atlas-xcache-limits.conf /etc/security/limits.d
COPY atlas-xcache/supervisord.d/10-atlas-xcache.conf /etc/supervisord.d/
COPY atlas-xcache/image-config.d/10-atlas-xcache.sh /etc/osg/image-init.d/
COPY atlas-xcache/rsyslog-atlas-xcache.conf /etc/rsyslog.d/atlas-xcache.conf

RUN mkdir -p /var/log/xrootd/atlas-xcache && \
    touch    /var/log/xrootd/atlas-xcache/xrootd.log && \
    chown -R xrootd:xrootd /var/log/xrootd/atlas-xcache
