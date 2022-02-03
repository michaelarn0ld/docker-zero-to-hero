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
----

## DOCKER CONTEXT AND IGNORE
In the command:

`docker build -t image_name:version -f {file_name.ext} .`
The `.dockerignore` file should live in the root of your context; if you want to
ignore two directories, `foo/` and `bar/` from being considered in your context,
then you `printf "%s\n" "foo/" "bar/" > .dockerignore`

The `.` at the end provides the context for all the commands in the `Dockerfile`;
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

## DOCKER CONTAINERS

## DOCKER VOLUMES

## DOCKER NETWORKS

## DOCKER COMPOSE


## References
[Official Docker Docs](https://docs.docker.com/)
