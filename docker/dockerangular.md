# Docker

Comandi dockerizzare app angular

```
FROM node:13.6.0 as node
WORKDIR /app
COPY . .
RUN npm install -g @angular/cli@8.3.22
RUN npm install
RUN ng build

FROM nginx:alpine
COPY ./nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=node /app/dist/ippica /usr/share/nginx/html
```

Docker file che prende la versione di node indicata, copia tutto il contentuto del progetto nella cartella app meno i file indicati nel file dockerignore ed esegue i comandi elencati.
Dopo la build prende un docjer con nginx, si copia il file con la configurazione del webserver (non obbligatoria) e ininde copia tutti i file buildati nel container con node e li copia nella cartella esposta da nginx.

```
docker build -t ippicastagingacr.azurecr.io/fe-web-mst-dk-it:v1.6.7-rc.0 -f Dockerfile.prod .
docker build -t ippicaacr.azurecr.io/fe-web-mst-dk-int:v1.0.0 -f Dockerfile .
docker build -t ippicastagingacr.azurecr.io/staging-container-mst:latest .
```

Build del progetto leggendo il dockerfile e creando un tag con il nome indicato

```
docker push ippicastagingacr.azurecr.io/staging-container-mst:latest
docker push ippicaacr.azurecr.io/fe-web-mst-dk-it:v1.6.8-rc.0
docker push ippicaacr.azurecr.io/fe-web-mst-dk-int:v1.0.0
```

Push del tag creato nel container registry

```
docker system prune -a --volumes
```

rimuove tutti i conainers stoppati, tutte le reti non in uso, tutte le immagini appese e le build in cache

```
docker rmi -f $(docker images -a -q)
```

rimuove tutte le immagini

````
docker run --name ippica -d -p 8080:80 ippicastagingacr.azurecr.io/staging-container-mst:latest

docker image ls

docker container ls
```
````

docker container cp 092db30ad291:/tmp/ng-4HJzOE/angular-errors.log

az acr login --name IppicaACR
docker build -t ippicaacr.azurecr.io/fe-web-mst-dk-it:v1.7.3-rc.0 -f Dockerfile.prod .
docker push ippicaacr.azurecr.io/fe-web-mst-dk-it:v1.7.3-rc.0

docker build -f Dockerfile.debug -t ippicastagingacr.azurecr.io/staging-container-mst:v1.6.7 .
docker push ippicastagingacr.azurecr.io/staging-container-mst:v1.6.7
