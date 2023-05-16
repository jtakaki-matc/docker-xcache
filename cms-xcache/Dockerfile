##############
# cms-xcache #
##############

FROM xcache AS cms-xcache
LABEL maintainer OSG Software <help@opensciencegrid.org>

ARG BASE_YUM_REPO=testing

ENV XC_IMAGE_NAME cms-xcache

RUN yum install -y \
                cms-xcache \
                xcache-consistency-check && \
    yum clean all --enablerepo=* && rm -rf /var/cache/

COPY cms-xcache/limits.d/10-cms-xcache-limits.conf /etc/security/limits.d/
COPY cms-xcache/supervisord.d/10-cms-xcache.conf /etc/supervisord.d/
COPY cms-xcache/cron.d/* /etc/cron.d/
RUN chmod 0644 /etc/cron.d/*
COPY cms-xcache/image-config.d/* /etc/osg/image-init.d/
COPY cms-xcache/xcache-consistency-check-wrapper.sh /usr/bin/xcache-consistency-check-wrapper.sh
COPY cms-xcache/rsyslog-cms-xcache.conf /etc/rsyslog.d/cms-xcache.conf

RUN mkdir -p /var/log/xrootd/cms-xcache && \
    touch    /var/log/xrootd/cms-xcache/xrootd.log && \
    chown -R xrootd:xrootd /var/log/xrootd/cms-xcache

EXPOSE 1094

####################
# cms-xcache-debug #
####################

FROM cms-xcache AS cms-xcache-debug
# Install debugging tools
# Previous arg has gone out of scope
ARG BASE_YUM_REPO=testing
RUN yum -y install -y --enablerepo="osg-$BASE_YUM_REPO" \
    gdb \
    strace
