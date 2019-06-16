---
title: (KULLANIM DIŞI) Azure Swarm kümesi Docker API'si ile yönetme
description: Kapsayıcıları Azure Container Service'teki Docker Swarm kümesi dağıtma
services: container-service
author: rgardler
manager: madhana
ms.service: container-service
ms.topic: article
ms.date: 09/13/2016
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 04cc9048271d653bd77fd7f2707c8f510ea8c29f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61456566"
---
# <a name="deprecated-container-management-with-docker-swarm"></a>(KULLANIM DIŞI) Docker Swarm ile kapsayıcı Yönetimi

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Docker Swarm, havuza alınmış Docker ana bilgisayarları grubuna kapsayıcılı iş yükleri dağıtmak için bir ortam sağlar. Docker Swarm yerel Docker API’sini kullanır. Docker Swarm kapsayıcılarında iş akışının yönetilmesi tek kapsayıcılı ana bilgisayardakiyle neredeyse aynıdır. Bu belge Docker Swarm’ın Azure Kapsayıcı Hizmeti örneğine kapsayıcılı iş yüklerini dağıtmaya ilişkin basit örnekler sağlar. Docker Swarm hakkında daha fazla ayrıntılı belgeler için bkz. [Docker.com’da Docker Swarm ](https://docs.docker.com/swarm/).

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

Bu belgedeki alıştırmalar için ön koşullar:

[Azure Container Service'te Swarm kümesi oluşturma](container-service-deployment.md)

[Azure Container Service'teki Swarm kümesine bağlanma](../container-service-connect.md)

## <a name="deploy-a-new-container"></a>Yeni bir kapsayıcı dağıtma
Docker Swarm’da yeni bir kapsayıcı oluşturmak için `docker run` komutunu kullanın (yukarıdaki önkoşullara uygun olarak SSH tünelini ana sunuculara açtığınızdan emin olun). Bu örnek `yeasy/simple-web` görüntüsünden bir kapsayıcı oluşturur:

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

![Gerçek ziyaret sonuçları](./media/container-service-docker-swarm/real-visit.jpg)  

Varsayılan olarak Yük Dengeleyicinin 80, 8080 ve 443 bağlantı noktaları açıktır. Başka bir bağlantı noktasında bağlanmak istiyorsanız Aracı Havuzu için Azure Load Balancer üzerinde ilgili bağlantı noktasını açmanız gerekir.

## <a name="deploy-multiple-containers"></a>Birden çok kapsayıcı dağıtma
Birden çok kapsayıcı başlatıldığında ‘docker çalışmasını’ birden çok kez yürüterek kapsayıcıların hangi ana bilgisayarlar üzerinde çalıştığını görmek için `docker ps` komutunu kullanabilirsiniz. Aşağıdaki örnekte üç kapsayıcı üç Swarm aracısında eşit olarak yayılır:  

```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
11be062ff602        yeasy/simple-web    "/bin/sh -c 'python i"   11 seconds ago      Up 10 seconds       10.0.0.6:83->80/tcp   swarm-agent-34A73819-2/clever_banach
1ff421554c50        yeasy/simple-web    "/bin/sh -c 'python i"   49 seconds ago      Up 48 seconds       10.0.0.4:82->80/tcp   swarm-agent-34A73819-0/stupefied_ride
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   2 minutes ago       Up 2 minutes        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

## <a name="deploy-containers-by-using-docker-compose"></a>Docker Compose kullanarak kapsayıcıları dağıtma
Birden çok kapsayıcının dağıtımını ve yapılandırmasını otomatik hale getirmek için Docker Compose’u kullanabilirsiniz. Bunu yapmak için, bir Secure Shell (SSH) tüneli oluşturulduğundan ve DOCKER_HOST değişkeninin ayarlandığından emin olun (yukarıdaki önkoşullara bakın).

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

Doğal olarak, yalnızca `compose.yml` dosyanızda tanımlanan kapsayıcıları incelemek için `docker-compose ps` kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Docker Swarm hakkında daha fazla bilgi edinme](https://docs.docker.com/swarm/)

