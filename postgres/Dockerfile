FROM postgres:16

# Environment variables
ENV POSTGRES_USER=${POSTGRES_USER}
ENV POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
ENV POSTGRES_DB=${POSTGRES_DB}
ENV POSTGRES_HOSTNAME=${POSTGRES_HOSTNAME}

# Copy init scripts
COPY ./init/ /docker-entrypoint-initdb.d/

# Create SSL certificate with full DN
RUN mkdir -p /var/lib/postgresql/ssl && \
    openssl req -new -x509 -days 365 -nodes \
        -out /var/lib/postgresql/ssl/server.crt \
        -keyout /var/lib/postgresql/ssl/server.key \
        -subj "/C=VN/ST=Ho Chi Minh/L=Ho Chi Minh City/O=ssre/OU=DB/CN=${POSTGRES_HOSTNAME}" && \
    chmod 600 /var/lib/postgresql/ssl/server.key && \
    chown -R postgres:postgres /var/lib/postgresql/ssl

# Copy separate postgresql.conf to enable SSL
COPY postgresql.conf /etc/postgresql/postgresql.conf

EXPOSE 5432

# Use custom config file
CMD ["postgres", "-c", "config_file=/etc/postgresql/postgresql.conf"]
