FROM python:3.7
LABEL maintainer="shirakiya"

# change Timezone to Asia/Tokyo
ENV TZ Asia/Tokyo
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && \
    echo $TZ > /etc/timezone

ENV JUMAN_VERSION 7.01
ENV KNP_VERSION 4.19

RUN apt-get update && \
    apt-get install -y gcc g++ make wget bzip2 git curl xz-utils file sudo && \
# install JUMAN
    wget http://nlp.ist.i.kyoto-u.ac.jp/nl-resource/juman/juman-${JUMAN_VERSION}.tar.bz2 -O /tmp/juman-${JUMAN_VERSION}.tar.bz2 && \
    tar xvf /tmp/juman-${JUMAN_VERSION}.tar.bz2 -C /tmp && \
    cd /tmp/juman-${JUMAN_VERSION}/ && \
    ./configure --prefix=/usr/local/ && \
    make && \
    make install && \
# install KNP
    wget http://nlp.ist.i.kyoto-u.ac.jp/nl-resource/knp/knp-${KNP_VERSION}.tar.bz2 -O /tmp/knp-${KNP_VERSION}.tar.bz2 && \
    tar xvf /tmp/knp-${KNP_VERSION}.tar.bz2 -C /tmp && \
    cd /tmp/knp-${KNP_VERSION}/ && \
    ./configure --prefix=/usr/local --with-juman-prefix=/usr/local && \
    make && \
    make install && \
# install MeCab
    git clone --depth 1 https://github.com/taku910/mecab.git /tmp/mecab && \
    cd /tmp/mecab/mecab  && \
    ./configure  --enable-utf8-only && \
    make && \
    make install && \
    ldconfig && \
    cd /tmp/mecab/mecab-ipadic && \
    ./configure --with-charset=utf8 && \
    make && \
    make install && \
# install mecab-ipadic-NEologd
    git clone --depth 1 https://github.com/neologd/mecab-ipadic-neologd.git /tmp/mecab-ipadic-neologd && \
    cd /tmp/mecab-ipadic-neologd && \
    ./bin/install-mecab-ipadic-neologd -n -y && \
    rm -rf /tmp/* &&\
    rm -rf /var/cache/apk/*

# clean up all temporary files
RUN apt-get autoclean -y && \
    apt-get autoremove -y && \
    apt-get clean && \
    rm -rf /tmp/* /var/tmp/* && \
    rm -rf /var/lib/apt/lists/*

CMD /bin/bash
