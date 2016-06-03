docker-moodle
=============

A Dockerfile that installs the latest Moodle, Apache, PHP, MySQL and SSH

## Installation

```
git clone https://github.com/Sebastien-LOZANO/docker-moodle-mysql.git
cd docker-moodle-mysql
docker build -t moodle .
```

## Lancement du container Mysql sur le HOST2 :
```
docker run -d --name db -e MYSQL_ROOT_PASSWORD=root -e MYSQL_USER=admin -e MYSQL_PASSWORD=admin -e MYSQL_DATABASE=moodle mysql
```
## Lancement du container Ambassador sur le HOST2 :
```
docker run -d --link db:db --name moodle_ambassador -p 3306:3306 svendowideit/ambassador
```
## Lancement du container Ambassador sur le HOST1 :
```
docker run -d --name moodle_ambassador --expose 3306 -e DB_PORT_3306_TCP=tcp://10.203.0.84:3306 svendowideit/ambassador
```
## Voir les variables dans le container mysql_ambassador sur le HOST2 :
```
docker exec -it moodle_ambassador env
```
## Création et lancement du container moodle sur le HOST1 :
```
docker run -d -p 80:80 --name moodle --link moodle_ambassador:db -e MOODLE_DB_HOST=db -e MOODLE_DB_USER=root -e MOODLE_DB_PASSWORD=root -e MOODLE_DB_NAME=moodle moodle
```
