FROM centos:7.9.2009

ENV TZ=Asia/Shanghai

# CentOS 7 已 EOL (2024-06-30)，切换到 vault 存档镜像源
RUN sed -i 's|^mirrorlist=|#mirrorlist=|g' /etc/yum.repos.d/CentOS-*.repo && \
    sed -i 's|^#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*.repo

RUN yum install -y epel-release && \
    sed -i 's|^mirrorlist=|#mirrorlist=|g' /etc/yum.repos.d/epel*.repo && \
    sed -i 's|^#baseurl=http://download.fedoraproject.org/pub|baseurl=https://archive.fedoraproject.org/pub|g' /etc/yum.repos.d/epel*.repo && \
    yum install -y --setopt=tsflags=nodocs \
        nginx redis mariadb-devel python36 python36-devel \
        openldap-devel supervisor git gcc wget unzip \
        net-tools sshpass rsync sshfs && \
    yum -y clean all --enablerepo='*'

RUN pip3 install --no-cache-dir --upgrade pip -i https://mirrors.aliyun.com/pypi/simple/
RUN pip3 install --no-cache-dir -i https://mirrors.aliyun.com/pypi/simple/ \
    gunicorn \
    mysqlclient \
    cryptography==36.0.2 \
    apscheduler==3.7.0 \
    asgiref==3.2.10 \
    Django==2.2.28 \
    channels==2.3.1 \
    channels_redis==2.4.1 \
    paramiko==2.11.0 \
    django-redis==4.10.0 \
    requests==2.32.0 \
    GitPython==3.1.41 \
    python-ldap==3.4.0 \
    openpyxl==3.0.3 \
    user_agents==2.2.0

RUN localedef -c -i en_US -f UTF-8 en_US.UTF-8
ENV LANG=en_US.UTF-8
ENV LC_ALL=en_US.UTF-8
RUN echo -e '\n# Source definitions\n. /etc/profile\n' >> /root/.bashrc
RUN mkdir -p /data/repos
COPY ./docs/docker/init_spug /usr/bin/
COPY ./docs/docker/nginx.conf /etc/nginx/
COPY ./docs/docker/ssh_config /etc/ssh/
COPY ./docs/docker/spug.ini /etc/supervisord.d/
COPY ./docs/docker/redis.conf /etc/
COPY ./docs/docker/entrypoint.sh /

VOLUME /data
EXPOSE 80
ENTRYPOINT ["/entrypoint.sh"]