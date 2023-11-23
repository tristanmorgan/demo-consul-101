FROM golang:alpine AS build

RUN apk add --no-cache curl git alpine-sdk

RUN go install github.com/GeertJohan/go.rice/rice@latest

WORKDIR $GOPATH/src/github.com/hashicorp/demo-consul-101/services/dashboard-service

COPY . .

RUN go mod tidy

RUN CGO_ENABLED="0" GOARCH=${arch} go build -trimpath -ldflags="-s -w" -a -o /dashboard-service
RUN rice append --exec /dashboard-service

FROM scratch

LABEL maintainer="Tristan Morgan <tristan.morgan@hashicorp.com>"
LABEL Description="dashboard-service in a single binary Container"

WORKDIR /

COPY --from=build /dashboard-service /
ENTRYPOINT ["/dashboard-service"]
