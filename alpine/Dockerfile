FROM alpine:3.5

ENV LANG=en_US.UTF-8

COPY requirements.txt /tmp/requirements.txt

# add our user first to make sure the ID get assigned consistently,
# regardless of whatever dependencies get added
RUN addgroup -S mitmproxy && adduser -S -G mitmproxy mitmproxy \
    && apk add --no-cache \
        su-exec \
        git \
        g++ \
        libffi \
        libffi-dev \
        libstdc++ \
        openssl \
        openssl-dev \
        python3 \
        python3-dev \
    && python3 -m ensurepip \
    && LDFLAGS=-L/lib pip3 install -r /tmp/requirements.txt \
    && apk del --purge \
        git \
        g++ \
        libffi-dev \
        openssl-dev \
        python3-dev \
    && rm /tmp/requirements.txt \
    && rm -rf ~/.cache/pip

VOLUME /home/mitmproxy/.mitmproxy

COPY docker-entrypoint.sh /usr/local/bin/
ENTRYPOINT ["docker-entrypoint.sh"]

EXPOSE 8080 8081
CMD ["mitmproxy"]
