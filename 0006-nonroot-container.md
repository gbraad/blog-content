---
Title: non-root user inside a Docker container
Date: 2016-9-8
Category: Docker
Tags: docker, fedora
---

One of the things that you notice when using Docker, is that all commands you run from the `Dockerfile` with `RUN` or `CMD` are performed as the `root` user. This is not only a bad security practice for running internet facing services, it might even prevent certain applications from working properly. So, how do you run commands as a non-root user? For people using Red Hat-based systems, such as Fedora or CentOS I will explain how you can do this.


## Let's begin
Let's say you have a `Dockerfile` using Fedora as a base:

```
FROM fedora:24
```


## Create user
Before we can use a different user, we need to create one:

```
RUN adduser user
```


## Run as user
Now you can run commands as this user by doing:

```
RUN su - user -c "touch me"
```

The container start process can be changed to:

```
CMD ["su", "-", "user", "-c", "/bin/bash"]
```

This way, a `bash` shell will open as the user.


## Allow `sudo` usage
Since you are running a normal user, it might be handy to install the following package inside the container:

```
RUN dnf install -y sudo
```

This will allow you to use `sudo` to perform actions as the root user:

```
RUN echo "user ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/user && \
    chmod 0440 /etc/sudoers.d/user
```


## Final `Dockerfile`
To combine all of this, your `Dockerfile` would look as follows:

`Dockerfile`
```
FROM fedora:24

RUN dnf install -y sudo && \
    adduser user && \
    echo "user ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/user && \
    chmod 0440 /etc/sudoers.d/user

RUN su - user -c "touch mine"

CMD ["su", "-", "user", "-c", "/bin/bash"]
```


## Result
Building this container is simple:

```
$ docker build -t user .
Step 1 : FROM fedora:24
 ---> f9873d530588
Step 2 : RUN dnf install -y sudo &&     adduser user &&     echo "user ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/user &&     chmod 0440 /etc/sudoers.d/user
 ---> Using cache
 ---> 05c3f3c7e9fc
Step 3 : RUN su - user -c "touch me"
 ---> Running in 584ab0ad025b
 ---> 66ea558b9855
Removing intermediate container 584ab0ad025b
Step 4 : CMD su - user -c /bin/bash
 ---> Running in 780e0ccd5f61
 ---> b19a10012b28
Removing intermediate container 780e0ccd5f61
Successfully built b19a10012b28
```

Running it will produce the following result:

```
$ docker run -it --rm user
[user@48add6313ab6 ~]$ ls -al
total 20
drwx------ 2 user user 4096 Sep  8 08:51 .
drwxr-xr-x 3 root root 4096 Sep  8 08:46 ..
-rw-r--r-- 4 user user   18 May 17 14:22 .bash_logout
-rw-r--r-- 4 user user  193 May 17 14:22 .bash_profile
-rw-r--r-- 4 user user  231 May 17 14:22 .bashrc
-rw-rw-r-- 1 user user    0 Sep  8 08:51 me
[user@48add6313ab6 ~]$ pwd
/home/user
[user@48add6313ab6 ~]$ 
```

I hope this has been of some help to you. You can [follow me](https://twitter.com/gbraad) on Twitter or leave a comment below.
