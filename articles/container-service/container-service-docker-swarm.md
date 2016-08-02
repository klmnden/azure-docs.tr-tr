<properties
   pageTitle="Docker Swarm ile Azure Kapsayıcı Hizmeti kapsayıcı yönetimi | Microsoft Azure"
   description="Azure Kapsayıcı Hizmetinde kapsayıcıları bir Docker Swarm’a dağıtma"
   services="container-service"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Containers, Micro-services, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/13/2016"
   ms.author="nepeters"/>

# Docker Swarm ile kapsayıcı yönetimi

Docker Swarm, havuza alınmış Docker ana bilgisayarları grubuna kapsayıcılı iş yükleri dağıtmak için bir ortam sağlar. Docker Swarm yerel Docker API’sini kullanır. Docker Swarm kapsayıcılarında iş akışının yönetilmesi tek kapsayıcılı ana bilgisayardakiyle neredeyse aynıdır. Bu belge Docker Swarm’ın Azure Kapsayıcı Hizmeti örneğine kapsayıcılı iş yüklerini dağıtmaya ilişkin basit örnekler sağlar. Docker Swarm hakkında daha fazla ayrıntılı belgeler için bkz. [Docker.com’da Docker Swarm ](https://docs.docker.com/swarm/).

Bu belgedeki alıştırmalar için ön koşullar:

[Azure Kapsayıcı Hizmeti’nde bir Swarm kümesi oluşturma](container-service-deployment.md)

[Azure Kapsayıcı Hizmeti’nde bir Swarm kümesine bağlanma](container-service-connect.md)

## Yeni bir kapsayıcı dağıtma

Docker Swarm’da yeni bir kapsayıcı oluşturmak için `docker run` komutunu kullanın. Bu örnek `yeasy/simple-web` görüntüsünden bir kapsayıcı oluşturur:


```bash
user@ubuntu:~$ docker run -d -p 80:80 yeasy/simple-web

4298d397b9ab6f37e2d1978ef3c8c1537c938e98a8bf096ff00def2eab04bf72
```

Kapsayıcıyı oluşturduktan sonra, kapsayıcı hakkında bilgileri döndürmek için `docker ps` kullanın. Burada, kapsayıcıyı barındıran Swarm aracının listelendiğine dikkat edin:


```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   31 seconds ago      Up 9 seconds        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

Artık Swarm aracı yük dengeleyicinin genel DNS adı aracılığıyla bu kapsayıcıda çalışan uygulamaya erişebilirsiniz. Bu bilgileri Azure portalda bulabilirsiniz:  


![Gerçek ziyaret sonuçları](media/real-visit.jpg)  

## Birden çok kapsayıcı dağıtma

Docker Swarm kümesinde birden çok kapsayıcı başlatıldığında, kapsayıcıların hangi ana bilgisayarlar üzerinde çalıştığını görmek için `docker ps` komutunu kullanabilirsiniz. Bu örnekte, üç kapsayıcı üç Swarm aracısında eşit olarak yayılır:  


```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
11be062ff602        yeasy/simple-web    "/bin/sh -c 'python i"   11 seconds ago      Up 10 seconds       10.0.0.6:83->80/tcp   swarm-agent-34A73819-2/clever_banach
1ff421554c50        yeasy/simple-web    "/bin/sh -c 'python i"   49 seconds ago      Up 48 seconds       10.0.0.4:82->80/tcp   swarm-agent-34A73819-0/stupefied_ride
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   2 minutes ago       Up 2 minutes        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

## Docker Compose kullanarak kapsayıcıları dağıtma

Birden çok kapsayıcının dağıtımını ve yapılandırmasını otomatik hale getirmek için Docker Compose’u kullanabilirsiniz. Bunu yapmak için, bir Secure Shell (SSH) tüneli oluşturulduğundan ve DOCKER_HOST değişkeninin ayarlandığından emin olun.

Yerel sisteminizde docker-compose.yml dosyası oluşturun. Bunu yapmak için, bu [örneği](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml) kullanın.

```bash
web:
  image: adtd/web:0.1
  ports:
    - "80:80"
  links:
    - rest:rest-demo-azure.marathon.mesos
rest:
  image: adtd/rest:0.1
  ports:
    - "8080:8080"

```

Kapsayıcı dağıtımlarını başlatmak için `docker-compose up -d` komutunu çalıştırın:


```bash
user@ubuntu:~/compose$ docker-compose up -d
Pulling rest (adtd/rest:0.1)...
swarm-agent-3B7093B8-0: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-3: Pulling adtd/rest:0.1... : downloaded
Creating compose_rest_1
Pulling web (adtd/web:0.1)...
swarm-agent-3B7093B8-3: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-0: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/web:0.1... : downloaded
Creating compose_web_1
```

Son olarak, çalışan kapsayıcılar listesi döndürülür. Bu liste Docker Compose kullanılarak dağıtılan kapsayıcıları yansıtır:


```bash
user@ubuntu:~/compose$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                     NAMES
caf185d221b7        adtd/web:0.1        "apache2-foreground"   2 minutes ago       Up About a minute   10.0.0.4:80->80/tcp       swarm-agent-3B7093B8-0/compose_web_1
040efc0ea937        adtd/rest:0.1       "catalina.sh run"      3 minutes ago       Up 2 minutes        10.0.0.4:8080->8080/tcp   swarm-agent-3B7093B8-0/compose_rest_1
```

## Sonraki adımlar

[Docker Swarm hakkında daha fazla bilgi edinme](https://docs.docker.com/swarm/)



<!---HONumber=Jun16_HO2-->


