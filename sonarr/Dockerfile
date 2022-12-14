FROM debian:11 as base
ENV DEBIAN_FRONTEND=noninteractive

FROM base as build
RUN apt-get update && apt-get install -y \
        curl &&                          \
    apt-get clean &&                     \
    rm -rf /var/lib/apt/
RUN curl -L                                                                       \
        'https://services.sonarr.tv/v1/download/main/latest?version=3&os=linux' | \
    tar -xz &&                                                                    \
    mv Sonarr/ /app/

FROM base
WORKDIR /app/
RUN apt-get update && apt-get install -y                     \
        libmono-corlib4.5-cil                                \
        libmono-microsoft-csharp4.0-cil                      \
        libmono-posix4.0-cil                                 \
        libmono-system-componentmodel-dataannotations4.0-cil \
        libmono-system-configuration-install4.0-cil          \
        libmono-system-configuration4.0-cil                  \
        libmono-system-core4.0-cil                           \
        libmono-system-data-datasetextensions4.0-cil         \
        libmono-system-data4.0-cil                           \
        libmono-system-identitymodel4.0-cil                  \
        libmono-system-io-compression4.0-cil                 \
        libmono-system-net-http4.0-cil                       \
        libmono-system-numerics4.0-cil                       \
        libmono-system-runtime-serialization4.0-cil          \
        libmono-system-security4.0-cil                       \
        libmono-system-servicemodel4.0a-cil                  \
        libmono-system-serviceprocess4.0-cil                 \
        libmono-system-transactions4.0-cil                   \
        libmono-system-web4.0-cil                            \
        libmono-system-xml-linq4.0-cil                       \
        libmono-system-xml4.0-cil                            \
        libmono-system4.0-cil                                \
        mediainfo                                            \
        mono-runtime &&                                      \
    apt-get clean &&                                         \
    rm -rf /var/lib/apt/
COPY --from=build /app/ .
COPY docker-entrypoint.sh /usr/local/bin/
VOLUME /data/ /config/
RUN useradd -m app
EXPOSE 8989
ENTRYPOINT ["docker-entrypoint.sh"]
CMD ["mono", "./Sonarr.exe", "--nobrowser", "--data=/config"]
