# docs
Documentations and notes


### Docker install
* https://store.docker.com/editions/community/docker-ce-desktop-mac

### Docker Redis install
* docker pull redis
* docker run -p 6379:6379 --name some-redis -d redis
* test telnet localhost 6379

### question-rest testing
* mvn jetty:run -DredisServer=localhost -DredisPort=6379


