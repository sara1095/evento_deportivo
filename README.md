# evento_deportivo
# Réplicas
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

#Particionamiento

-Se crean los contenedores
docker-compose up -d

docker-compose down

-Configuración
-Entrar a la replica1
mongosh localhost:27017

-Conjunto de replicas
rs.initiate({
  _id: "configReplSet",
  members: [
    { _id: 0, host: "config1:27017" },
    { _id: 1, host: "config2:27018" },
    { _id: 2, host: "config3:27019" }
  ]
});

-Se entra al shard1
rs.initiate({
  _id: "shard1ReplSet",
  members: [{ _id: 0, host: "shard1:27020" }]
});


-Se entra al shard2
rs.initiate({
  _id: "shard2ReplSet",
  members: [{ _id: 0, host: "shard2:27021" }]
});

-Se entra al orquestador
docker exec -it mongos mongosh --port 27022

-Se añaden los shards
sh.addShard("shard1ReplSet/shard1:27020");
sh.addShard("shard2ReplSet/shard2:27021");

-se habilita el particionamiento
sh.enableSharding("evento_deportivo");

particiones
sh.status()

replicas
 rs.conf()
