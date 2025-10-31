FROM mongo:7.0

ENV MONGO_INITDB_ROOT_USERNAME=${MONGO_INITDB_ROOT_USERNAME}
ENV MONGO_INITDB_ROOT_PASSWORD=${MONGO_INITDB_ROOT_PASSWORD}
ENV MONGO_INITDB_DATABASE=${MONGO_INITDB_DATABASE}
ENV MONGO_HOSTNAME=${MONGO_HOSTNAME}

COPY ./init/ /docker-entrypoint-initdb.d/

# Implement SSL cert
RUN mkdir -p /etc/mongo/ssl && \
    sh -c "openssl req -new -x509 -days 365 -nodes \
        -out /etc/mongo/ssl/mongo.crt \
        -keyout /etc/mongo/ssl/mongo.key \
        -subj '/CN=$MONGO_HOSTNAME'" && \
    cat /etc/mongo/ssl/mongo.key >> /etc/mongo/ssl/mongo.crt && \
    chmod 600 /etc/mongo/ssl/*

EXPOSE 27017

CMD ["mongod", "--auth", "--tlsMode", "requireTLS", "--tlsCertificateKeyFile", "/etc/mongo/ssl/mongo.crt"]