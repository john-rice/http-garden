FROM http-garden-soil:latest

ARG APP_REPO
RUN git clone --recurse-submodules "$APP_REPO"

ARG APP_VERSION
RUN cd /app/mongoose && git pull && git checkout "$APP_VERSION" && mkdir /app/mongoose/examples/echo

ARG SERVER_SOURCE
COPY $SERVER_SOURCE /app/mongoose/examples/echo/main.c

RUN cd /app/mongoose/examples/echo && afl-cc -fsanitize=address,undefined -I/app/mongoose main.c /app/mongoose/mongoose.c -o echo

WORKDIR /app/mongoose

CMD ["afl-showmap", "-o", "/tmp/trace", "-t", "2147483647", "--", "/app/mongoose/examples/echo/echo"]
