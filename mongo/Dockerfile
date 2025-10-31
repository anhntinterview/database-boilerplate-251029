FROM mongo:7.0

ENV MONGO_INITDB_ROOT_USERNAME=${MONGO_INITDB_ROOT_USERNAME}
ENV MONGO_INITDB_ROOT_PASSWORD=${MONGO_INITDB_ROOT_PASSWORD}
ENV MONGO_INITDB_DATABASE=${MONGO_INITDB_DATABASE}
ENV MONGO_HOSTNAME=${MONGO_HOSTNAME}

COPY ./init/ /docker-entrypoint-initdb.d/

# Create SSL folder
RUN mkdir -p /etc/mongo/ssl && chmod 700 /etc/mongo/ssl

# --- Create self-signed CA ---
RUN openssl genrsa -out /etc/mongo/ssl/ca.key 4096 && \
    openssl req -x509 -new -nodes -key /etc/mongo/ssl/ca.key \
        -sha256 -days 3650 -out /etc/mongo/ssl/ca.crt \
        -subj "/CN=MongoRootCA"

# --- Create key & certificate for MongoDB, sign by CA ---
RUN openssl genrsa -out /etc/mongo/ssl/mongo.key 4096 && \
    openssl req -new -key /etc/mongo/ssl/mongo.key -out /etc/mongo/ssl/mongo.csr \
        -subj "/CN=${MONGO_HOSTNAME}" && \
    openssl x509 -req -in /etc/mongo/ssl/mongo.csr -CA /etc/mongo/ssl/ca.crt \
        -CAkey /etc/mongo/ssl/ca.key -CAcreateserial -out /etc/mongo/ssl/mongo.crt \
        -days 365 -sha256 && \
    cat /etc/mongo/ssl/mongo.key >> /etc/mongo/ssl/mongo.crt && \
    chmod 600 /etc/mongo/ssl/*

EXPOSE 27017

# Start MongoDB with TLS
CMD ["mongod", "--auth", "--tlsMode", "requireTLS", \
     "--tlsCertificateKeyFile", "/etc/mongo/ssl/mongo.crt", \
     "--tlsCAFile", "/etc/mongo/ssl/ca.crt"]