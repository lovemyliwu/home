FROM rust:1.63.0-alpine3.15 as build
ENV RUSTFLAGS='-Clink-arg=-s -Ctarget-feature=+crt-static'

# env prepare
RUN apk add --no-cache git musl-dev pkgconfig openssl-dev openssl-libs-static
ENV OPENSSL_STATIC=1
ENV OPENSSL_LIB_DIR=/usr/lib
ENV OPENSSL_INCLUDE_DIR=/usr/include/openssl

# code prepare
WORKDIR /tmp
RUN git clone https://github.com/DistPub/websocat.git \
    && cd websocat \
    && git checkout 11959c1cf217d31087eaf5d4a88bc5394158f208

# build target
WORKDIR /tmp/websocat
RUN cargo build --release --target=x86_64-unknown-linux-musl \
    && strip target/x86_64-unknown-linux-musl/release/websocat

# save release file
FROM scratch
WORKDIR /
COPY --from=build /tmp/websocat/target/x86_64-unknown-linux-musl/release/websocat /