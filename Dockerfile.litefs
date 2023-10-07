FROM golang:latest AS build-backend

RUN mkdir /app
ADD . /app
WORKDIR /app

RUN CGO_ENABLED=0 GOOS=linux go build -o pocketbase .

FROM alpine:latest AS production
COPY --from=build-backend /app .
EXPOSE 8081

# Install required packages
RUN apk add --no-cache \
    ca-certificates \
    fuse3 \
    sqlite

COPY --from=flyio/litefs:0.5 /usr/local/bin/litefs /usr/local/bin/litefs

ENTRYPOINT litefs mount
