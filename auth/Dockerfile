FROM golang:1.24-alpine as builder

WORKDIR /app

COPY . .

RUN ls

RUN go mod vendor

RUN go build -o main

FROM alpine:latest

WORKDIR /app

COPY --from=builder /app/main .

EXPOSE 9090

CMD ["./main"]
