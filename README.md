# Exemplos de utilização do Docker na Raspberry PI

## Hello World

## Exemplos: Standalone
```
docker run -it --rm arm32v6/alpine echo "Hello world"
```

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

**Ping**
```
docker exec -it redis redis-cli ping
```

#### PostgreSQL
```
docker run -it --rm --name postgres -e POSTGRES_PASSWORD=root -p 5432:5432 arm32v6/postgres:11-alpine
```

```
docker run -it --rm arm32v6/postgres:11-alpine psql -h 192.168.0.150 -U postgres
```

## Exemplos: docker-compose

## Referências

- [Docker Hub - arm32v7](https://hub.docker.com/r/arm32v7/)
- [Docker Hub - arm32v6](https://hub.docker.com/r/arm32v6/)
- [Portainer arm para RPI](https://github.com/douglaszuqueto/portainer-arm)