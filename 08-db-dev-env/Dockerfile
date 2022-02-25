FROM postgres:alpine

ENV POSTGRES_PASSWORD password

ENV POSTGRES_DB gym-prod

COPY init.sql /docker-entrypoint-initdb.d/
