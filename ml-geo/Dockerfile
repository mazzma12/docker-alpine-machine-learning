FROM alpine:3.7

ENV MAIN_PKGS="\
    # For GEO reqs
    geos geos-dev gdal gdal-dev \
    libressl2.7-libcrypto  \
    freetype-dev libpng-dev \
    # Core
    python3 python3-dev lapack libstdc++" \
    BUILD_PKGS="\
    cmake make git \
    file binutils \
    g++ gfortran musl-dev lapack-dev" \
    PIP_PKGS1="cython numpy scipy fiona shapely" \
    PIP_PKGS2="pandas scikit-learn matplotlib geopandas rtree" 

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

ARG N_PROC=
RUN echo "**** compile spatialindex ****" && \
    git clone https://github.com/libspatialindex/libspatialindex /tmp/spatialindex && \
    cd /tmp/spatialindex && \
    cmake . \
    -DCMAKE_INSTALL_PREFIX=/usr && \
    make -j $(grep -c processor /proc/cpuinfo) && \
    make install

RUN echo "*** Clean Install ***" && \
    find / -type d -name __pycache__ -exec rm -r {} +   && \
    rm -r /usr/lib/python*/ensurepip                    && \
    rm -r /usr/lib/python*/lib2to3                      && \
    rm -r /usr/lib/python*/turtledemo                   && \
    rm /usr/lib/python*/turtle.py                       && \
    rm /usr/lib/python*/webbrowser.py                   && \
    rm /usr/lib/python*/doctest.py                      && \
    rm /usr/lib/python*/pydoc.py                        && \
    find /usr/lib/python3.*/ -name 'tests' -exec rm -r '{}' + && \
    find /usr/lib/python3.*/site-packages/ -name '*.so' -print -exec sh -c 'file "{}" | grep -q "not stripped" && strip -s "{}"' \; && \
    rm /usr/include/xlocale.h && \
    apk del .builddeps && rm -rf /root/.cache
