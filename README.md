# evento_deportivo
1. docker network create mongo-replica-net (Crear network en docker para comunicar los contenedores)

2. docker run -d -p 27017:27017 --name mongo1 --net mongo-replica-net mongo --replSet rs0 (crear bases de datos1) 

docker run -d -p 27018:27017 --name mongo2 --net mongo-replica-net mongo --replSet rs0 (crear bases de datos2) 

docker run -d -p 27019:27017 --name mongo3 --net mongo-replica-net mongo --replSet rs0 (crear bases de datos3) 

3. mongosh "localhost:27017" (conexión bd)

4. rs.initiate({
    _id: "rs0",
    members: [
        { _id: 0, host: "mongo1:27017" },
        { _id: 1, host: "mongo2:27017" },
        { _id: 2, host: "mongo3:27017" }
    ]
}); (Iniciar el grupo de nodos) 
