---
title: (KULLANIM DIŞI) Azure Container Service Öğreticisi - uygulama hazırlama
description: Azure Container Service öğreticisi - Uygulama Hazırlama
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 02/26/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 961f09d7581a26596b03e7a2f8e97d98fe83e6aa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61217104"
---
# <a name="deprecated-create-container-images-to-be-used-with-azure-container-service"></a>(KULLANIM DIŞI) Azure Container Service ile kullanılmak üzere kapsayıcı görüntüleri oluşturma

> [!TIP]
> Azure Kubernetes hizmeti kullanan Bu öğretici için güncelleştirilmiş sürümü görmek [Öğreticisi: Azure Kubernetes Service (AKS) için uygulama hazırlama](../../aks/tutorial-kubernetes-prepare-app.md).

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

Bu yedi parçalık öğreticinin ilk bölümünde, bir çoklu kapsayıcı uygulaması Kubernetes’te kullanılmak üzere hazırlanmaktadır. Tamamlanan adımlar:  

> [!div class="checklist"]
> * GitHub’dan uygulama kaynağını kopyalama  
> * Uygulama kaynağından kapsayıcı görüntüsü oluşturma
> * Uygulamayı yerel bir Docker ortamında test etme

Tamamlandıktan sonra, aşağıdaki uygulamaya yerel geliştirme ortamınızdan erişilebilir.

![Azure’da Kubernetes kümesinin görüntüsü](media/container-service-kubernetes-tutorials/azure-vote.png)

Sonraki öğreticilerde, kapsayıcı görüntüsü Azure Container Registry’ye yüklenip Azure’da barındırılan bir Kubernetes kümesinde çalıştırılır.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğreticide kapsayıcılar, kapsayıcı görüntüleri ve temel docker komutları gibi temel Docker kavramları hakkında bilgi sahibi olduğunuz varsayılmıştır. Gerekirse kapsayıcı temelleri hakkında bilgi için bkz. [Docker ile çalışmaya başlama]( https://docs.docker.com/get-started/). 

Bu öğreticiyi tamamlamak için Docker geliştirme ortamı gerekir. Docker, [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) veya [Linux](https://docs.docker.com/engine/installation/#supported-platforms)’ta Docker’ı kolayca yapılandırmanızı sağlayan paketler sağlar.

Azure Cloud Shell, bu öğreticideki her adımı tamamlamak için gerekli olan Docker bileşenlerini içermez. Bu yüzden, eksiksiz bir Docker geliştirme ortamı kullanmanızı öneririz.

## <a name="get-application-code"></a>Uygulama kodunu alma

Bu öğreticide kullanılan örnek uygulama, temel oylama uygulamasıdır. Bu uygulama, ön uç bileşen ile arka uç Redis örneğinden oluşur. Web bileşeni, özel kapsayıcı görüntüsüne paketlenmiştir. Redis örneği, Docker Hub’dan alınan değiştirilmemiş bir görüntü kullanır.  

Geliştirme ortamına uygulamanın bir kopyasını indirmek için Git kullanın.

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

Kopyalanan dizinden çalışabilmeniz için dizinleri değiştirin.

```
cd azure-voting-app-redis
```

Dizinin içinde uygulama kaynak kodu, önceden oluşturulmuş Docker Compose dosyası ve Kubernetes bildirim dosyası bulunur. Bu dosyalar öğretici kümesi boyunca kullanılır. 

## <a name="create-container-images"></a>Kapsayıcı görüntüleri oluşturma

[Docker Compose](https://docs.docker.com/compose/) kapsayıcı görüntülerinden alınan derlemeyi ve çoklu kapsayıcı uygulamalarının dağıtımını otomatikleştirmek için kullanılabilir.

Kapsayıcı görüntüsünü oluşturmak için `docker-compose.yml` dosyasını çalıştırın, Redis görüntüsünü indirin ve uygulamayı başlatın.

```bash
docker-compose up -d
```

Tamamlandığında, oluşturulan görüntüleri görmek için [docker images](https://docs.docker.com/engine/reference/commandline/images/) komutunu kullanın.

```bash
docker images
```

İndirilen veya oluşturulan üç görüntü olduğunu göz önünde bulundurun. `azure-vote-front` görüntüsü uygulamayı içerir ve temel olarak `nginx-flask` görüntüsünü kullanır. `redis` görüntüsü bir Redis örneği başlatmak için kullanılır.

```bash
REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

Çalışan kapsayıcıları görmek için [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) komutunu kullanın.

```bash
docker ps
```

Çıkış:

```bash
CONTAINER ID        IMAGE             COMMAND                  CREATED             STATUS              PORTS                           NAMES
82411933e8f9        azure-vote-front  "/usr/bin/supervisord"   57 seconds ago      Up 30 seconds       443/tcp, 0.0.0.0:8080->80/tcp   azure-vote-front
b68fed4b66b6        redis             "docker-entrypoint..."   57 seconds ago      Up 30 seconds       0.0.0.0:6379->6379/tcp          azure-vote-back
```

## <a name="test-application-locally"></a>Uygulamayı yerel olarak test etme

Çalıştırılan uygulamayı görüntülemek için `http://localhost:8080` adresine göz atın.

![Azure’da Kubernetes kümesinin görüntüsü](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık uygulama işlevselliği doğrulandığından, çalışan kapsayıcılar durdurulup kaldırılabilir. Kapsayıcı görüntülerini silmeyin. Sonraki öğreticide `azure-vote-front` görüntüsü bir Azure Container Registry örneğine yüklenir.

Çalışan kapsayıcıları durdurmak için aşağıdaki komutu çalıştırın.

```bash
docker-compose stop
```

Aşağıdaki komutu kullanarak, durdurulan kapsayıcıları ve kaynakları silin.

```bash
docker-compose down
```

Tamamlandığında, Azure Vote uygulamasını içeren bir kapsayıcı görüntüsüne sahip olursunuz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir uygulama test edildi ve bu uygulamaya yönelik kapsayıcı görüntüleri oluşturuldu. Aşağıdaki adımlar tamamlandı:

> [!div class="checklist"]
> * GitHub’dan uygulama kaynağını kopyalama  
> * Uygulama kaynağından kapsayıcı görüntüsü oluşturuldu
> * Uygulama yerel bir Docker ortamında test edildi

Kapsayıcı görüntülerini bir Azure Container Registry’de depolama hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Container Registry’ye görüntüleri gönderme](./container-service-tutorial-kubernetes-prepare-acr.md)
