###############
# stash-cache #
###############

FROM xcache AS stash-cache
LABEL maintainer OSG Software <help@opensciencegrid.org>

ARG BASE_YUM_REPO=testing

ENV XC_IMAGE_NAME stash-cache

RUN yum install -y stash-cache && \
    yum clean all --enablerepo=* && rm -rf /var/cache/

COPY stash-cache/cron.d/* /etc/cron.d/
RUN chmod 0644 /etc/cron.d/*
COPY stash-cache/supervisord.d/* /etc/supervisord.d/
COPY stash-cache/image-config.d/* /etc/osg/image-init.d/

# Add a placeholder authfile, incase this cache isn't registered
# and can't pull down a new one
COPY stash-cache/Authfile /run/stash-cache/Authfile
# Same for scitokens.conf
COPY stash-cache/scitokens.conf /run/stash-cache-auth/scitokens.conf

COPY stash-cache/rsyslog-stash-cache.conf /etc/rsyslog.d/stash-cache.conf

RUN mkdir -p /var/log/xrootd/stash-cache \
             /var/log/xrootd/stash-cache-auth && \
    touch    /var/log/xrootd/stash-cache/xrootd.log \
             /var/log/xrootd/stash-cache-auth/xrootd.log && \
    chown -R xrootd:xrootd /var/log/xrootd/stash-cache \
                           /var/log/xrootd/stash-cache-auth

EXPOSE 8000

#####################
# stash-cache-debug #
#####################

FROM stash-cache AS stash-cache-debug
# Install debugging tools
# Previous arg has gone out of scope
ARG BASE_YUM_REPO=testing
RUN yum -y install -y --enablerepo="osg-$BASE_YUM_REPO" \
    gdb \
    strace
