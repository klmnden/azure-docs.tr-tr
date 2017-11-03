---
title: "Azure Öğreticisi - App hazırlama üzerinde Kubernertes | Microsoft Docs"
description: "AKS Öğreticisi - App hazırlama"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: "Docker, Container’lar, Mikro hizmetler, Kumernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: db8169b1c3c30efa1b684e49e1f113bee6a7fd45
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="prepare-application-for-azure-container-service-aks"></a>Uygulama Azure kapsayıcı hizmeti (AKS) için hazırlama

Bu öğreticide, sekiz, biri bölümü çok kapsayıcı uygulama Kubernetes kullanım için hazırlanır. Tamamlanan adımları içerir:  

> [!div class="checklist"]
> * GitHub’dan uygulama kaynağını kopyalama  
> * Uygulama kaynağından bir kapsayıcı görüntü oluşturma
> * Uygulamayı bir yerel Docker ortamında test etme

Tamamlandığında, aşağıdaki uygulama yerel geliştirme ortamınızda erişilebilir.

![Azure’da Kubernetes kümesinin görüntüsü](./media/container-service-tutorial-kubernetes-prepare-app/azure-vote.png)

Sonraki öğreticilerde, kapsayıcı görüntünün bir Azure kapsayıcı kayıt defterine karşıya ve bir AKS küme çalıştırın.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğreticide kapsayıcılar, kapsayıcı görüntüleri ve temel docker komutları gibi temel Docker kavramları hakkında bilgi sahibi olduğunuz varsayılmıştır. Gerekirse kapsayıcı temelleri hakkında bilgi için bkz. [Docker ile çalışmaya başlama]( https://docs.docker.com/get-started/). 

Bu öğreticiyi tamamlamak için Docker geliştirme ortamı gerekir. Docker, [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) veya [Linux](https://docs.docker.com/engine/installation/#supported-platforms)’ta Docker’ı kolayca yapılandırmanızı sağlayan paketler sağlar.

Azure bulut Kabuk her adımı tamamlamak için gereken Docker bileşenleri Bu öğretici içermez. Bu nedenle, bir tam Docker geliştirme ortamında kullanmanızı öneririz.

## <a name="get-application-code"></a>Uygulama kodunu alma

Bu öğreticide kullanılan örnek temel bir oylama uygulama uygulamasıdır. Uygulama bir ön uç web bileşeni ve bir arka uç Redis örneği oluşur. Web bileşeni özel kapsayıcı görüntüsüne paketlenmiştir. Redis örneği Docker hub'a değiştirilmemiş bir görüntüden kullanır.  

Geliştirme ortamınızı uygulamaya bir kopyasını indirmek için Git kullanın.

```console
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

Dizinleri kopyalanan dizinden çalıştığınız şekilde değiştirin.

```console
cd azure-voting-app-redis
```

Dizini içinde uygulama kaynak koduna, önceden oluşturulmuş bir Docker compose dosyası ve bir Kubernetes bildirim dosyası. Bu dosyalar öğretici kümesi kullanılır. 

## <a name="create-container-images"></a>Kapsayıcı görüntüleri oluşturma

[Docker Compose](https://docs.docker.com/compose/) yapı kapsayıcı görüntüler ve birden çok kapsayıcı uygulamalarının dağıtımını otomatik hale getirmek için kullanılabilir.

Çalıştırma `docker-compose.yml` kapsayıcı görüntü oluşturma, Redis görüntüsünü karşıdan yüklemek ve uygulamayı başlatmak için dosya.

```console
docker-compose up -d
```

Tamamlandığında kullanmak [docker görüntüleri](https://docs.docker.com/engine/reference/commandline/images/) oluşturulan görüntüleri görmek için komutu.

```console
docker images
```

Üç görüntüleri indirilebilir veya oluşturulan dikkat edin. `azure-vote-front` Görüntü uygulama içerir ve kullandığı `nginx-flask` görüntüyü temel olarak. `redis` Görüntü bir Redis örneği başlatmak için kullanılır.

```
REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

Çalıştırma [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) çalışan kapsayıcıları görmek için komutu.

```console
docker ps
```

Çıktı:

```
CONTAINER ID        IMAGE             COMMAND                  CREATED             STATUS              PORTS                           NAMES
82411933e8f9        azure-vote-front  "/usr/bin/supervisord"   57 seconds ago      Up 30 seconds       443/tcp, 0.0.0.0:8080->80/tcp   azure-vote-front
b68fed4b66b6        redis             "docker-entrypoint..."   57 seconds ago      Up 30 seconds       0.0.0.0:6379->6379/tcp          azure-vote-back
```

## <a name="test-application-locally"></a>Uygulamayı yerel olarak test etme

Çalışan uygulama görmek için http://localhost: 8080 için göz atın.

![Azure’da Kubernetes kümesinin görüntüsü](./media/container-service-tutorial-kubernetes-prepare-app/azure-vote.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Uygulama işlevselliği doğrulandı, çalışan kapsayıcılar durdurulur ve kaldırılmış. Kapsayıcı görüntüleri silmeyin. `azure-vote-front` Görüntü sonraki öğreticide Azure kapsayıcı kayıt defteri örneğine yüklenir.

Çalışan kapsayıcılar durdurmak için aşağıdaki komutu çalıştırın.

```console
docker-compose stop
```

Durdurulan kapsayıcıları ve aşağıdaki komut ile kaynakları silin.

```console
docker-compose down
```

Tamamlandığında, Azure oy uygulamayı içeren bir kapsayıcı görüntüsüne sahip.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir uygulamayı test edilmiştir ve kapsayıcı görüntüleri uygulama için oluşturulan. Aşağıdaki adımlar tamamlandı:

> [!div class="checklist"]
> * GitHub’dan uygulama kaynağını kopyalama  
> * Uygulama kaynağından bir kapsayıcı görüntüsü oluşturuldu
> * Uygulamayı yerel bir Docker ortamında test

Kapsayıcı görüntülerini bir Azure Container Registry’de depolama hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Container Registry’ye görüntüleri gönderme](./tutorial-kubernetes-prepare-acr.md)
