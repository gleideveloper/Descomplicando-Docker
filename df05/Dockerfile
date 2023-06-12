FROM golang AS buildando

WORKDIR /app
ADD . /app
RUN go env -w GO111MODULE=off && go build -o meugo


FROM alpine
WORKDIR /app2
COPY --from=buildando /app/meugo /app2

ENTRYPOINT [ "./meugo" ]
