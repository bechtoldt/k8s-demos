# Secrets

## Creating Secrets

You can create a secret from files:

```bash
$ kubectl create secret generic my-secret --from-file=secret/username --from-file=secret/password

$ kubectl get secrets
```

### Creating it manually

You can also encode your secrets with base64 and put them into a json/yaml file:

```bash
$ echo "root" | base64
cm9vdAo=

$ echo "supersecret" | base64
c3VwZXJzZWNyZXQK

$ cat my-secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: Opaque
data:
  password: c3VwZXJzZWNyZXQK
  username: cm9vdAo=
```

## Consuming secrets

### Mount as file

You can consume multiple secrets by mounting it to a path using the volumes.

```
$ cat secret-file-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-file-pod
spec:
  containers:
    - name: secret-file-pod
      image: busybox:1.24
      volumeMounts:
        - name: foo
          mountPath: /etc/my-secret
          readOnly: true
  volumes:
    - name: foo
      secret:
        secretName: my-secret

$ kubectl create -f secret-file-pod.yaml
...

$ kubectl logs secret-file-pod
root
```

### Secret as environment variable

You can also consume secrets as environment variable in pods:

```
$ cat secret-env-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-env-pod
spec:
  containers:
    - name: secret-env-pod
      image: busybox:1.24
      command: [ "/bin/sh", "-c", "env" ]
      env:
        - name: SECRET_USERNAME
          valueFrom:
            secretKeyRef:
              name: my-secret
              key: username
        - name: SECRET_PASSWORD
          valueFrom:
            secretKeyRef:
              name: my-secret
              key: password
  restartPolicy: Never

$ kubectl create -f secret-env-pod.yaml
...

$ kubectl logs secret-env-pod
KUBERNETES_PORT=tcp://10.247.0.1:443
KUBERNETES_SERVICE_PORT=443
HOSTNAME=secret-env-pod
SHLVL=1
HOME=/root
SECRET_PASSWORD=supersecret

KUBERNETES_PORT_443_TCP_ADDR=10.247.0.1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
KUBERNETES_PORT_443_TCP_PORT=443
KUBERNETES_PORT_443_TCP_PROTO=tcp
SECRET_USERNAME=root

KUBERNETES_PORT_443_TCP=tcp://10.247.0.1:443
KUBERNETES_SERVICE_PORT_HTTPS=443
PWD=/
KUBERNETES_SERVICE_HOST=10.247.0.1
```
