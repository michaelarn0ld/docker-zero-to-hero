# Docker: Zero to Hero

This project is meant to learn the basics of docker. It should be enough to 
get both our Java applications and our React applications packaged in
containers so that they can be deployed to the cloud. The following topics are
covered:

* Dockerfile
* Docker images
* Docker containers
* Docker volumes
* Docker networks
* Docker compose

By default, the Docker root is at `/var/lib/docker` \ 
Get Docker root info -> `docker info | grep Root`


## DOCKERFILE
Basic docker command to build an image from a `Dockerfile`

```docker build -t image_name:version .```

The above command works when your `Dockerfile` is named `Dockerfile`. If it has
another file name, you must specify:

```docker build -t image_name:version -f {file_name.ext} .```

Reference: [Dockerfile](https://docs.docker.com/engine/reference/builder/)

Commands
* `FROM` - image to base your container on; if you don't want a base then it is
`FROM scratch`
* `RUN` - executes shell commands during the image build process
* `COPY` - copy files from the docker host (local machine) to the image
* `ADD` - get files from the internet (downloadable links) and add to the image
* `ENV` - define environment variables
* `WORKDIR` - set the directory that other `Dockerfile` commands get excecuted with
respect to; you can alter this if you need to change directories
* `LABEL` - No functionality, just metadata about our image
* `USER` - After you add users via `RUN` commands, use this to switch to the users
you made in your image
* `ARG` - allows us to pass command line arguments during docker build; use
the --build-arg key=value tag. Key should be aligned with ARG key=default in
the `Dockerfile`
* `CMD` - command that is executed when you create an instance (container) of 
the image; the container needs a process running IN THE FOREGROUND to stay alive,
when the process terminates, the container exits immediately
* `VOLUME /path/to/source` - allows us to create an anonymous volume that copies
data from `/path/to/source` in our container filesystem to an anonymous volume on
our host machine.
----

## DOCKER CONTEXT AND IGNORE
In the command:

`docker build -t image_name:version -f {file_name.ext} .`
The `.dockerignore` file should live in the root of your context; if you want to
ignore two directories, `foo/` and `bar/` from being considered in your context,
then you `printf "%s\n" "foo/" "bar/" > .dockerignore`

* The `.` at the end provides the context for all the commands in the `Dockerfile`;
it tells docker to look in our current directory to give context to our build.
Say we have `COPY cmd.sh /scripts/cmd.sh` in our `Dockerfile` but we execute the
build command in a directory that doesn't have a file `cmd.sh`, then the build
will fail because the `.` only scans the directory (and subdirectories) from 
where the build command was executed.
----

## DOCKER IMAGES
Dangling images are those that have a <none>:<none> identifer when you execute
the command `docker images`. If you want a way to quickly remove these danglers,
then run the command:

`docker rmi $(docker images -f dangling=true -q)`

Multistage allows us to clean up our final build by using a preliminary set of
layers to perform some operations and then copy the things we need from the 
first set of layers into a final images. See `06-**/MULTI_STAGE` for an example
where we use a multistage build to use a `maven` container to build a `jar`,
then we move the `jar` to a new `openjdk` layer so that we don't have to keep
all the bulky source code and maven dependencies in our final image.
----

## DOCKER CONTAINERS
* `docker rename old_name new_name` to rename a container
* `docker run --rm -d -p 9090:80 image_name` start a container from `image_name`
and expose your machines 9090 port, which is mapped to port 80 of the container
* `free -h` execute on docker host machine to see how much resources we have.
* `grep "model name" /proc/cpuinfo | wc -l` to see how many threads
we have available
* `docker stats container_name(s)` to see the resources that a container is using;
we can use this info to limit ram and CPU usage.
* `docker run --help | grep memory` filters docker commands so we can see memory
specific tools
* `docker run --rm -d -e "myvar=1234" -e "anothervar=456" nginx:alpine` set 
env variables with `-e`
* `docker run -d --name test --memory "200mb" nginx:alpine` limit `test` to
a maximum of 200mb total memory usage.
* `docker run --help | grep cpu` filters docker commands so we can see cpu
specific tools
* `docker run -d --name test2 --memory "400mb" --cpuset-cpus 0,3 nginx:alpine`
limit `test2` to a maximum of 400mb total memory usage and use CPUs 0 and 3; we
can also provide a range of CPUs by `0-3`.  
* `docker cp source container_name:/path/to/destination/filename.ext` copy stuff
`source` from docker host to container; when container dies, the file we copied
inside of it also dies. For persistent data, put the copy at the Dockerfile level
* `docker cp container:/path/to/source/stuff.ext destination` copy stuff from
docker container to the docker host.
* `docker commit container_name final_image_name` capture state of container so
the changes are persistent and can be rebooted into a new container when you
destroy THIS container. Turns THIS container into an image.
* `docker run -d image_name [ARGS...]` the `[ARGS...]` after the image_name will
override the `CMD` from `Dockerfile` when you start up a docker container.
* `sudo du -sh /path/to/target` this command shows the total disk usage of a 
specific target directory or file.
* `sudo systemctl stop docker` stop the docker service
* `sudo systemctl start docker` start the docker service
----

## DOCKER VOLUMES
*`docker run ... -v /mnt/mysql:/var/lib/mysql ...` maps a directory from your docker
host to a directory in the container. Store all the information from the container
`/var/lib/mysql` in the `/mnt/mysql` directory on your machine. This command 
works in both directions; you can do it to start a fresh container or you can
do it to boot data from `/mnt/mysql` into a new container.
* `docker volume create volume_name` creates a folder under the docker root 
directory (in the volumes subdirectory) with the name that you specified.
* `docker run ... -v volume_name:/var/lib/mysql` maps the `volume_name` to the
container directory (all data inside) `/var/lib/mysql ...`
* `docker rm -v container_name` dangerous command because it removes the container
as well as the volume associated with the container; this does not work for bind
volumes or for named volumes, the `-v` flag is only available for anonymous vols.
Bind volumes and named volumes are persistent and would need need manually be 
removed by navigating and executing on the host filesystem.
* `docker volume rm $(docker volume ls -f dangling=true -q)` remove dangling
volumes that are found.
* Docker volumes can be shared; please reference the instructions and files in
`/08-shared-volumes/` to see an example of this.

## DOCKER NETWORKS

## DOCKER COMPOSE


## References
[Official Docker Docs](https://docs.docker.com/)
