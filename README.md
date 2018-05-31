# Exemplos de utilização do Docker na Raspberry PI

## Hello World
```
docker run -it --rm arm32v6/alpine echo "Hello world"
```

## Exemplos: Standalone

### Servidor web - Nginx
```
docker run -it --rm -p 8080:80 arm32v6/nginx:1.14-alpine
```

#### Com arquivo local

Dentro de determinada pasta, crie um arquivo index.html, depois rode o comando abaixo
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Docker on Raspberry</title>
</head>
<body>
    <h1>Docker</h1>
</body>
</html>
```

```
docker run -it --rm -p 8080:80 -v $(pwd):/usr/share/nginx/html:ro arm32v6/nginx:1.14-alpine
```

### Serviços

#### Portainer
```
docker run -it --rm -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data douglaszuqueto/portainer:raspberry-1.17.1
```

### Banco de dados

#### Redis
```
docker run -it --rm --name redis -p 6379:6379 arm32v7/redis
```

**Com persistência**
```
docker run -it --rm --name redis -p 6379:6379 -v redis_data:/data arm32v7/redis
```

**Teste - Ping**
```
docker exec -it redis redis-cli ping
```

#### PostgreSQL
```
docker run -it --rm --name postgres -e POSTGRES_PASSWORD=root -p 5432:5432 arm32v6/postgres:11-alpine
```

**Com persistência**
```
docker run -it --rm --name postgres -e POSTGRES_PASSWORD=root -p 5432:5432 -v postgresql_data:/var/lib/postgresql/data arm32v6/postgres:11-alpine
```

**Teste - psql**
```
docker run -it --rm arm32v6/postgres:11-alpine psql -h 192.168.0.150 -U postgres
```

## Exemplo: docker-compose

Em um breve resumo: Docker-compose nada mais é que um 'ajudante' que auxilia em
manter as receitas do bolo de um projeto em específico, tudo isso, pois geralmente
um projeto tende a ter mais de um serviço: front-end, back-end, cache, servidor web, banco de dados e etc.

Abaixo segue uma receita baseada no primeiro exemplo que foi mencionado servidor web.

**docker-compose.yml**
```
version: "3"
services:
  app:
    image: arm32v6/nginx:1.14-alpine
    ports:
      - "8080:80"
    volumes:
      - ".:/usr/share/nginx/html:ro"
```

O exemplo tem um formato mais agradável, onde vamos colocando ingrediente a ingrediente, camada a camada, assim formando a arquitetura de um projeto. Num cenário simples apenas colocamos o webserver para rodar na porta 8080.

Lembre-se, o arquivo docker-compose.yml deve estar no mesmo diretório do **index.html** criado no tópico passado.

## Finalizando

Tudo que foi tratato acima, levo como consideração que você já tenha certo conhecimento em Docker. O meu objetivo aqui
era ir testando imagens e ao mesmo tempo documentando e compartilhando com vocês.

Assim que der, vou explorar as imagens referentes a **linguagens de programação**, as quais, em sua grande maioria são mantidas nos repositórios
citados e utilizados nesta exploração.

* Python
* PHP
* Golang
* JS(NodeJS)

## Referências

- [Docker Hub - arm32v7](https://hub.docker.com/r/arm32v7/)
- [Docker Hub - arm32v6](https://hub.docker.com/r/arm32v6/)
- [Portainer arm para RPI](https://github.com/douglaszuqueto/portainer-arm)
- [Documentação do Docker](http://docs.docker.com/)