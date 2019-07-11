# docker-compose-push

## What is this project for?
Let's first try to understand what is the requirement, what is the problem and how can we use docker-compose to solve the problem. For a project that uses Container, it is not as simple as a "Hello World" example. Since Containers allow you to compartmentalize the functionality you build, it also brings added advantage in the way you package it. For example, let's say that you are creating a simple spring boot stateless microservice application that generates PDF files. Let's assume that you want to use different layers on how to build PDF file generating capability. I know it is a weird example, but to prove my point, I will use this example. In the first layer, let's use iText, and in the second layer, we will use Aspose. Now, let's try to understand what we need to package the application. You need the base OS, JRE to run your application, Spring Boot libraries, iText libraries, your application code for the first use-case. For second use-case, we need the same list, except instead of packaging iText, we will package the Aspose in it. Now as you can see, we can build a re-usable code here.
First, we package OS and JRE, then wrap the Spring Boot, then Application Code, then finally create two branches to bundle one with iText and the next one to bundle it with Aspose. We are clear with the use-case and the requirement, and reusable design. So what is the problem? The problem is we have different layers now that we need to push to the Container Registry, in my case, Docker Hub Repository. And also, each segment will have its upgradable library. For instance, we package the initial application with JRE8. Now, since we know, JRE8 no longer supported, we need to build the application with, let's say JRE11 or JRE12. And hence, we need to repackage the upward application with the new library. So the challenge how do we build and push the images to the repository. Note: You are still responsible to create the relevant Dockerfile to package the components. I don't have a way to automate that. I want to find all the Dockerfiles, build them, and then push them to the registry. For that, I will use the docker-compose command.

> I'm not going to explain what is in the `docker-compose.yml` file, nor will I explain what is in the `Dockerfile.` It is fairly simple; you should know what is `docker compose` files and the `Dockerfile.` Now let's take a look at the command we are going to use here.

### First, docker login
```
docker login -u <user name>
```

### Second, invoke `docker-compose` to build
```
docker-compose build --compress --force-rm --no-cache --pull --parallel
```
Let's see what the options mean here

1. `--compress` - Compress the build context using gzip
2. `--force-rm` - Always remove intermediate containers
3. `--no-cache` - Do not use cache when building the image
4. `--pull` - Always attempt to pull a newer version of the image
5. `--parallel` - Build images in parallel

### Third, invoke `docker-compose` to push
```
docker-compose push --ignore-push-failures
```
Let's see what the options mean here
1. `--ignore-push-failures` - Push what it can and ignores images with push failures.

### Finally, let's logout
```
docker logout
```

### That's it, we are done
