FROM postgres:9.3
MAINTAINER njordan@hsr.ch

RUN localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8
ENV LANG en_US.utf8

RUN ["echo", "deb http://http.debian.net/debian jessie main", ">>", "/etc/apt/sources.list"]
RUN DEBIAN_FRONTEND=noninteractive apt-get update &&\
    DEBIAN_FRONTEND=noninteractive apt-get -y install\
    postgresql-server-dev-$PG_MAJOR\
    postgis\
    make\
    patch

WORKDIR /build/pgtap
COPY ./pgtap/pgtap-0.95.0 /build/pgtap

# for the tap extensions
RUN cpan TAP::Parser::SourceHandler::pgTAP

RUN make && make install

COPY ./docker-entrypoint-initdb.d /docker-entrypoint-initdb.d
