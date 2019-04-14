FROM golang:1.12.4-alpine as gobuilder

ENV GO_SRC=/go/src/
ENV CGO_ENABLED=0
ENV GOOS=linux
ENV GOARCH=amd64

WORKDIR ${GO_SRC}

COPY main.go ${GO_SRC} 

RUN go build -o bin/app

# next images
FROM alpine:3.9

ENV GO_WORK_DIR=/go/app/

COPY --from=gobuilder /go/src/bin/app ${GO_WORK_DIR}

WORKDIR ${GO_WORK_DIR}

EXPOSE 8080

ENTRYPOINT [ "./app" ]
