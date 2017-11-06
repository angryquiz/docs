
## Setting up ElasticSearch and Kibana

### Delete questionbank from console
* Using Kibana Console - http://localhost:5601/app/kibana#/management/kibana/index?_g=()
```
DELETE /questionbank
```

### Create Sample data
* Using Kibana Console - http://localhost:5601/app/kibana#/management/kibana/index?_g=()
```
PUT questionbank
{
    "settings" : {
        "number_of_shards" : 3,
        "number_of_replicas" : 2
    }
}
Response:
{
  "acknowledged": true,
  "shards_acknowledged": true,
  "index": "questionbank"
}
```
* check - http://localhost:9200/questionbank

### Importing sample data utility

* https://github.com/taskrabbit/elasticsearch-dump

### Import/Export using ElasticDump

* Site - https://github.com/taskrabbit/elasticsearch-dump
* Manual install - sudo npm install elasticdump -g
* Docker install - docker pull taskrabbit/elasticsearch-dump
* Deleting index - https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-delete-index.html
* Import mappings - localhost (remove --net=host if not importing from localhost)
```
docker run --net=host --rm -ti -v /Users/aq/workspaces/angryquiz-stuff/docs:/tmp taskrabbit/elasticsearch-dump \
  --input=/tmp/question-bank-es-mapping.json \
  --output=http://localhost:9200/questionbank \
  --type=mapping

Mon, 06 Nov 2017 00:54:30 GMT | starting dump
Mon, 06 Nov 2017 00:54:30 GMT | got 1 objects from source file (offset: 0)
Mon, 06 Nov 2017 00:54:30 GMT | sent 1 objects to destination elasticsearch, wrote 2
Mon, 06 Nov 2017 00:54:30 GMT | got 0 objects from source file (offset: 1)
Mon, 06 Nov 2017 00:54:30 GMT | Total Writes: 2
Mon, 06 Nov 2017 00:54:30 GMT | dump complete
```

* Verify on browser - Get index value 
```
GET http://localhost:9200/questionbank

{"questionbank":{"aliases":{},"mappings":{"config":{"properties":{"questionSetNum":{"type":"long"},"script":{"properties":{"inline":{"type":"text"},"lang":{"type":"text"},"params":{"properties":{"count":{"type":"long"}}}}}}},"questions":{"properties":{"date":{"type":"date"},"owner":{"type":"text"},"questionName":{"type":"text"},"questionSetNum":{"type":"long"},"questionTag":{"type":"text"},"questions":{"properties":{"answer":{"type":"text"},"correct":{"type":"boolean"},"correctAnswerCount":{"type":"long"},"explanation":{"type":"text"},"illustration":{"type":"text"},"memberAnswer":{"type":"text"},"question":{"type":"text"},"questionNumber":{"type":"long"},"questionType":{"type":"text"},"selections":{"properties":{"a":{"type":"text"},"b":{"type":"text"},"c":{"type":"text"},"d":{"type":"text"},"e":{"type":"text"},"f":{"type":"text"},"g":{"type":"text"},"h":{"type":"text"},"i":{"type":"text"},"j":{"type":"text"},"k":{"type":"text"},"l":{"type":"text"},"m":{"type":"text"},"o":{"type":"text"},"p":{"type":"text"},"q":{"type":"text"},"r":{"type":"text"},"s":{"type":"text"}}},"testerAnswer":{"type":"text"},"testerCorrect":{"type":"boolean"}}}}}},"settings":{"index":{"creation_date":"1509929354887","number_of_shards":"3","number_of_replicas":"2","uuid":"cQQfNrmDRxSVbx-uMAqG-Q","version":{"created":"5060299"},"provided_name":"questionbank"}}}}
```

* Import data (remove --net=host if not importing from localhost)

```
docker run --net=host --rm -ti -v /Users/aq/workspaces/angryquiz-stuff/docs:/tmp taskrabbit/elasticsearch-dump \
  --input=/tmp/question-bank-es-data.json \
  --output=http://localhost:9200/questionbank \
  --type=data

Mon, 06 Nov 2017 00:57:39 GMT | starting dump
Mon, 06 Nov 2017 00:57:40 GMT | got 13 objects from source file (offset: 0)
Mon, 06 Nov 2017 00:57:40 GMT | sent 13 objects to destination elasticsearch, wrote 13
Mon, 06 Nov 2017 00:57:40 GMT | got 0 objects from source file (offset: 13)
Mon, 06 Nov 2017 00:57:40 GMT | Total Writes: 13
Mon, 06 Nov 2017 00:57:40 GMT | dump complete
```


* Export mappings - localhost
```
docker run --net=host --rm -ti -v /Users/aq/workspaces/angryquiz-stuff/aq/es-dump:/tmp taskrabbit/elasticsearch-dump \
  --input=http://localhost:9200/questionbank \
  --output=/tmp/questionbank-mapping-6-15-2017-v1.json \
  --type=mapping
```

* Other notes

```

elasticdump \
  --input=https://host.es.amazonaws.com/questionbank \
  --output=/Users/aq/workspaces/aq-stuff/es-dump/questionbank-mapping-6-15-2017.json \
  --type=mapping

elasticdump \
  --input=https://host.amazonaws.com/questionbank \
  --output=/Users/aq/workspaces/aq-stuff/es-dump/questionbank-analyzer-6-15-2017.json \
  --type=analyzer

elasticdump \
  --input=https://host.es.amazonaws.com/questionbank \
  --output=/Users/aq/workspaces/aq-stuff/es-dump/questionbank-data-6-15-2017.json \
  --type=data

elasticdump \
  --input=/Users/aq/workspaces/aq-stuff/es-dump/questionbank-mapping-6-15-2017-v1.json \
  --output=https://host.es.amazonaws.com/questionbank \
  --type=mapping

elasticdump \
  --input=/Users/aq/workspaces/aq-stuff/es-dump/questionbank-data-6-15-2017-v1.json \
  --output=https://host.es.amazonaws.com/questionbank \
  --type=data

```

