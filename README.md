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

```
docker run -d --name zipkin --restart unless-stopped --netword kong-net -p 9411:9411 openzipkin/zipkin:2
```
