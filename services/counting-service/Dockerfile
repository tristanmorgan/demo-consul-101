FROM golang:alpine AS build

RUN apk add --no-cache curl git dropbear-ssh alpine-sdk

WORKDIR $GOPATH/src/github.com/hashicorp/demo-consul-101/services/counting-service

COPY . .

RUN go mod tidy

RUN CGO_ENABLED="0" GOARCH=${arch} go build -trimpath -ldflags="-s -w" -a -o /counting-service

FROM scratch

LABEL maintainer="Tristan Morgan <tristan.morgan@hashicorp.com>"
LABEL Description="Counting-service in a single binary Container"

WORKDIR /

COPY --from=build /counting-service /
ENTRYPOINT ["/counting-service"]
