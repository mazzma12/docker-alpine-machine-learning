FROM alpine:3.7

ARG PANDAS_VERSION="0.22.0"
ARG SCIKIT_VERSION="0.19.1"

ENV MAIN_PKGS="\
        python3 \
        python3-dev \
        # For Numpy, scipy
        lapack \
        libstdc++" \
    BUILD_PKGS="\
        g++ \
        gfortran \
        musl-dev \
        lapack-dev" \
    PIP_PKGS1="\
        cython \
        numpy \
        scipy" \
    PIP_PKGS2="\
        pandas==${PANDAS_VERSION} \
        scikit-learn==${SCIKIT_VERSION}" 

RUN echo "*** Install Main PAckages ***"; \
    echo http://dl-cdn.alpinelinux.org/alpine/edge/main | tee /etc/apk/repositories; \
    echo http://dl-cdn.alpinelinux.org/alpine/edge/testing | tee -a /etc/apk/repositories; \
    echo http://dl-cdn.alpinelinux.org/alpine/edge/community | tee -a /etc/apk/repositories; \
    apk update; \
    apk --update --no-cache add ${MAIN_PKGS} && \
    echo "*** Install Build packages ***" && \
    apk --update --no-cache add --virtual .builddeps ${BUILD_PKGS} && \
    echo "*** Install pip packages ***" && \
    ln -s locale.h /usr/include/xlocale.h && \
    pip3 install --no-cache-dir ${PIP_PKGS1} && \
    pip3 install --no-cache-dir ${PIP_PKGS2}

RUN echo "*** Clean Install ***" && \
    find / -type d -name __pycache__ -exec rm -r {} +   && \
    rm -r /usr/lib/python*/ensurepip                    && \
    rm -r /usr/lib/python*/lib2to3                      && \
    rm -r /usr/lib/python*/turtledemo                   && \
    rm /usr/lib/python*/turtle.py                       && \
    rm /usr/lib/python*/doctest.py                      && \
    rm /usr/lib/python*/pydoc.py                        && \
    apk del .builddeps && rm -rf /root/.cache
