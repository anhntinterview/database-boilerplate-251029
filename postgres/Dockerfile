FROM postgres:16

ENV POSTGRES_USER=admin
ENV POSTGRES_PASSWORD=secret
ENV POSTGRES_DB=pg_appdb

COPY ./init/ /docker-entrypoint-initdb.d/

EXPOSE 5432