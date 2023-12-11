# Pinning to golang v1.20 as there appears to be a known issue that
# cAdvisor works with 1.20 and not latest. Issue linked below.
# https://github.com/google/cadvisor/pull/3384#issuecomment-1689045254
FROM golang:1.20 as builder

LABEL maintainer="brady_hill@trimble.com"

ENV VERSION "v0.47.2"

RUN apt-get update \
    && apt-get install -y git dmsetup \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN git clone --branch ${VERSION} https://github.com/google/cadvisor.git \
    /go/src/github.com/google/cadvisor

WORKDIR /go/src/github.com/google/cadvisor

RUN make build

# Image for usage
FROM debian:bookworm-slim

LABEL maintainer="brady_hill@trimble.com"

RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get upgrade -y --no-install-recommends \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

COPY --from=builder /go/src/github.com/google/cadvisor/_output/cadvisor \
     /usr/bin/cadvisor

EXPOSE  8080

ENTRYPOINT [ "/usr/bin/cadvisor", "-logtostderr" ]
