---
layout: post
title:  "Building first docker container"
date:   2020-04-16 21:22:39 -0700
categories: [docker]
---

{:refdef: style="text-align: center;"}
![Docker container](/images/containers.jpg)
{: refdef}

#### Today we will do a practical. We will create a docker container for our program. In current technology scenario, we can easily utilize container technologies like Docker to make our application portable and maintainable.

### 1. Installing Docker
  First we download docker on our system. We will use the following link `https://www.docker.com/get-started` to download it. Once downloaded use terminal to run this command:
```
$ docker -v
```
It should result in something like this:
```
Docker version 19.03.8, build afacb8b
```
So now we have successfully installed docker. It is a good time to cover some very important docker commands. These commands will help us to manage our docker containers. We will keep referring back to these commands in our Docker life.
- Command to list all the docker containers on the system
```
$ docker container ls -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                      PORTS                    NAMES
4ab39b2e1f70        simplego:1.0        "./main"                 2 hours ago         Up 2 hours                  0.0.0.0:8000->8080/tcp   bb
```
- Command to stop a docker container by name
```
$ docker container stop {Name}
```
- command to stop all docker containers
```
$ docker container stop $(docker container ls -aq)
```
- command to remove a docker container by name
```
$ docker container rm {Name}
```
- command to remove all docker containers
```
$ docker container rm $(docker container ls -aq)
```

### 2. Installing Golang
Then we create a program that we want to run in our container. We can use any program that we have or we can just write small one here.

We will create a program in golang language. It is a wonderful language to keep in your armory. Even docker is built in golang. To run a golang program, we will need Go binaries in our system. This is the webpage that has link to download Go binaries, `https://golang.org/dl/`. Once downloaded use terminal to run this command:
```
$ go version
```
It should result in something like this:
```
$ go version go1.13 darwin/amd64
```
OK so we have successfully installed golang. We have done the most difficult task of this practical. Now it is time to get reward for our efforts. Let us proceed and create a golang program.

### 3. Creating program
We begin by creating a folder, let us name is simpleGo. Inside this folder we place main.go file.
```
simpleGo
  |
  |-> main.go
```

This code creates simply a server that responds to `localhost:8080/` and `localhost:8080/ping` .
```
# put in file main.go
package main

import (
	"net/http"
)

func main() {

	http.HandleFunc("/", index)
	http.HandleFunc("/ping", ping)

	http.ListenAndServe(":8080", nil)
}

func index(writer http.ResponseWriter, request *http.Request) {
	writer.WriteHeader(200)
	writer.Write([]byte("Simple Go server"))
}

func ping(writer http.ResponseWriter, request *http.Request) {
	writer.WriteHeader(200)
	writer.Write([]byte("pong"))
}
```
We can run this program and test our endpoints.
To run this program, we can open a terminal and move to the simpleGo folder (the folder that we just created) and run the following command:
```
$ go run main.go
```
Then on browser, we can test the following endpoints,
- `localhost:8080/`
- `localhost:8080/ping`

Once we get a successful response, we can move to containerize our application.

### 4. Creating Dockerfile
It is a convention, that we name the file that contains docker code as Dockerfile. Let us create a Dockerfile in our simpleGo folder.
```
simpleGo
  |
  |-> main.go
  |-> Dockerfile
```
To understand dockerfile we should understand the purpose of writing one. We want to automate the enviroment creation, copy the code in the created environemt, expose necessary port in the newly created environment and finally the command to run pur application.
We should have the following content (in YAML) in Dockerfile:
```
FROM golang:alpine

# Set necessary environmet variables needed for our image
ENV GO111MODULE=on \
    CGO_ENABLED=0 \
    GOOS=linux \
    GOARCH=amd64

# Move to working directory /build
WORKDIR /build/simpleGo

# Copy the code into the container
COPY . .

# Build the application
RUN go build main.go

# Export necessary port
EXPOSE 8080

# Command to run when starting the container
CMD ["./main"]
```
Let us go through the YAML code in our Dockerfile. The first line `FROM golang:alpine` a wow moment in our Dockerfile. Here we expect that an Alpine Linux distribution with golang installed will become available to us. And indeed it does, from DockerHub. DockerHub (`https://hub.docker.com/search?q=&type=image`) is a know site site that contains lot of images, which we can use right off the bat and we can build on top of it. We then set the golang environment variables with ENV statement.

Next we crate a working directory, in our newly created Alpine distribution. WORKDIR statement creates the sepecified folder structure and bring us into the child directory simpleGo. Once inside the folder, we copy all the files from our local simpleGo directory to Alpine simpleGo directory. We can express this simply as `COPY . .`, where each dot signify the current directory. First dot here tells us that files from current local directory (remember we are going to run our Dockerfile from inside simpleGo local directory.) Second dot tells us that copy to the current directory in out Docker workflow.

Once we have copied the necessary file, we will build the golang binaries by running command `go build main.go`. We also want to open the port to listen to outside world (outside the docker container), so that we can interact with our application. If you see our golang program you will note that the application is running at port 8080. And that is exactly the port we want to expose to the world.

Finally we want to run the binary that we created earlier. We specify this by `CMD ["./main"]`, CMD keyword is used to specify the default command that we want to execute.

Phew! that was a lot of theory. Running our docker container consist of 2 steps. The first one is to build the docker image and the next is to run the docker image.

### 5. Building Docker image
To build the docker image (and tag it), we use the following command:
```
$ docker build --tag simplego:1.0 .
```
This command will build the docker image that we have specified in our Dockerfile. If this command runs successfully, we will see something like this:
```
Successfully built c006affeb6c5
Successfully tagged simplego:1.0
```
If this command did not run successfully, then most likely the problem is due to container with same name or that the port we want to run on is already in use. In either case, use the docker command from top of this post to list the containers, stop and remove them.


### 6. Running Docker image
To run the created docker image, simplego:1.0 we use the following command:
```
$ docker run --publish 8000:8080 --detach --name sg simplego:1.0
```
The `--publish 8000:8080` part of the command, specifies that the Docker container exposed port 8080 to host port 8000. `--publish` will forward the incoming request to host port 8000 to container port 8080. `--detach` part of the command means that the docker will run in the background. `--name sg` part of the command specifies the name with which we can refer to our container in subsequent commands, in this case `sg`.

If this command did not run successfully, then most likely the problem is due to container with same name or that the port we want to run on is already in use. In either case, use the docker command from top of this post to list the containers, stop and remove them.

### 7. Final
We can now test that our container is running properly. We can do this by hitting url `localhost:8000/` and `localhost:8000/ping` ok. If we are receiving correct responses then its time to pat our back. Lets wash our hands and dance a little. See you guys in my next post.
