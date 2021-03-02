FROM centos:7
LABEL maintainer "ueno.s <ueno.s@gamestudio.co.jp>"

COPY files/get-keyvault-certificate.sh /opt/perforce/helix-auth-svc/get-keyvault-certificate.sh

# See: https://hub.docker.com/_/centos/
RUN cd /lib/systemd/system/sysinit.target.wants/ && \
    for i in *; do \
        [ $i == systemd-tmpfiles-setup.service ] || rm -vf $i ; \
    done ; \
    rm -vf /lib/systemd/system/multi-user.target.wants/* && \
    rm -vf /etc/systemd/system/*.wants/* && \
    rm -vf /lib/systemd/system/local-fs.target.wants/* && \
    rm -vf /lib/systemd/system/sockets.target.wants/*udev* && \
    rm -vf /lib/systemd/system/sockets.target.wants/*initctl* && \
    rm -vf /lib/systemd/system/basic.target.wants/* && \
    rm -vf /lib/systemd/system/anaconda.target.wants/* && \
    mkdir -p /etc/selinux/targeted/contexts/ && \
    echo '<busconfig><selinux></selinux></busconfig>' > /etc/selinux/targeted/contexts/dbus_contexts \
    && yum clean all --enablerepo='*' \
    && yum clean metadata --enablerepo='*' \
    && yum clean all --enablerepo='*' \
    && rm -rf /var/cache/yum \
    && rpm --import https://package.perforce.com/perforce.pubkey \
    && yum-config-manager --add-repo https://gist.githubusercontent.com/hexagrimoire/f235f327baacdf6f109903eec5d9a717/raw/f54bb09d6a73d390762feaf0ef3344d128677dc9/perforce.centos.repo \
    && yum install -y tzdata logrotate sudo which cronie openssl curl git gcc-c++ make \
    && curl -sL https://rpm.nodesource.com/setup_14.x | sudo -E bash - \
    && ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime \
    && localedef -i /usr/share/i18n/locales/ja_JP -f UTF-8 /usr/lib/locale/ja_JP.UTF-8 \
    && localedef --add-to-archive /usr/lib/locale/ja_JP.UTF-8 \
    && source /etc/locale.conf \
    && systemctl enable crond \
    && sudo yum -q -y install nodejs \
    && sudo npm install -g pm2 \
    && sudo yum -y install helix-auth-svc

COPY files/ecosystem.config.js /opt/perforce/helix-auth-svc/ecosystem.config.js
