# Docker

## Time zone mismatch with host

Solution: Mount `/etc/localtime` file from the host.

## Can not start services in order

Use a script like [wait-for-it.sh](https://github.com/vishnubob/wait-for-it) to delay the entry point until a socket server in an other container has been started.

## Containers grow like mushroom

It is often a good idea to mount temporary or cache directories as [tmpfs mounts](https://docs.docker.com/storage/#good-use-cases-for-tmpfs-mounts). Use `git diff IMAGE` to see the chages in a container's file system. Also, do not forget to bind mount the log directories!
