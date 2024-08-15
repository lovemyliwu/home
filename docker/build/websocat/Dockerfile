FROM rust:1.63.0-alpine3.15 as buildsocat

WORKDIR /tmp

# websocat build from source:
# Issue: https://github.com/vi/websocat/issues/240
# Resolved: https://github.com/vi/websocat/tree/e289e9f039169e2bc9df3c0b1aaf15a59f948aac
RUN apk add --no-cache git musl-dev pkgconfig openssl-dev

RUN git clone https://github.com/vi/websocat.git \
    && cd websocat \
    && git checkout e289e9f039169e2bc9df3c0b1aaf15a59f948aac

WORKDIR /tmp/websocat
#ENV RUSTFLAGS='-Ctarget-feature=-crt-static -Clinker=musl-gcc'
#RUN rustup target add x86_64-unknown-linux-musl
RUN cargo build --release --target=x86_64-unknown-linux-musl && strip target/x86_64-unknown-linux-musl/release/websocat

FROM scratch
WORKDIR /
# Stage 3 or later you can copy executable like below.
COPY --from=buildsocat /tmp/websocat/target/x86_64-unknown-linux-musl/release/websocat /