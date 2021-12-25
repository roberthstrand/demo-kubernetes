FROM golang:1.17.3-alpine3.14 AS build
WORKDIR /app

COPY go.mod .
COPY go.sum .

RUN go mod download

COPY *.go .

RUN go build -o webapp

FROM alpine:3.14
LABEL maintainer="Roberth Strand <me@robstr.dev>"
WORKDIR /app

RUN adduser -D nonroot

COPY --chown=nonroot:nonroot --from=build /app/webapp /app/webapp

ENV SERVER_PORT="80"
EXPOSE 80
USER nonroot:nonroot

CMD [ "/app/webapp" ]