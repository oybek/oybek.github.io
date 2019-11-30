---
layout: post
title: Travis CI with VPS
tag: [devops]
---

Programmers like to write code and don't like much the moments when they need to close
IDE or text editor and press arrow keys or even mouse in order to compile code
or deploy or whatever else (not programming).
![]()
![](https://media.giphy.com/media/13HgwGsXF0aiGY/giphy.gif)

That is why we came up with CI/CD tools. In this article I'll write about how to
setup travis CI for open github repo and forget about devops things.

# Given

* Remote virtual machine with static ip address and access over `ssh`
* Github project in publish repository
* Travis CI

# Need

When `master` branch updated:
1. Compile and Test the code
2. Assemble binary
3. Deploy and update binary at remote host

# Let's go

First we need to setup travis for our repository:
[there is an official guide](https://docs.travis-ci.com/user/tutorial/).

You'll get minimal `.travis.yml` file (it is all you need to setup CI in travis)

After this go to repo folder and execute command (DONT specify password):

```sh
ssh-keygen -t rsa -b 4096 -C '<repo>@travis-ci.org' -f ./travis_deploy_key
```

Then execute this command

```sh
travis encrypt-file travis_deploy_key --add
```

This command encrypts your private rsa key and add lines to `.travis.yml`

Also add this lines to `travis.yml`

```
addons:
   ssh_known_hosts: <deploy-host>
after_success:
 - eval "$(ssh-agent -s)"
 - chmod 600 .travis_deploy_key
 - ssh-add .travis_deploy_key
```

Why? `ssh_known_hosts` tell travis to add your host to trusted ssh hosts, travis will
not be able to ask for your confirm.

Block `after_success` is executed after build and tests success. And here we setup
ssh key in order to access remote host from travis docker container.

After that remove private key from project folder. Other files need to be commited.

Now you can add `scp` command in `after_success` block to copy compiled files to
your host and also execute commands over ssh.

