FROM golang:alpine as builder

RUN apk update && apk add --no-cache git

WORKDIR $GOPATH/src/go_microservice/consignment-service

COPY . .

# Using go get.
RUN go get -u github.com/golang/dep/cmd/dep
RUN dep init && dep ensure
# Build the binary.
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -a -installsuffix cgo -ldflags '-extldflags "-static"' -o main .
#RUN go get -u github.com/golang/dep/cmd/dep
#RUN curl https://raw.githubusercontent.com/golang/dep/master/install.sh | sh
#RUN dep init && dep ensure
#RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo .
#RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -ldflags '-extldflags "-static"' -o main .

############################
# STEP 2 build a small image
############################
FROM scratch

RUN apk --no-cache add ca-certificates

RUN mkdir /app
WORKDIR /app
COPY --from=builder $GOPATH/src/go_microservice/consignment-service .

# Use an unprivileged user.
USER appuser

CMD ["./consignment-service"]