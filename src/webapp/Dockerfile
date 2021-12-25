FROM golang:1.17.3-alpine3.14 AS build
WORKDIR /app

COPY go.mod .
COPY go.sum .

RUN go mod download

COPY *.go .

RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o webapp

FROM alpine:3.14
LABEL maintainer="Roberth Strand <me@robstr.dev>"
RUN apk --no-cache add ca-certificates \
&& addgroup -S nonroot \
&& adduser -S nonroot -G nonroot

WORKDIR /home/nonroot

USER nonroot
COPY --chown=nonroot:nonroot --from=build /app/webapp ./

ENV SERVER_PORT="80"
EXPOSE 80

ENTRYPOINT [ "./webapp" ]