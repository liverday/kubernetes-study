FROM golang:1.15
WORKDIR /app

COPY . .

RUN go build -o server .

ENTRYPOINT ["/app/server"]