# Multistage build for lightweight Dropbear SSH server (2022.83)

# ----- Build stage -----
FROM alpine:3.20 AS builder

RUN apk add --no-cache build-base wget zlib-dev zlib-static
ENV DROPBEAR_VERSION=2022.83

WORKDIR /build
RUN wget https://matt.ucc.asn.au/dropbear/releases/dropbear-${DROPBEAR_VERSION}.tar.bz2 \
    && tar xjf dropbear-${DROPBEAR_VERSION}.tar.bz2 \
    && cd dropbear-${DROPBEAR_VERSION} \
    && ./configure --prefix=/usr --enable-static --disable-lastlog --disable-wtmp --disable-utmpx \
    && make PROGRAMS="dropbear dropbearkey dropbearconvert scp" \
    && make install DESTDIR=/build/install

# Generate host keys in build stage (optional but avoids runtime deps)
RUN mkdir -p /build/install/etc/dropbear \
    && /build/install/usr/bin/dropbearkey -t rsa -f /build/install/etc/dropbear/dropbear_rsa_host_key \
    && /build/install/usr/bin/dropbearkey -t ecdsa -f /build/install/etc/dropbear/dropbear_ecdsa_host_key \
    && /build/install/usr/bin/dropbearkey -t ed25519 -f /build/install/etc/dropbear/dropbear_ed25519_host_key

# ----- Final minimal runtime stage -----
FROM alpine:3.20

# Only required runtime package for scp
RUN apk add --no-cache zlib

# Copy only needed binaries + keys
COPY --from=builder /build/install/ /

# Create test user (note: password in image is insecure; for testing only)
RUN adduser -D testuser \
    && echo "testuser:testpass" | chpasswd

# Expose SSH port
EXPOSE 22

# Run Dropbear in foreground, log to stderr, disable root password login
CMD ["/usr/sbin/dropbear", "-F", "-E", "-p", "22"]
