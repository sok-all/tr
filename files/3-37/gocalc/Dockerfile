

FROM golang:1.20-alpine 
ENV GO111MODULE=auto
RUN apk add git
#RUN echo $GOPATH  =/go 
WORKDIR /go/src/app
COPY . .
RUN go get -d -v
RUN go build -o app
FROM alpine:3.10.3
COPY --from=0 /go/src/app/app /usr/local/bin    
CMD ["app"]


