# KONG API GATEWAY

## Instalación

[Documentación oficial](https://getkong.org/install/)	

## Implementacion en Docker

- Correr el comando sobre la carpeta "Docker"

	`$ docker-compose up -d`

- Test Microservicio

	`$ export KONG_HOST=127.0.0.1

	`$ http $KONG_HOST:8000`

- Test API

	`$ http httpbin.org/headers`

- Registrar API

	`$ http POST $KONG_HOST:8001/apis name=demo hosts=127.0.0.1 upstream_url=http://httpbin.org`

- Test API KONG

	`$ http $KONG_HOST:8000/headers`

