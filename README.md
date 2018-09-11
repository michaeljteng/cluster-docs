# cluster docs

1. If you want to ssh directly to headnode (for scp-ing or laziness), add this to your `.ssh/config`:

```
Host remote.cs.ubc.ca       
    User [username]
Host submit
    User [username]
    ProxyCommand ssh -W %h:%p remote.cs.ubc.ca 22
```

## UBC GPU cluster

The gpu queues are 'desktop', 'gpu', and 'gpu-desktop'. They'll have all the stuff you'd expect - nvidia-smi, nvidia-docker. 
Get an interactive session with:
```bash
$ ssh username@remote.cs.ubc.ca
$ ssh submit
$ qsub -I -q [desktop/gpu/gpu-week]
$ [some fancy command]
```
The distinction between the queues are most notably the time limit for each - i.e. 1 hour/1 day/1 week respectively. 
Also, gpu-week grabs one of the machines with TitanX GPUs, and you can use this for long running experiments. 

If you want to run something with missing dependencies, ask Frank or Michael about adding it to the cluster. Alternatively, run things through docker:

```bash
username@gpu-machine:user-scratch$ nvidia-docker --user some-docker-user --rm -it -v $PWD:/workspace username/some-docker-image:some-tag bash
some-docker-user@7891fd18edc2:/workspace# [some fancy command]
```
Some important tips:
1. If you need to persist things, like experimental run data or training artifacts, you should mount plai-scratch or some directory in the docker image. The way the cluster machines are set up, you will need a user in the docker image that mirrors your own user on ubc network. As your own user on headnode, run `id -u`. This is the user id associated with your user. In the docker image, you will need to add a user with these same identifying information:

```
# SOME DOCKERFILE
FROM ubuntu:16.04

RUN random docker stuff
...
...

RUN useradd -u 99999 mjteng
```
Now, use docker with the created user as normal and things should work.
