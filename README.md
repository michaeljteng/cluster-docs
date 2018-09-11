# cluster docs

1. If you want to ssh directly to headnode (for scp-ing or laziness), add this to your `.ssh/config`:

```
Host remote.cs.ubc.ca       
    User username
Host submit
    User username
    ProxyCommand ssh -W %h:%p remote.cs.ubc.ca 22
```

## UBC GPU cluster

The gpu queues are 'desktop' and 'gpu'. They'll have all the stuff you'd expect - nvidia-smi, nvidia-docker. 
Get an interactive session with:
```bash
$ ssh username@remote.cs.ubc.ca
$ ssh submit
$ qsub -I -q desktop
```
