FROM ubuntu:22.04

ENV TZ=Asia/Shanghai
ENV DEBIAN_FRONTEND=noninteractive

# 安装系统依赖
RUN apt-get update && apt-get install -y --no-install-recommends \
    nginx redis-server supervisor \
    python3 python3-pip python3-dev \
    default-libmysqlclient-dev libldap2-dev libsasl2-dev \
    build-essential pkg-config \
    git wget unzip net-tools sshpass rsync sshfs \
    locales curl \
    && rm -rf /var/lib/apt/lists/*

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

RUN locale-gen en_US.UTF-8
ENV LANG=en_US.UTF-8
ENV LC_ALL=en_US.UTF-8
RUN echo -e '\n# Source definitions\n. /etc/profile\n' >> /root/.bashrc
RUN mkdir -p /data/repos /var/log/redis

COPY ./docs/docker/init_spug /usr/bin/
COPY ./docs/docker/nginx.conf /etc/nginx/
COPY ./docs/docker/ssh_config /etc/ssh/
COPY ./docs/docker/spug.ini /etc/supervisor/conf.d/spug.conf
COPY ./docs/docker/redis.conf /etc/redis.conf
COPY ./docs/docker/entrypoint.sh /

VOLUME /data
EXPOSE 80
ENTRYPOINT ["/entrypoint.sh"]