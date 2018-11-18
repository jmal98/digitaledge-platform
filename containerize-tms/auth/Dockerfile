FROM ubuntu:16.04

RUN apt-get update \
 && apt-get install -y \
 			ca-certificates \
 			curl \
 			dnsutils \
            gcc \
            jq \
            logrotate \
            libssl-dev \
            libffi-dev \
            make \
            net-tools \
            openjdk-8-jdk \
            openssl \
            python-cffi \
            python-crypto \
            python-dev \
            python-openssl \
            python-setuptools \
            python-yaml \
            sudo \
            tofrodos \
            tar \
            unzip \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/* \
 && easy_install setuptools cryptography==1.5    

RUN easy_install ansible==2.7.1 \
 && mkdir -p $PLAYBOOK /mnt/appfs/release/$RTWS_RELEASE /etc/ansible \
 && echo 'localhost' > /etc/ansible/hosts \
 && apt-get -y remove \
 			gcc \
 			make \
 && apt-get -y autoremove \
 && rm -rf /var/lib/apt/lists/*
 
EXPOSE 443 8161 8443 10389

CMD ["/sbin/bootstrap"]

ENV PLAYBOOK=/tmp/.pbook

ARG RTWS_RELEASE=unknown

ADD tms-process-groups.tar.gz $PLAYBOOK/

#ADD params.yml $PLAYBOOK/tms-process-groups/

COPY release/* /mnt/appfs/release/$RTWS_RELEASE/

COPY configuration/* /mnt/appfs/configuration/tms.local.deleidos.com/

RUN touch /etc/default/is_tms_auth \
 && ansible-playbook $PLAYBOOK/tms-process-groups/site.yml -c local --extra-vars rtws_release=$RTWS_RELEASE \
 && rm -rf $PLAYBOOK /etc/default/is_tms_auth
