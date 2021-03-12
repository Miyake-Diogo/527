## Deploy das maquinas
```sh
vagrant up testing automation --provider virtualbox
vagrant up logging validation --provider virtualbox
```
## Parar(stop) as maquinas
```sh
vagrant halt testing automation
vagrant halt logging validation
```
## Se fizer um update no VagrantFile
1. Pare a vm usando o `vagrant halt`
2. Use o comando para subir com update `vagrant up {nome da maquina} --provision`


# Aula 01

* Yago Ésquines
* yago.almeida@4linux.com.br
* Linkedin: https://www.linkedin.com/in/yago-esquines/
* itextPad: https://www.itextpad.com/5CU3HpObSI

# Links

https://moodle-prod.4linux.com.br  
https://github.com/4linux/527  
https://gitforwindows.org/  
https://app.vagrantup.com  
http://agilemanifesto.org/  
https://martinfowler.com/  
https://sre.google/  
https://www.youtube.com/watch?v=uTEL8Ff1Zvk  
[trunk based development & feature flag](https://martinfowler.com/articles/feature-toggles.html)  
https://www.hackthebox.eu/  
TryHackMe
https://www.devsecops.org/  
https://git-secret.io/  
https://www.archerysec.com/  
https://www.sonatype.com/nexus/repository-pro  
https://www.defectdojo.org/  
https://github.com/hakluke/how-to-exit-vim  

# Ambiente Prático

* Git: apt install git / yum install git 
  - https://git-scm.com/downloads
* Virtualbox: https://www.virtualbox.org/wiki/Downloads
* Vagrant: https://www.vagrantup.com/downloads

> Pacote Ubuntu: `sudo apt install -y libarchive-tools`

```bash
git clone https://github.com/4linux/527
cd 527/
vagrant status
vagrant up testing automation --provider virtualbox
vagrant halt testing automation
```

---

# SonarQube

# Jenkins
http://192.168.77.10:8080/

# Container Sonar
```bash
docker container run --name sonarqube -d -p 9000:9000 sonarqube
docker container logs sonarqube --tail 10
docker container start sonarqube
```
http://192.168.77.20:9000

# LINK
https://www.veracode.com/  
https://www.microfocus.com/pt-br/products/static-code-analysis-sast/overview  
https://www.shiftleft.io/scan/  
https://security-code-scan.github.io/  
https://www.sonarsource.com/products/sonarlint/  
https://hub.docker.com/_/sonarqube/  
https://docs.sonarqube.org/latest/setup/install-server/  
https://www.keycloak.org/  
https://github.com/JanssenProject/home  
https://landscape.cncf.io  
https://digital.ai/periodic-table-of-devops-tools  
https://plugins.jenkins.io/  
https://github.com/4linux/Dexter-Intranet  
https://owasp.org/www-project-dependency-check/  
https://docs.sonarqube.org/latest/user-guide/rules/  
https://www.jenkins.io/blog/2012/03/13/why-does-jenkins-have-blue-balls/#:~:text=This%20is%20because%20historically%2C%20Japanese,be%20a%20shade%20of%20blue.&text=Jenkins%20has%20blue%20balls%20because,stop%20and%20blue%20means%20go  
https://www.jenkins.io/doc/book/pipeline/jenkinsfile/  
https://www.jenkins.io/doc/book/pipeline/syntax/  

# Analysis Properties
sonar.projectKey=dexter
sonar.sources=$WORKSPACE/intranet
sonar.projectVersion=$BUILD_NUMBER
## sonarcube token
4linux training	Never	February 23, 2021
0e674896b3f95fcf75c2f42d7b445f451b883818

---

# JenkinsFile
```
pipeline{
agent any
stages {
  stage('Git'){
    steps{
      git 'https://github.com/yesquines/Projetos-Faculdade-Java'
    }
  }
  stage('Sonarqube'){
    environment {
      scanner = tool 'sonar-scanner'
    }
    steps{
      withSonarQubeEnv('sonarqube') {
        sh "${scanner}/bin/sonar-scanner -Dsonar.projectKey=robin -Dsonar.sources=${WORKSPACE} -Dsonar.projectVersion=${BUILD_NUMBER} -Dsonar.java.binaries=${WORKSPACE}/* -X"
      }
    }
  }
  stage('Quality Gate') {
    steps{
      waitForQualityGate abortPipeline: true
          }
      }
  }
}
```
---
# OWASP ZAP
# CLI
```bash
zap.sh -version
zap.sh -cmd -addoninstallall
```

# LINKS
http://zaproxy.org/  
https://www.zaproxy.org/download/  
https://software.opensuse.org/download.html?project=home%3Acabelo&package=owasp-zap  
https://www.zaproxy.org/docs/desktop/cmdline/  
https://www.youtube.com/watch?v=QRYzre4bf7I  
https://owasp.org/www-project-juice-shop/  
https://pwning.owasp-juice.shop/part1/ctf.html  
https://github.com/OWASP/Top10/raw/master/2017/OWASP%20Top%2010-2017%20(en).pdf  
https://cheatsheetseries.owasp.org/  
https://www.zaproxy.org/docs/desktop/addons/selenium/  
@b0rk  

# Juice Shop
http://192.168.77.20:3000/  
```bash
docker container run -p 3000:3000 --name juice-shop -d bkimminich/juice-shop
```
# INTERFACE GRÁFICA (WINDOWS)
```bash
startx 2> /dev/null &
DISPLAY=:0 zap.sh &> /dev/null &
DISPLAY=:0 xrandr
DISPLAY=:0 xrandr -s 1024x768
kill $(ps axu | egrep 'startx|zap|X' | awk '{ if ($8 != "R+") print $2; }')

python3 -m ComplexHTTPServer
```
# SCRIPT DE BUILD
```bash
#!/bin/bash

if [ "$(curl -s localhost:8082)" ]; then
  echo "Aplicação está Ativa :D"
else
  if [ ! -d "${DEXTER_PATH}" ]; then
    git clone https://github.com/4linux/Dexter-Intranet
  fi
  docker image build ${DEXTER_PATH} -t dexter && \
  docker container run -p 8082:80 --name dexter-app -d dexter && \
  echo "[OK] Build Completo"
fi
```
--- 
# Elastick Stack
# LINKS
https://github.com/oracle/centos2ol  
https://rockylinux.org/  

Slack: https://slack.k8s.io/ (canal ﻿kubernetes-docs-pt)
Issue de tracking: https://github.com/kubernetes/website/issues/13939  
https://github.com/lmenezes/cerebro  

# COMANDOS

## Logging
dnf install -y elasticsearch kibana logstash

## Automation
dpkg -i logstash-7.8.1.deb

# /etc/docker/daemon.json
{
	"log-driver": "syslog",
	"log-opts": {
		"tag": "{{.Name}}",
		"syslog-facility": "local7"
	}
}

systemctl restart docker
docker container rm dexter-app
docker container run -d -p 8082:80 --name dexter-app dexter
http://192.168.77.30:9200/

# /etc/elasticsearch/elasticsearch.yml
network.host: 192.168.77.30
http.port: 9200
discovery.seed_hosts: ["192.168.77.30"]

# /etc/kibana/kibana.yml
server.port: 5601
server.host: "192.168.77.30"
elasticsearch.hosts: ["http://192.168.77.30:9200"]

# Erro Kibana
{"type":"log","@timestamp":"2021-02-26T01:32:57+00:00","tags":["warning","savedobjects-service"],"pid":4632,"message":"Unable to connect to Elasticsearch. Error: Request timed out"}

---
