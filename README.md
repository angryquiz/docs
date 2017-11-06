# docs
Documentations and notes


### Docker install
* https://store.docker.com/editions/community/docker-ce-desktop-mac

### Docker Redis install
* https://hub.docker.com/_/redis/
* docker pull redis
* docker run -p 6379:6379 --name some-redis -d redis
* test telnet localhost 6379

### question-rest testing
* mvn jetty:run -DredisServer=localhost -DredisPort=6379
* http://localhost:8585/question-rest/apidocs/

### question-web testing
* mvn jetty:run -DquestionRestEndpoint="http://localhost:8080/question-rest"
* http://localhost:9999/question-web/

## Create maven repo
* Resources - http://kwebble.com/blog/2014/02/19/use-github-to-host-your-own-maven-repo
* Clone - git clone https://github.com/angryquiz/maven-repo
* Create releases:
```
mvn install:install-file -DgroupId=com.question.rest -DartifactId=question-rest -Dversion=1.0.0 -Dpackaging=war -Dfile=/Users/angryquiz/.m2/repository/com/question/rest/question-rest/1.0.0/question-rest-1.0.0.war -DlocalRepositoryPath=/Users/angryquiz/workspaces/angryquiz-stuff/maven-repo

mvn install:install-file -DgroupId=com.question.web -DartifactId=question-web -Dversion=1.0.0 -Dpackaging=war -Dfile=/Users/angryquiz/.m2/repository/com/question/web/question-web/1.0.0/question-web-1.0.0.war -DlocalRepositoryPath=/Users/angryquiz/workspaces/angryquiz-stuff/maven-repo

```
* git push origin master (twice to ask the password) 
* Control-Space -> KeyChain Access (to change/check Mac password for git)
* change login name / password
```
git config --global --list
git config --global user.name
git config --global user.name "angryquiz"
git config --global user.email
git config --global user.email "angryquiz77@gmail.com"
```
### Docker ElasticSearch / Kibana install
* https://hub.docker.com/r/nshou/elasticsearch-kibana/
* docker pull nshou/elasticsearch-kibana
* docker run -d -p 9200:9200 -p 5601:5601 nshou/elasticsearch-kibana
* Then you can connect to Elasticsearch by localhost:9200
* Kibana front-end by localhost:5601
* http://localhost:9200/
* http://localhost:5601/
* Sample request at http://localhost:8080/question-bank/apidocs/#!/questions/uploadJSONQuestion
* Create index in elastic search to avoid the error below
```

http://localhost:8080/question-bank/rest/question-bank/search/aaa

{
  "error": {
    "root_cause": [
      {
        "type": "index_not_found_exception",
        "reason": "no such index",
        "resource.type": "index_or_alias",
        "resource.id": "questionbank",
        "index_uuid": "_na_",
        "index": "questionbank"
      }
    ],
    "type": "index_not_found_exception",
    "reason": "no such index",
    "resource.type": "index_or_alias",
    "resource.id": "questionbank",
    "index_uuid": "_na_",
    "index": "questionbank"
  },
  "status": 404
}
```
* https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-create-index.html
* http://localhost:5601/app/kibana#/dev_tools/console?load_from=https://www.elastic.co/guide/en/elasticsearch/reference/current/snippets/indices-create-index/3.json
* Example on Kibana console - http://localhost:5601/app/kibana#/dev_tools/console?_g=()
```
PUT questionbank
{
    "settings" : {
        "number_of_shards" : 3,
        "number_of_replicas" : 2
    }
}

Reponse:

{
  "acknowledged": true,
  "shards_acknowledged": true,
  "index": "questionbank"
}
```
* Another test http://localhost:8080/question-bank/rest/question-bank/search/aaa
```
{
  "took": 9,
  "timed_out": false,
  "_shards": {
    "total": 3,
    "successful": 3,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": 0,
    "max_score": null,
    "hits": []
  }
}
```
### Follow instructions for manually configuring elasticsearch 

* https://github.com/angryquiz/docs/blob/master/ElasticSearchKibanaSetup.md

### question-bank testing
* mvn jetty:run -D elasticSearchUrl="http://localhost:9200"
* On Browser - http://localhost:8080/question-bank/apidocs/#!/questions/uploadJSONQuestion

### Test / build static Web

* Static page
* npm install
* npm start
* http://192.168.0.17:8100

### Build Docker file 

* https://github.com/cmoro-deusto/docker-tomcat8
* https://hub.docker.com/r/angryquiz77/oracle-tomcat/

### Docker Memory Issue

* https://stackoverflow.com/questions/44533319/how-to-assign-more-memory-to-docker-container
* Docker whale preferences - Adjust memory from 2GB to 4GB
* docker stats (to view memory usage)

### Re-import data if ElasticSearch / Kibana has been rebuilt

* import mapping and data

```
docker run --net=host --rm -ti -v /Users/aq/workspaces/angryquiz-stuff/docs:/tmp taskrabbit/elasticsearch-dump \
  --input=/tmp/question-bank-es-mapping.json \
  --output=http://localhost:9200/questionbank \
  --type=mapping

docker run --net=host --rm -ti -v /Users/aq/workspaces/angryquiz-stuff/docs:/tmp taskrabbit/elasticsearch-dump \
  --input=/tmp/question-bank-es-data.json \
  --output=http://localhost:9200/questionbank \
  --type=data
```



