FROM alpine:3.7

# NOTE: pip is installed with python3 in alpine repos.

RUN apk add --update --no-cache python3 python3-dev && \
    find / -type d -name __pycache__ -exec rm -r {} +   && \
    rm -r /usr/lib/python*/ensurepip                    && \
    rm -r /usr/lib/python*/lib2to3                      && \
    rm -r /usr/lib/python*/turtledemo                   && \
    rm /usr/lib/python*/turtle.py                       && \
    rm /usr/lib/python*/doctest.py                      && \
    rm /usr/lib/python*/pydoc.py                        && \
    echo "*** Clean Install ***" && \
    rm -rf /root/.cache /var/cache /usr/share/terminfo
