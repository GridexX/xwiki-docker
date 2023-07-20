# Use the official Solr 8 image as the base image
FROM solr:8

# Expose port 8983
EXPOSE 8983

# Copy Solr cores configuration to their respective locations
COPY ./config/solr /opt/solr/server/solr/

# Define the entrypoint and command for the container
ENTRYPOINT ["bash"]
CMD ["-c", "precreate-core xwiki; precreate-core xwiki_events; precreate-core xwiki_extension_index; precreate-core xwiki_ratings; exec solr -f"]
