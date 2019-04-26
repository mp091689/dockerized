The description how to configure nginx configurations dynamically with docker-compose.

The `envsubst` tool provides us to configure nginx dynamically.

Example of using `envsubst` with docker-compose `.env` configuration. Examples provided for alpine based image.

Prepare nginx config file. Variables provided as `${VAR_NAME}`. There are two variables `${HOST}` and `${PATH}` below:
`nginx.conf`
```
server {
    server_name ${HOST};
    index index.php index.html;
    error_log  /var/log/nginx/error.log;
    access_log /var/log/nginx/access.log;
    root ${PATH};
    index index.php;
...
}
```
To be able to use defined variables in nginx configuration file use `envsubst` tool:
```sh
envsubst '$$HOST, $$PATH' < /source/file/path.example > /destination/file/path/default.conf'
```
Let's define variables in `.env` to be able to use them with docker-compose:
```
NGINX_HOST=app.local
NGINX_PATH=/app/public
```
Then provide these variables to nginx container as environments:
```yaml
environment:
   - HOST=${NGINX_HOST}
   - PATH=${NGINX_PATH}
```
Example of configured container:
`docker-coompose.yml`
```yaml
    nginx:
        image: nginx:alpine
        volumes:
            - ./nginx.conf:/etc/nginx/conf.d/nginx.conf.template
        environment:
            - HOST=${NGINX_HOST}
            - PATH=${NGINX_PATH}
        command: /bin/ash -c "envsubst '$$HOST, $$PATH' < /etc/nginx/conf.d/nginx.conf.template > /etc/nginx/conf.d/default.conf && exec nginx -g 'daemon off;'"
```
As result docker-compose will declare volume for nginx.conf, set environment variables and run command to substitute variables in nginx.conf with variables from environment.