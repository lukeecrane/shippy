FROM golang:alpine as builder

RUN apk update && apk add --no-cache git

WORKDIR $GOPATH/src/go_microservice/consignment-service

COPY . .

# Using go get.
RUN go mod download
# Build the binary.
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -a -installsuffix cgo -ldflags '-extldflags "-static"' -o main .


############################
# STEP 2 build a small image
############################
FROM alpine:latest

RUN apk --no-cache add ca-certificates

# Same as before, create a directory for our app.
RUN mkdir /app
WORKDIR /app

# Here, instead of copying the binary from our host machine,
# we pull the binary from the container named `builder`, within
# this build context. This reaches into our previous image, finds
# the binary we built, and pulls it into this container. Amazing!
COPY --from=builder $GOPATH/src/go_microservice/consignment-service .

# Use an unprivileged user.
USER appuser

CMD ["./service-consignment"]