FROM golang:1.17.3-alpine3.15 AS build
WORKDIR /app

COPY go.mod .
COPY go.sum .

RUN go mod download

COPY *.go .

RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o webapp

FROM alpine:3.15
LABEL maintainer="Roberth Strand <me@robstr.dev>"
RUN apk --no-cache add ca-certificates \
&& mkdir /home/nonroot && adduser -S nonroot

USER nonroot
WORKDIR /home/nonroot

COPY --from=build /app/webapp ./

ENV SERVER_PORT="80"
EXPOSE 80

ENTRYPOINT [ "/bin/sh", "-c", "/home/nonroot/webapp"]