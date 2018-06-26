FROM python:3.5-alpine

ENV MAIN_PKGS="lapack libstdc++" \
    BUILD_PKGS="g++ gfortran musl-dev lapack-dev" \
    PIP_PKGS1="cython numpy scipy" \
    PIP_PKGS2="pandas scikit-learn" 

RUN echo "*** Install Main PAckages ***"; \
    echo http://dl-cdn.alpinelinux.org/alpine/edge/main | tee /etc/apk/repositories; \
    echo http://dl-cdn.alpinelinux.org/alpine/edge/testing | tee -a /etc/apk/repositories; \
    echo http://dl-cdn.alpinelinux.org/alpine/edge/community | tee -a /etc/apk/repositories; \
    apk update; \
    apk --no-cache add ${MAIN_PKGS} && \
    echo "*** Install Build packages ***" && \
    apk --no-cache add --virtual .builddeps ${BUILD_PKGS} && \
    echo "*** Install pip packages ***" && \
    ln -s locale.h /usr/include/xlocale.h && \
    pip install --no-cache-dir ${PIP_PKGS1} && \
    pip install --no-cache-dir ${PIP_PKGS2} && \
    echo "*** Clean Install ***" && \
    apk del .builddeps && rm -rf /root/.cache
