FROM curlimages/curl:8.2.0 AS downloader
ARG XWIKI_VERSION=15.5
WORKDIR /home/curl_user
ENV SOLR_SERVER_CORE_URL=https://maven.xwiki.org/releases/org/xwiki/platform/xwiki-platform-search-solr-server-core/${XWIKI_VERSION}/xwiki-platform-search-solr-server-core-${XWIKI_VERSION}.jar
ENV SOLR_CORE_MINIMAL_URL=https://maven.xwiki.org/releases/org/xwiki/platform/xwiki-platform-search-solr-server-core-minimal/${XWIKI_VERSION}/xwiki-platform-search-solr-server-core-minimal-${XWIKI_VERSION}.zip
RUN echo $SOLR_SERVER_CORE_URL
RUN curl $SOLR_SERVER_CORE_URL -o ./xwiki-server-core.zip
RUN curl $SOLR_CORE_MINIMAL_URL -o ./xwiki-minimal-core.zip


FROM dockerqa/unzip:latest AS builder
WORKDIR /unzip
COPY --from=downloader /home/curl_user /unzip/
WORKDIR /home
RUN unzip -qq /unzip/xwiki-server-core.zip -d ./xwiki
RUN unzip -qq /unzip/xwiki-minimal-core.zip -d ./xwiki_events
RUN unzip -qq /unzip/xwiki-minimal-core.zip -d ./xwiki_extension_index
RUN unzip -qq /unzip/xwiki-minimal-core.zip -d ./xwiki_ratings

# Use the official Solr 8 image as the base image
FROM solr:8
# Copy Solr cores configuration to their respective locations
COPY --from=builder /home /opt/solr/server/solr/

ENTRYPOINT ["bash"]
CMD ["-c", "precreate-core xwiki; precreate-core xwiki_events; precreate-core xwiki_extension_index; precreate-core xwiki_ratings; exec solr -f"]
