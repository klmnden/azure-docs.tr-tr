---
title: "Azure kapsayıcı hizmeti Öğreticisi - App hazırlama"
description: "Azure kapsayıcı hizmeti Öğreticisi - App hazırlama"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 3dcfc0d454926d6040692b0e8a9d44b13e7603c5
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="create-container-images-to-be-used-with-azure-container-service"></a>Azure kapsayıcı hizmeti ile kullanılmak üzere kapsayıcı görüntüleri oluşturma

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

Bu öğreticide, bölümü yedi, biri çok kapsayıcı uygulama Kubernetes kullanım için hazırlanır. Tamamlanan adımları içerir:  

> [!div class="checklist"]
> * GitHub’dan uygulama kaynağını kopyalama  
> * Uygulama kaynağından bir kapsayıcı görüntü oluşturma
> * Uygulamayı bir yerel Docker ortamında test etme

Tamamlandığında, aşağıdaki uygulama yerel geliştirme ortamınızda erişilebilir.

![Azure’da Kubernetes kümesinin görüntüsü](media/container-service-kubernetes-tutorials/azure-vote.png)

Sonraki öğreticilerde, kapsayıcı görüntünün bir Azure kapsayıcı kayıt defterine yüklenir ve ardından Azure çalıştırmada Kubernetes küme barındırılan.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğreticide kapsayıcılar, kapsayıcı görüntüleri ve temel docker komutları gibi temel Docker kavramları hakkında bilgi sahibi olduğunuz varsayılmıştır. Gerekirse kapsayıcı temelleri hakkında bilgi için bkz. [Docker ile çalışmaya başlama]( https://docs.docker.com/get-started/). 

Bu öğreticiyi tamamlamak için Docker geliştirme ortamı gerekir. Docker, [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) veya [Linux](https://docs.docker.com/engine/installation/#supported-platforms)’ta Docker’ı kolayca yapılandırmanızı sağlayan paketler sağlar.

Azure bulut Kabuk her adımı tamamlamak için gereken Docker bileşenleri Bu öğretici içermez. Bu nedenle, bir tam Docker geliştirme ortamında kullanmanızı öneririz.

## <a name="get-application-code"></a>Uygulama kodunu alma

Bu öğreticide kullanılan örnek temel bir oylama uygulama uygulamasıdır. Uygulama bir ön uç web bileşeni ve bir arka uç Redis örneği oluşur. Web bileşeni özel kapsayıcı görüntüsüne paketlenmiştir. Redis örneği Docker hub'a değiştirilmemiş bir görüntüden kullanır.  

Geliştirme ortamınızı uygulamaya bir kopyasını indirmek için Git kullanın.

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

Dizinleri kopyalanan dizinden çalıştığınız şekilde değiştirin.

```
cd azure-voting-app-redis
```

Dizini içinde uygulama kaynak koduna, önceden oluşturulmuş bir Docker compose dosyası ve bir Kubernetes bildirim dosyası. Bu dosyalar öğretici kümesi kullanılır. 

## <a name="create-container-images"></a>Kapsayıcı görüntüleri oluşturma

[Docker Compose](https://docs.docker.com/compose/) yapı kapsayıcı görüntüler ve birden çok kapsayıcı uygulamalarının dağıtımını otomatik hale getirmek için kullanılabilir.

Çalıştırma `docker-compose.yml` kapsayıcı görüntü oluşturma, Redis görüntüsünü karşıdan yüklemek ve uygulamayı başlatmak için dosya.

```bash
docker-compose up -d
```

Tamamlandığında kullanmak [docker görüntüleri](https://docs.docker.com/engine/reference/commandline/images/) oluşturulan görüntüleri görmek için komutu.

```bash
docker images
```

Üç görüntüleri indirilebilir veya oluşturulan dikkat edin. `azure-vote-front` Görüntü uygulama içerir ve kullandığı `nginx-flask` görüntüyü temel olarak. `redis` Görüntü bir Redis örneği başlatmak için kullanılır.

```bash
REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

Çalıştırma [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) çalışan kapsayıcıları görmek için komutu.

```bash
docker ps
```

Çıktı:

```bash
CONTAINER ID        IMAGE             COMMAND                  CREATED             STATUS              PORTS                           NAMES
82411933e8f9        azure-vote-front  "/usr/bin/supervisord"   57 seconds ago      Up 30 seconds       443/tcp, 0.0.0.0:8080->80/tcp   azure-vote-front
b68fed4b66b6        redis             "docker-entrypoint..."   57 seconds ago      Up 30 seconds       0.0.0.0:6379->6379/tcp          azure-vote-back
```

## <a name="test-application-locally"></a>Uygulamayı yerel olarak test etme

Çalışan uygulama görmek için http://localhost: 8080 için göz atın.

![Azure’da Kubernetes kümesinin görüntüsü](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Uygulama işlevselliği doğrulandı, çalışan kapsayıcılar durdurulur ve kaldırılmış. Kapsayıcı görüntüleri silmeyin. `azure-vote-front` Görüntü sonraki öğreticide Azure kapsayıcı kayıt defteri örneğine yüklenir.

Çalışan kapsayıcılar durdurmak için aşağıdaki komutu çalıştırın.

```bash
docker-compose stop
```

Durdurulan kapsayıcıları ve aşağıdaki komut ile kaynakları silin.

```bash
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
> [Azure Container Registry’ye görüntüleri gönderme](./container-service-tutorial-kubernetes-prepare-acr.md)
