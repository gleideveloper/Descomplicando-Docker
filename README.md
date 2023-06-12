# Tutorial principais comandos de Docker

### Alias

```bash
alias dc="docker container"
alias di="docker image"
alias dv="docker volume"
alias dcr="docker container run"
```

<p>
Solução porta em uso: você pode usar o comando sudo lsof -i :5432 para identificar o processo que está usando a porta e, em seguida, usar o comando sudo kill <PID> para encerrar o processo pelo seu ID.
</p>

### Porta associada a um container

paramentro '-P' associa o container com a porta exposta no Dockerfile

`dcr -ti -P`

caso não tem a porta defina no Dockerfile usa-se:

`dcr -ti -p 8080:80`

### **Dockerfile**

```bash
FROM debianRUN apt-get update && apt-get install -y apache2ENV APACHE_LOCK_DIR="/var/lock"
ENV APACHE_PID_FILE="/var/run/apache2.pid"
ENV APACHE_RUN_USER="/www-data"
ENV APACHE_RUN_GROUP="/www-data"
ENV APACHE_LOG_DIR="/var/log/apache2"LABEL description="Webserver"
LABEL version="1.0.0"
VOLUME [ "/var/www/html" ]
EXPOSE 80
```

### Criando bando mysql com docker

```bash
dcr -d --name mysql-lv \
-p 3306:3306 \
-e MYSQL_ROOT_PASSWORD=P@123456 \
-e MYSQL_DATABASE=loja-virtual \
-e MYSQL_USER=lv_user \
-e MYSQL_PASSWORD=P455w0rd \
mysql:latest
```

### Criando bando postgreSql com docker

```bash
dcr -d --name psql-lv\
-p 5432:5432 \
--volumes-from dbdados \
-e POSTGRESQL_USER=lv_user \
-e POSTGRESQL_PASS=P455w0rd \
-e POSTGRESQL_DB=loja-virtual \
kamui/postgresql
```

### Rodar um container

```bash
dcr -it hashContainer
dc attach hashContainer
dc exec -ti hashContainer bash
dc exec -ti hashContainer ls -l /usr/share |grep mysql
```

### Criando volume do type bind

```bash
mkdir /opt/giropops
dcr -ti --mount type=bind,src=/opt/giropops,dst=/giropops debian
```

### Criando volume do type volume

```bash
dv create nomeVolume
dcr -ti --mount type=volume,src=giropops,dst=/giropops debian
```

### Remove container/volume/image

```bash
Remove all stopped container/volume/image
dc prune
di prune
dv prune
#
docker kill $(docker ps -q)
#
dc rm $(dc ls -a -q)
#
dc rm $(dc ls -a -q --filter 'ancestor=debian')
#
dc rm -f $(dc ls -a  -q --filter 'ancestor=toskeira:1.0')
#
di rmi $(di ls -q)
di rm $(di ls -q --filter "reference=myrepo")
#
dv rm $(dv ls -q)

```

### Data in container: como era feito a criação antigamente

```bash
dc create -v /opt/giropops/:/giropops --name dbdados centos
```

### Como era feito a associação entre container => volume.

```bash
dc create -v /data --name dbdados centos

dcr -d --name psql \
-p 5432:5432 \
--volumes-from dbdados \
-e POSTGRESQL_USER=docker \
-e POSTGRESQL_PASS=docker \
-e POSTGRESQL_DB=loja-virtual \
kamui/postgresql
```

### Como é feito hoje a associação entre container => volume.

```bash
dv create dbdados

dcr -d --name psql1
--mount type=volume,src=dbdados,dst=/data
-p 5432:5432
-e POSTGRESQL_USER=lv_user
-e POSTGRESQL_PASS=P455w0rd
-e POSTGRESQL_DB=loja-virtual
kamui/postgresq
```

### Criando um container para fazer um backup

```bash
dcr -ti
--mount type=volume,src=dbdados,dst=/data
--mount type=bind,src=/opt/backup,dst=/backup
debian tar -cvf /backup/bkp-banco.tar /data
```

### Como executar o Dockerfile na pasta do arquivo e gerar uma imagem

```bash
di build -t my-apache:1.0 .
```

### Entrypoint: É o principal processo do container
