# elk-cluster-docker-nodo01

####

vim /etc/sysctl.conf
vm.max_map_count=262144

############

CLUSTER 3 NODOS SEM TLS

docker-compose up -d && docker-compose logs -f

curl -X GET "localhost:9200/_cat/nodes?v&pretty"

docker-compose down -v

##### CLUSTER 3 NODOS COM TLS

docker-compose -f create-certs.yml run --rm create_certs

docker-compose -f elastic-docker-tls.yml up -d

docker exec es01 /bin/bash -c "bin/elasticsearch-setup-passwords \
auto --batch --url https://es01:9200"

docker-compose stop

docker-compose -f elastic-docker-tls.yml up -d

### acessar container e criar keystore para senha do usuario elastic monitoramento de cluster:
docker exec -it kib01 bash
./bin/kibana-keystore create
./bin/kibana-keystore add elasticsearch.username
./bin/kibana-keystore add elasticsearch.password
