FROM alpine:latest

#Define build arg
ARG VERSION=1.14.0

RUN set -x      && \
    apk add --no-cache --virtual .build-deps \
        build-base gnupg pcre-dev wget zlib-dev curl && \ 
    \
#Retrieve, verigy and unpack nginx source \
TMP="$(mktemp -d)" && cd "$TMP"             && \
#gpg --keyserver pgp.mit.edu --recv-keys                 \
 #   B0F4253373F8F6F510D42178520A9993A1C052F8        && \
wget -q http://nginx.org/download/nginx-${VERSION}.tar.gz   && \
wget -q http://nginx.org/download/nginx-${VERSION}.tar.gz.asc && \
# gpg --verify nginx-${VERSION}.tar.gz.asc    && \
tar -xf nginx-${VERSION}.tar.gz        && \
\
#Build and install nginx \
cd nginx-${VERSION} && \
./configure \
    --with-ld-opt="-static"    \
    --with-http_sub_module      && \
    \
    make install    && \
    strip /usr/local/nginx/sbin/nginx   && \
    \
    #Clean Up       \
    cd / && rm -rf "$TMP"       && \
    apk del .build-deps             && \
    \
    #configure tasek
    ln -sf /dev/stdout /usr/local/nginx/logs/access.logs    && \
    ln -sf /dev/stderr /usr/local/nginx/logs/error.logs

    #customize static content
    COPY index.html /usr/local/nginx/html/
    COPY nginx.conf /usr/local/nginx/conf/

    #Change default stop signal from SIGTERM to SIGQUIT
    #when docker sends stop container it sends SIGTERM - to stop everythi we need SIGQUIT
    #so forward that signal to SIGQUIT
    STOPSIGNAL SIGQUIT

    #EXPOSER port
    EXPOSE 80


#HEALTHCHECK --interval=3s CMD curl --fail -m 2 http://localhost:80/ || exit 1


CMD ["-g", "daemon off;"]
ENTRYPOINT ["/usr/local/nginx/sbin/nginx"] 