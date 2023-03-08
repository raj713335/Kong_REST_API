# Kong_REST_API

``` 
docker-compose up -d
docker-compose ps
docker-compose stop
```

``` 
docker-compose stop kong
docker-compose rm kong
```


## Analytics and Monitoring

### Zipkin

```
docker run -d --name zipkin --restart unless-stopped --network kong-net -p 9411:9411 openzipkin/zipkin:2
```

### Adding Elastic Stack

```
----------------------------------------------------------------------------------------
Elasticsearch 7 scripts
----------------------------------------------------------------------------------------

docker run -d --name elasticsearch --restart always --network kong-net -p 9200:9200 -p 9300:9300 -e "ES_JAVA_OPTS=-Xms1024m -Xmx1024m" -e "discovery.type=single-node" elasticsearch:7.9.2

docker run -d --name logstash --restart always --network kong-net -p 5555:5555/udp -p 5044:5044 -p 9600:9600 -e "ELASTICSEARCH_URL=http://elasticsearch:9200" -v d:/development/api-management/logstash/pipeline:/usr/share/logstash/pipeline/ logstash:7.9.2

docker run -d --name kibana --restart always --network kong-net -p 5601:5601  -e "ELASTICSEARCH_URL=http://elasticsearch:9200" kibana:7.9.2
```
