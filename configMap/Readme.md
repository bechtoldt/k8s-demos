# ConfigMap

## Create ConfigMaps

### Create Config from files

We can create a config map from one or multiple files or from a directory (including all files inside)

```bash
$ ls configs
myprops.properties
otherprops.properties

$ cat configs/myprops.properties
awesome=true

$ cat configs/otherprops.properties
do.i.look.awesome=yes

$ kubectl create configmap my-config --from-file=configs
```

Validate the created ConfigMap:

```bash
$ kubectl describe configmaps my-config
$ kubectl get configmaps my-config -o yaml
```

### Creating Config from literal

We can also create a ConfigMap from literals.

```bash
$ kubectl create configmap lit-config --from-literal=this.is.great=true --from-literal=some.type=nil

$ kubectl get configmaps lit-config -o yaml
```

## Consume configmaps

### Env variable

A ConfigMap can be injected as env variable and can also be used as commandline args.

```
$ kubectl create -f config-env-pod.yaml
..

$ kubectl logs env-pod
KUBERNETES_SERVICE_PORT=443
KUBERNETES_PORT=tcp://10.247.0.1:443
HOSTNAME=env-pod
SHLVL=1
HOME=/root
KUBERNETES_PORT_443_TCP_ADDR=10.247.0.1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
KUBERNETES_PORT_443_TCP_PORT=443
KUBERNETES_PORT_443_TCP_PROTO=tcp
SOME_TYPE=nil
KUBERNETES_PORT_443_TCP=tcp://10.247.0.1:443
KUBERNETES_SERVICE_PORT_HTTPS=443
PWD=/
KUBERNETES_SERVICE_HOST=10.247.0.1
THIS_IS_GREAT=true
```

### Config file

A ConfigMap can be injected as a configuration file.

```
$ kubectl create -f config-file-pod.yaml
...

$ kubectl logs file-config-container
true
```
