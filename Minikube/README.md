# Minikube demo

Als Minikube nog niet gestart geeft dit fouten:

```
docker version
docker run hello-world
```

Dus nu starten we Minikube

```
minikube start
minikube status
```

Echter minikube is nog niet gelinked aan de Docker Engine van Minikube, dus dit gaat nog steeds fout:

```
docker version
```

Zo linken we de Docker client aan de  Docker Engine van Minikube:

```
minikube docker-env
minikube -p minikube docker-env | Invoke-Expression
```

Nu werken deze commando's wel:

```
docker version
docker ps
docker images
```

We zien al de native containers van Minikube/Kubernetes draaien.

We kunnen ook het Kubernetes dashboard openen door in een nieuwe terminal te typen:

```
minikube dashboard
```


Nu kunnen we bijvoorbeeld een image builden van een docker file in de src directory:

```
docker build -t code-cafe-nginx .
docker images
docker tag code-cafe-nginx:latest code-cafe-nginx:1.0
```


Echter als we nu het image runnen kunnen we er niet naar connecten:

```
docker run -d -p 80:80 code-cafe-nginx:1.0
```

Openen in de browser lukt niet [http://127.0.0.1:80/ ](http://127.0.0.1:80/ )


Om dat te kunnen moeten we naar Kubernetes deployen:

```
kubectl run code-cafe-nginx --image code-cafe-nginx:1.0 --port=80
kubectl get pods
kubectl expose pod code-cafe-nginx --type="NodePort"
kubectl get services
minikube service code-cafe-nginx --url
```

De url die we krijgen van het laatste commando kunnen we wel in de browser openen.

We kunnen ons image ook via een deployement runnen, en bijvoorbeeld scalen naar 3 replicas:

```
kubectl create deployment code-cafe-nginx-dep --image=code-cafe-nginx:1.0
kubectl expose deployment code-cafe-nginx-dep --port=80 --type="NodePort"
minikube service code-cafe-nginx-dep --url
kubectl scale deployment code-cafe-nginx-dep --replicas=3
```

Door het deployment te verwijderen worden automatisch al onze pods verwijdert:

```
kubectl delete deployment.apps/code-cafe-nginx-dep
```

Als we nu de webpagina aan willen passen moeten we opnieuw de image builden en deployen. Niet handig!

Om dat te voorkomen kunnen we mounten naar het lokale file system met een yaml file waarin we de mount paden opgeven.

```
kubectl apply -f .\mounted-pod.yaml
kubectl expose pod mounted-pod --type="NodePort"
minikube service mounted-pod --url
```

Nu kun je de code aanpassen op je lokale file systeem in C:\Users\rpaul67\development\nginx\html


