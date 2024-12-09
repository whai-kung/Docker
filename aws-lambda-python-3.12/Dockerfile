# https://github.com/aws/aws-sam-build-images/blob/develop/build-image-src/Dockerfile-python312
FROM public.ecr.aws/sam/build-python3.12:latest

LABEL python="3.12"
LABEL node="22.x"
LABEL poetry="1.8.4"
LABEL yarn="1.22.22"

# Add poetry to PATH
# https://stackoverflow.com/a/38905161/2426653
# https://python-poetry.org/docs/#installation
ENV PATH="$PATH:~/.local/bin"


# Install all other packages
RUN yum install -y nano wget zip


ARG TARGETPLATFORM
RUN echo "@@@ @@@ @@@ @@@ Building for ${TARGETPLATFORM}"

# Required for lxml 4.9.3 [on arm64 only]
ARG TARGETPLATFORM
RUN ARCH="$TARGETPLATFORM"; \
  if [ "$ARCH" == "linux/arm64" ]; then \
    yum install -y libxslt-devel libxml2-devel; \
  else \
    echo "x86 skipping lxml dependency install"; \
  fi


# Install nodejs 22.x:
# https://stackoverflow.com/a/35165401/2426653
# Cannot install 18.x due to glibc 2.28 dependency:
# https://stackoverflow.com/a/74896209/2426653
RUN yum install -y https://rpm.nodesource.com/pub_22.x/nodistro/repo/nodesource-release-nodistro-1.noarch.rpm
RUN yum install -y nodejs --setopt=nodesource-nodejs.module_hotfixes=1


# Install poetry and sorting plugin
RUN curl -sSL https://install.python-poetry.org | python3 - --version 1.8.4 && \
  ~/.local/share/pypoetry/venv/bin/poetry self add poetry-plugin-sort


# install yarn classic
RUN curl -sL https://dl.yarnpkg.com/rpm/yarn.repo -o /etc/yum.repos.d/yarn.repo && \
  yum install -y yarn


# Install Go
# https://github.com/aws/aws-sam-build-images/blob/develop/build-image-src/Dockerfile-go1x
# https://stackoverflow.com/a/67601735/2426653
ARG TARGETPLATFORM
RUN ARCH="$TARGETPLATFORM"; \
  if [ "$ARCH" == "linux/arm64" ]; then ARCH=arm64; else ARCH=amd64; fi; \
  curl -L https://go.dev/dl/$(curl -L "https://go.dev/VERSION/?m=text" | grep go1.).linux-${ARCH}.tar.gz | tar -zx -C /usr/local
ENV PATH=$PATH:/usr/local/go/bin:/root/go/bin
# Set GOPROXY envvar to avoid using the default proxy.golang.org proxy
ENV GOPROXY=direct


# Set bash prompt
# Using bash_profile doesn't work
# Using ~/.bashrc doesn't work
RUN echo "PS1='\w: '" >> /root/.bashrc
# Ignore duplicates in history
RUN echo "export HISTCONTROL=ignoreboth:erasedups" >> /root/.bashrc