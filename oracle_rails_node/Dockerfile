# ARG REPO_URI=738086221825.dkr.ecr.ap-southeast-1.amazonaws.com
# ARG BASE_IMAGE_TAG=12.2

FROM centos:7 as init

LABEL maintainer="wong_yan_yee@tech.gov.sg" \
      description="Base image for bgp-react application. Oracle Linux 7 slim + Ruby 2.3.1 + Node 7.1.0"

RUN yum install -y git \
    && yum clean all

RUN git clone git://github.com/rbenv/rbenv.git /usr/local/rbenv \
    && git clone git://github.com/rbenv/ruby-build.git /usr/local/rbenv/plugins/ruby-build \
    && git clone git://github.com/jf/rbenv-gemset.git /usr/local/rbenv/plugins/rbenv-gemset \
    && /usr/local/rbenv/plugins/ruby-build/install.sh

# multi-stage with govtechsg sqlplus image
FROM govtechsg/cicd-images:sqlplus-12.2-latest

COPY --from=init /usr/local/rbenv /usr/local/rbenv

# ----------------------------------------
# install necessary tools needed to compile
# ----------------------------------------
RUN yum install -y \
    sudo \
    # epel-release \
    # git \
    curl \
    # wget \
    # python \
    readline-devel \
    openssl-devel \
    zlib-devel \
    # && yum group install "Development Tools" -y \
    bzip2 \
    tar \
    gzip \
    gcc \
    gcc-c++ \
    make \
    procps \
    file \
    && yum clean all

# wget https://bootstrap.pypa.io/get-pip.py \
# RUN curl -O https://bootstrap.pypa.io/get-pip.py \
#    &&  python get-pip.py

# -------------------------------
# install ruby
# -------------------------------
ARG RUBY_VERSION=2.3.1
ENV RUBY_VERSION=${RUBY_VERSION} \
    PATH=/usr/local/rbenv/bin:$PATH \
    RBENV_ROOT=/usr/local/rbenv

RUN echo 'export RBENV_ROOT=/usr/local/rbenv' >> /etc/profile.d/rbenv.sh \
    &&  echo 'export PATH=/usr/local/rbenv/bin:$PATH' >> /etc/profile.d/rbenv.sh \
    &&  echo 'eval "$(rbenv init -)"' >> /etc/profile.d/rbenv.sh

RUN echo 'export RBENV_ROOT=/usr/local/rbenv' >> /etc/bashrc \
    &&  echo 'export PATH=/usr/local/rbenv/bin:$PATH' >> /etc/bashrc \
    &&  echo 'eval "$(rbenv init -)"' >> /etc/bashrc

ENV CONFIGURE_OPTS --disable-install-doc
ENV PATH /usr/local/rbenv/bin:/usr/local/rbenv/shims:$PATH

RUN eval "$(rbenv init -)"; rbenv install ${RUBY_VERSION} \
    &&  eval "$(rbenv init -)"; rbenv global ${RUBY_VERSION} \
    &&  eval "$(rbenv init -)"; gem install bundler

# -------------------------------
# install node
# -------------------------------
ARG NODEJS_VERSION=7.1.0
ENV NODEJS_VERSION=${NODEJS_VERSION}

RUN echo "Installing Node.js" \
    && curl -sL https://rpm.nodesource.com/setup_$(echo $NODEJS_VERSION | cut -d'.' -f1).x | bash - \
    && yum install -y "nodejs-${NODEJS_VERSION}-1nodesource.el7.centos" \
    && yum clean all

# -------------------------------
# install passenger + yarn
# -------------------------------

ENV PASSENGER_VERSION='6.0.2'

RUN gem install passenger -v "${PASSENGER_VERSION}" --no-rdoc --no-ri && \
    passenger-config install-standalone-runtime

# -------------------------------
# entrypoint
# -------------------------------
ENTRYPOINT /bin/bash
