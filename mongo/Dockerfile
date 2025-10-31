FROM mongo:7.0

ENV MONGO_INITDB_ROOT_USERNAME=${MONGO_INITDB_ROOT_USERNAME}
ENV MONGO_INITDB_ROOT_PASSWORD=${MONGO_INITDB_ROOT_PASSWORD}
ENV MONGO_INITDB_DATABASE=${MONGO_INITDB_DATABASE}
ENV MONGO_HOSTNAME=${MONGO_HOSTNAME}

COPY ./init/ /docker-entrypoint-initdb.d/

# SSL directory
# RUN mkdir -p /etc/mongo/ssl && chmod 700 /etc/mongo/ssl

# Generate self-signed CA (v3)
# RUN openssl genrsa -out /etc/mongo/ssl/ca.key 4096 && \
#     openssl req -x509 -new -nodes -sha256 -days 3650 \
#         -key /etc/mongo/ssl/ca.key \
#         -out /etc/mongo/ssl/ca.crt \
#         -subj "/C=VN/ST=Ho Chi Minh/L=Ho Chi Minh City/O=ssre/OU=DB/CN=MongoRootCA"

# Generate MongoDB key & cert signed by CA (v3, valid CN)
# RUN openssl genrsa -out /etc/mongo/ssl/mongo.key 4096 && \
#     openssl req -new -sha256 -key /etc/mongo/ssl/mongo.key -out /etc/mongo/ssl/mongo.csr \
#         -subj "/C=VN/ST=Ho Chi Minh/L=Ho Chi Minh City/O=ssre/OU=DB/CN=${MONGO_HOSTNAME}" && \
#     openssl x509 -req -in /etc/mongo/ssl/mongo.csr -CA /etc/mongo/ssl/ca.crt \
#         -CAkey /etc/mongo/ssl/ca.key -CAcreateserial \
#         -out /etc/mongo/ssl/mongo.crt -days 365 -sha256 && \
#     cat /etc/mongo/ssl/mongo.key >> /etc/mongo/ssl/mongo.crt

# Fix permissions
# RUN chown -R mongodb:mongodb /etc/mongo/ssl && chmod 600 /etc/mongo/ssl/*

EXPOSE 27017

# Run MongoDB with TLS
# CMD ["mongod", "--auth", "--tlsMode", "requireTLS", \
#      "--tlsCertificateKeyFile=/etc/mongo/ssl/mongo.crt", \
#      "--tlsCAFile=/etc/mongo/ssl/ca.crt", \
#      "--bind_ip_all"]
