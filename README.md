# KONG API GATEWAY

## Instalación

[Documentación oficial](https://getkong.org/install/)	

## Implementacion en Docker

- Correr el comando sobre la carpeta _Docker_

	`$ docker-compose up -d`

- Test Microservicio

	`$ export KONG_HOST=127.0.0.1

	`$ http $KONG_HOST:8000`

- Test API

	`$ http httpbin.org/headers`

- Registrar API

	`$ http POST $KONG_HOST:8001/apis name=demo hosts=$KONG_HOST upstream_url=http://httpbin.org`

- Test API KONG

	`$ http $KONG_HOST:8000/headers`

- Activar plugin [JWT](https://getkong.org/plugins/jwt/)

	`$ http POST $KONG_HOST:8001/apis/demo2/plugins name=jwt config.secret_is_base64=true`

- Test JWT

	`$ http $KONG_HOST:8000/headers`

- Crear usuario

	`$ http POST $KONG_HOST:8001/consumers username=<user>`

- Crear credenciales JWT

	`$ echo '{}' | http POST $KONG_HOST:8001/consumers/<user>/jwt`

- Listar usuarios o un usuario

    `$ http $KONG_HOST:8001/consumers/`

    `$ http $KONG_HOST:8001/consumers/<user>/jwt`

- Generar Token:
 
    1. [Debugger](https://jwt.io/)

        - HEADER
        
            ```json
            {
              "alg": "HS256",
              "typ": "JWT"
            }
            ```
            
        - PAYLOAD: `"iss": "<key>"`
        
        - VERIFY SIGNATURE
        
            ```
            HMACSHA256(
             base64UrlEncode(header) + "." +
             base64UrlEncode(payload),
             <secret>
            ) [x] secret base64 encoded // check 
            ```
    2. [PyJwt](https://github.com/jpadilla/pyjwt)
    
        ```python
        import jwt
        import base64
        
        encoded = jwt.encode({'iss': '<key>'}, base64.b64decode(b'<secret>'), algorithm='HS256')
        print(encoded)
        ```

- Consumir APIS

    `$ http $KONG_HOST:8000/headers 'Authorization:Bearer <token>'`
  
- [Rate limits](https://getkong.org/plugins/rate-limiting/)

    `$ http POST $KONG_HOST:8001/apis/headers/plugins name=rate-limiting consumer_id=<consumer_id> config.minute=10`

- [StatsD](https://getkong.org/plugins/statsd/)

    `$ http POST $KONG_HOST:8001/apis/headers/plugins name=statsd consumer_id=<consumer_id>`
