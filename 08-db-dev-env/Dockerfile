FROM postgres:alpine

ENV POSTGRES_PASSWORD password

ENV POSTGRES_DB user-service-prod

COPY init.sql /docker-entrypoint-initdb.d/
