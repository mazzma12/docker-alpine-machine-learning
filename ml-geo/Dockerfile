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

ENV GEO_BUILD_PACKAGES="
    cmake make git \
    file binutils \
    "
ENV GEO_MAIN_PKGS="\
    geos geos-dev gdal gdal-dev \
    libressl2.7-libcrypto  \
    freetype-dev libpng-dev \
    "
ENV GEO_PIP_PKGS1="\
    fiona shapely \
    "
ENV GEO_PIP_PKGS2="\
    geopandas matplotlib rtree \
    "

RUN apk add --update --no-cache --virtual .builddeps ${GEO_BUILD_PKGS}
RUN apk add --update --no-cache ${GEO_MAIN_PKGS}
RUN pip install --no-cache ${GEO_PIP_PKGS1}     
RUN pip install --no-cache ${GEO_PIP_PKGS2}     

RUN echo "**** compile spatialindex ****" && \
    git clone https://github.com/libspatialindex/libspatialindex /tmp/spatialindex && \
    cd /tmp/spatialindex && \
    cmake . \
    -DCMAKE_INSTALL_PREFIX=/usr && \
    make j-4 && \
    make install

RUN echo "*** Clean Install ***" && \
    rm -r /root/.cache && \
    find /usr/lib/python3.*/ -name 'tests' -exec rm -r '{}' + && \
    find /usr/lib/python3.*/site-packages/ -name '*.so' -print -exec sh -c 'file "{}" | grep -q "not stripped" && strip -s "{}"' \; && \
    rm /usr/include/xlocale.h && \
    apk del .builddeps && rm -rf /root/.cache
