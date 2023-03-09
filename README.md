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

docker run -d --name logstash --restart always --network kong-net -p 5555:5555/udp -p 5044:5044 -p 9600:9600 -e "ELASTICSEARCH_URL=http://elasticsearch:9200" -v C:/development/api-management/logstash/pipeline:/usr/share/logstash/pipeline/ logstash:7.9.2

docker run -d --name kibana --restart always --network kong-net -p 5601:5601  -e "ELASTICSEARCH_URL=http://elasticsearch:9200" kibana:7.9.2
```


```
1. For logstash configuration, download pipeline.conf
2. Put pipeline.conf into some folder (example : C:/logstash/pipeline or /usr/home/user/logstash/pipeline)
3. Adjust the docker script for logstash (2nd script)

docker run -d --name logstash ... -v d:/development/api-management/logstash/pipeline:/usr/share/logstash/pipeline/ ...

4. Change the d:/development/api-management/logstash/pipeline into the location where you put pipeline.conf, so for example it become like this :

docker run -d --name logstash ... -v c:/logstash/pipeline:/usr/share/logstash/pipeline/ ...

or

docker run -d --name logstash ... -v /usr/home/user/logstash/pipeline:/usr/share/logstash/pipeline/ ...

5. Run the script to create docker
NOTE : For Windows, enable shared folder from docker settings
```


#### 1. Stop & remove containers

```
docker container stop elasticsearch logstash kibana
docker container rm elasticsearch logstash kibana
```


#### 2. Remove images

```
docker image ls : then copy paste the image id into command below
docker image rm [image-id-elasticsearch] [image-id-logstash] [image-id-kibana]
```

```
Example remove images (image ID might be different):
docker image rm 9b5c02e589c5 905fa5c1c696 78e360357d1a
```
