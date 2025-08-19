# build stage
FROM mcr.microsoft.com/oss/go/microsoft/golang:latest AS build
RUN apt-get update \
 && apt-get install git ca-certificates \
 && apt-get autoremove \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/* 
ADD . /src
WORKDIR /src
ENV CGO_ENABLED=0
RUN go build \
    -ldflags "-X github.com/jpillora/chisel/share.BuildVersion=$(git describe --abbrev=0 --tags)" \
    -o /tmp/bin
# run stage
FROM scratch
LABEL maintainer="dev@jpillora.com"
COPY --from=build /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
WORKDIR /app
COPY --from=build /tmp/bin /app/bin
ENTRYPOINT ["/app/bin"]
