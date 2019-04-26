First of all create empty project folder. For example let it be in home directory:
```sh
$ cd ~ && mkdir app
```
Now we can clone the repository:
```sh
$ git clone https://github.com/mp091689/dockerized_environment.git dockerized
```
Go to the cloned directory:
```sh
$ cd dockerized
```
and create config file for docker-compose. There is `.env.example` file with default configurations in cloned directory. Let's use it:
```sh
$ cp .env.example .env
```
There are several variables in .env

> NGINX_HOST - the host to access to the project 
> NGINX_PORT - port to access to the project
> NGINX_PUBLIC - the public directory name where index.php file is placed. Default value `public`, for yii2 project value should be changed to `web`
> DB_NAME - data base name
> APP_PATH - the path to project on the local machine

Do not forget to configure hosts file. Add `127.0.0.1       app.local` in the end of /etc/hosts file.
If `NGINX_HOST` variable is change `hosts` should be change as well.

Finally run containers with command:
```sh
$ docker-compose up
```
Check the running containers:
```sh
$ docker ps
```
There are should be three containers stared.

To connect to the php container

as non root user:
```sh
$ docker exec -it dockerized_php_1 su dev
```
as root user:
```sh
$ docker exec -it dockerized_php_1 su dev
```
To open your project in the browser visit [http://app.local:8080](http://app.local:8080).