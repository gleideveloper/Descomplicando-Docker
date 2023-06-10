# Tutorial principais comandos de Docker

## Alias 

alas dc="docker container" <br>
alias di="docker image" <br>
alias dv="docker volume" <br>
alias dcr="docker container run" <br>
<p>
Solução porta em uso: você pode usar o comando sudo lsof -i :5432 para identificar o processo que está usando a porta e, em seguida, usar o comando sudo kill <PID> para encerrar o processo pelo seu ID.
</p>

## criando bando mysql com docker

dcr -d --name mysql-lv \
-p 3306:3306 \
-e MYSQL_ROOT_PASSWORD=P@123456 \
-e MYSQL_DATABASE=loja-virtual \
-e MYSQL_USER=lv_user \
-e MYSQL_PASSWORD=P455w0rd mysql:latest

## criando bando postgreSql com docker

dcr -d \
--name psql-lv \
-p 5432:5432 \
--volumes-from dbdados \
-e POSTGRESQL_USER=lv_user \
-e POSTGRESQL_PASS=P455w0rd \
-e POSTGRESQL_DB=loja-virtual kamui/postgresql

## rodar um container

dcr -it hashContainer
dc attach hashContainer
dc exec -ti hashContainer bash ou ls -l /usr/share |grep mysql

## criando volume do type bind

mkdir /opt/giropops
dcr -ti --mount type=bind,src=/opt/giropops,dst=/giropops debian

## criando volume do type volume

dv create nomeVolume
dcr -ti --mount type=volume,src=giropops,dst=/giropops debian

## remove all stopped volume/container

dc prune
dv prune

## data in container: como era feito a criação antigamente

dc create -v /opt/giropops/:/giropops --name dbdados centos

## como era feito a associação entre container => volume.

dc create -v /data --name dbdados centos
dcr -d -p 5432:5432 --name psql1 --volumes-from dbdados -e POSTGRESQL_USER=docker -e POSTGRESQL_PASS=docker -e POSTGRESQL_DB=loja-virtual kamui/postgresql

## como é feito hoje a associação entre container => volume.

dv create dbdados
dcr -d \
--name psql1 \
--mount type=volume,src=dbdados,dst=/data \
-p 5432:5432 \
-e POSTGRESQL_USER=lv_user \
-e POSTGRESQL_PASS=P455w0rd \
-e POSTGRESQL_DB=loja-virtual kamui/postgresql

## criando um container para fazer um backup

dcr -ti \
--mount type=volume,src=dbdados,dst=/data \
--mount type=bind,src=/opt/backup,dst=/backup \
debian tar -cvf /backup/bkp-banco.tar /data

## paramentro '-P' associa o container com a porta exposta no Dockerfile

dcr -ti -P

## caso não tem a porta defina no Dockerfile usa-se:

dcr -ti -p 8080:80

## como executar o Dockerfile na pasta do arquivo e gerar um imagem

di build -t my-apache:1.0 .

##ENTRYPOINT=> É o principal processo do container
