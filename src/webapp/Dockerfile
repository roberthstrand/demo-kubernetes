FROM golang:1.17.3-alpine3.14 AS build
LABEL maintainer="Roberth Strand <me@robstr.dev>"
WORKDIR /

COPY go.mod .
COPY go.sum .

RUN go mod download

COPY *.go .

RUN go build -o webapp

FROM alpine:3.14
WORKDIR /

RUN adduser -D nonroot

COPY --chown=nonroot:nonroot --from=build /webapp /webapp

EXPOSE 80
USER nonroot:nonroot

ENTRYPOINT ["./webapp"]