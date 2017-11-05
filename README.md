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
* http://localhost:8080/question-rest/apidocs/

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

