# Volume

## Docker

Create a container with image from alpine

```cmd
docker run -it alpine /bin/sh
```

In container, ls for list files and directories, not exist directory data, create, and in this create mydata.txt file, with touch, and edit with vi, and write and save some text

```cmd
# ls
# mkdir data
# cd data
# touch mydata.txt
# vi mydata.txt
````

Exit from cotainer, usig exit command, repeat create container and find file mydata.txt, not exist


### Volume

Create volume, using dockers

```cmd
docker volume create vol-test
```

Create container with volume creates in path /data:

```cmd
docker run -it -v vol-test:/data alpine /bin/sh
```

**For run in detach mode use "tail -f /dev/null" arg**

```cmd
docker run -d alpine tail -f /dev/null
```

In container, goto data directory, and create file mydata.txt, using vi write contents, save and check. Exit from container, in this case persist data in volume.

Create container again, using volume y verify data peristed.

```cmd
docker run -it -v vol-test:/data alpine /bin/sh
```

### Kubernetes

Create namespace **ns-demovol** for demo:

```cmd
kubectl create ns ns-demovol
```

Set namespace in kubernetes:
```cmd
kubectl config set-context --current --namespace=ns-demovol 
```

Create a statefulSet without volume, using not-pv-app.yml

```cmd
kubectl apply -f not-pv-app.yml
```

List pods with:
```cmd
kubectl get pods
```

In pod, create mydata.txt file (/data/mydata.txt)

```cmd
kubectl exec -it demovol-0 /bin/sh
```
In pod:

```cmd
# ls
# mkdir data
# cd data
# touch mydata.txt
# vi mydata.txt
# exit
```

Exec in pod again, and review file:

```cmd
# cat /data/mydata.txt
```

View and delete pod testing not persistency:

```cmd
kubectl get pods
kubectl delete pod demovol-0
```

Review file in new pod, Not exists

Finally, delete statefullset

```cmd
kubectl delete statefulSet demovol
```

### Persitent volume - Kubernetes

Create persistent volume using yaml **persistent-volume.yml**
```cmd
kubectl apply -f persistent-volume.yml
```

Claim persistent volume using yaml **claim-pv.yml**
```cmd
kubectl apply -f claim-pv.yml
```

Create statefulSet whit persistent volume, using **with-pv-app.yml** 

```cmd
kubectl apply -f with-pv-app.yml
```

Rewiew pods, exec inside and test with file mydata.txt, delete pod and check persistent data.

**storageClassName: hostpath** create a volume on each node hosts the pod, each volume per host is independent.


