# Docker file for wukong-robot
FROM ubuntu:bionic
MAINTAINER zwcway

RUN set -xe \
    && sed -i 's/archive\.ubuntu\.com/mirrors\.aliyun\.com/g' /etc/apt/sources.list \
    && apt-get update -y \
    && apt-get install -y --fix-missing wget git libssl-dev swig build-essential python3-pyaudio sox \
        ffmpeg python3 python3-dev python3-pip python3-venv libpcre3 libpcre3-dev libatlas-base-dev \
        alsa-base pulseaudio libsox-fmt-all automake bison flex libasound2-dev python-alsaaudio \
    && apt-get clean && rm -rf /var/lib/apt/lists/*
RUN set -xe \
    && cd /root/ \
    && wget https://bootstrap.pypa.io/get-pip.py --no-check-certificate \
    && python3 get-pip.py && rm get-pip.py \
    && python3 -m pip install pip --upgrade && python3 -m pip install wheel \
    && rm -rf /root/.cache/* /var/cache/*
RUN set -xe \
    && cd /root/ \
    && wget https://github.com/swig/swig/archive/rel-3.0.10.tar.gz -O swig-rel-3.0.10.tar.gz \
    && tar zxvf swig-rel-3.0.10.tar.gz && cd /root/swig-rel-3.0.10 \
    && ./autogen.sh && ./configure --prefix=/usr --without-clisp --without-maximum-compile-warnings \
    && make && make install \
#    && install -v -m755 -d /usr/share/doc/swig-3.0.10 && cp -v -R Doc/* /usr/share/doc/swig-3.0.10 \
    && rm -rf /root/swig*
RUN set -xe \
    && cd /root/ \
    && git clone --depth=1 https://github.com/zwcway/home-robot.git home-robot \
    && cd /root/home-robot \
    && git pull -f \
    && pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple -r requirements.txt \
    && rm -rf /root/home-robot/temp && ln -s /tmp /root/home-robot/temp \
    && rm -rf /root/home-robot/.git \
    && rm -rf /root/.cache/* /var/cache/*
RUN set -xe \
    && cd /root/ \
    && wget https://github.com/Kitt-AI/snowboy/archive/v1.3.0.tar.gz -O snowboy-1.3.0.tar.gz \
    && tar -zxvf snowboy-1.3.0.tar.gz \
    && cd /root/snowboy-1.3.0/swig/Python3 \
    && make && cp _snowboydetect.so /root/home-robot/snowboy/ \
    && rm -rf /root/snowboy*
RUN set -xe \
    && mkdir /root/.robot \
    && cd /root/.robot \
    && git clone --depth=1 https://github.com/wzpan/wukong-contrib.git contrib \
    && cd /root/.robot/contrib \
    && git pull -f \
    && pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple -r requirements.txt \
    && rm -rf /root/.cache/* /var/cache/* \
    && rm -rf /root/.robot/contrib/.git
RUN set -xe \
    && echo '#!/bin/bash' > /run.sh \
    && echo 'pulseaudio -D' >> /run.sh \
    && echo 'python3 wukong.py' >> /run.sh \
    && chmod +x /run.sh
ENV LANG=C.UTF-8
ENV LC_ALL=C.UTF-8
VOLUME ["/root/.robot","/tmp"]
WORKDIR /root/home-robot

CMD ["/run.sh"]
EXPOSE 5000
