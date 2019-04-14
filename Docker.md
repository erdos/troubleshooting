# Docker

## Time zone mismatch with host

Solution: Mount `/etc/localtime` file from the host.

## Can not start services in order

Use a script like [wait-for-it.sh](https://github.com/vishnubob/wait-for-it) to delay the entry point until a socket server in an other container has been started.
