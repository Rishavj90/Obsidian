```
docker run <>
docker run name:version_tag
docker run -it <>
docker run -p new_port:old_port
docker run -v new_dir:old_dir
docker run -e a=b <>
```
- to run any image, if it doesn't exits locally, it would get it from docker-hub
- `-i` for providing input
- `-t` : pseudo terminal, attaches our terminal to docker one
- `-p` : maps one port to another
- volume mapping : `-v` stores at diff location
- setup environment variables `-e`
- 

```
docker ps
docker ps -a
```
- list all the running containers
- `-a` for listing even stopped ones
```
docker inspect <conatiner name or id>
```
- gives more info about containers
- access environment variables here

```
docker stop <name_of_container>
```
-  stops the container

```
docker rm <name_of_container>
```
- deletes the container

```
docker images
```
- lists all the images on machine

```
docker rmi <name_of_image>
```
- deletes an image

```
docker pull <name_of_image>
```
- downloads an image from dockerhub

```
docker exec <name_of_image> [cmd]
```
- to run any command in

```
docker run -d <name_of_container>
```
- runs in background, `-d` : detached

```
docker attach <name or id of container>
```
- attaches the docker container

```
docker logs <conatiner name or id>
```
- shows logs