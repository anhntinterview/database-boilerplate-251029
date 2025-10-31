FROM postgres:16

ENV POSTGRES_USER=${POSTGRES_USER}
ENV POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
ENV POSTGRES_DB=${POSTGRES_DB}
ENV POSTGRES_HOSTNAME=${POSTGRES_HOSTNAME}

COPY ./init/ /docker-entrypoint-initdb.d/

# Implement SSL cert when build
RUN mkdir -p /var/lib/postgresql/ssl && \
    chmod 700 /var/lib/postgresql/ssl && \
    sh -c "openssl req -new -x509 -days 365 -nodes \
        -text -out /var/lib/postgresql/ssl/server.crt \
        -keyout /var/lib/postgresql/ssl/server.key \
        -subj '/CN=$POSTGRES_HOSTNAME'" && \
    chmod 600 /var/lib/postgresql/ssl/server.key

# Copy postgresql.conf to enable SSL
RUN echo "listen_addresses = '*'" >> /var/lib/postgresql/data/postgresql.conf && \
    echo "ssl = on" >> /var/lib/postgresql/data/postgresql.conf && \
    echo "ssl_cert_file = '/var/lib/postgresql/ssl/server.crt'" >> /var/lib/postgresql/data/postgresql.conf && \
    echo "ssl_key_file = '/var/lib/postgresql/ssl/server.key'" >> /var/lib/postgresql/data/postgresql.conf

EXPOSE 5432