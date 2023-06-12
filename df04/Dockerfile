FROM golang

WORKDIR /app
ADD . /app
RUN go env -w GO111MODULE=off && go build -o meugo
ENTRYPOINT [ "./meugo" ]
