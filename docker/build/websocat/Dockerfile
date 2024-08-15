FROM rust:1.63.0-alpine3.15 as build

# env prepare
RUN apk add --no-cache git musl-dev pkgconfig openssl-dev

# code prepare
WORKDIR /tmp
RUN git clone https://github.com/vi/websocat.git \
    && cd websocat \
    && git checkout b93a7f61816fc399fa9fc73e91bcd1e927c1bf00

# build target
WORKDIR /tmp/websocat
RUN cargo build --release --target=x86_64-unknown-linux-musl --features=workaround1,seqpacket,prometheus_peer,prometheus/process,crypto_peer

# save release file
FROM scratch
WORKDIR /
COPY --from=build /tmp/websocat/target/x86_64-unknown-linux-musl/release/websocat /