FROM golang:1.17.3-alpine3.14 AS build
WORKDIR /

COPY go.mod .
COPY go.sum .

RUN go mod download

COPY *.go .

RUN go build -o webapp

FROM alpine:3.14
LABEL maintainer="Roberth Strand <me@robstr.dev>"
WORKDIR /

RUN adduser -D nonroot

COPY --chown=nonroot:nonroot --from=build /webapp /webapp

ENV SERVER_PORT="80"
EXPOSE 80
USER nonroot:nonroot

RUN apk add dumb-init
ENTRYPOINT ["/usr/bin/dumb-init", "--"]

CMD ["./webapp"]