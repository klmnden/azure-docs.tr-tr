---
title: "Azure kapsayıcı örnekleri Öğreticisi - uygulamanızı hazırlama"
description: "Azure Container Instances’a dağıtılacak uygulamayı hazırlama"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: mmacy
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/07/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 5de53266e1dbadecb9fabb1649615fa9f4ba8b5f
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="create-container-for-deployment-to-azure-container-instances"></a>Azure Container Instances’a dağıtılacak kapsayıcıyı oluşturma

Azure Container Instances, Docker kapsayıcılarının herhangi bir sanal makine sağlama veya herhangi bir üst düzey hizmet benimsenmesi gerekmeden Azure altyapısına dağıtılmasını sağlar. Bu öğreticide Node.js içinde küçük web uygulaması oluşturma ve Azure kapsayıcı örneği kullanılarak çalıştırılabilir bir kapsayıcıda paket. Kapsanan konular:

> [!div class="checklist"]
> * GitHub’dan uygulama kaynağını kopyalama
> * Uygulama kaynağından kapsayıcı görüntüsü oluşturma
> * Görüntüleri yerel bir Docker ortamında test etme

Sonraki öğreticilerde, Azure kapsayıcı kayıt defterine görüntünüzü karşıya yükleyin ve ardından Azure kapsayıcı örnekleri dağıtın.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğretici, Azure CLI Sürüm 2.0.20 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli).

Bu öğretici Docker kavramları kapsayıcıları, kapsayıcı görüntüler ve basic gibi çekirdek temel bir anlayış varsayar `docker` komutları. Gerekirse kapsayıcı temelleri hakkında bilgi için bkz. [Docker ile çalışmaya başlama]( https://docs.docker.com/get-started/).

Bu öğreticiyi tamamlamak için Docker geliştirme ortamı gerekir. Docker, [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) veya [Linux](https://docs.docker.com/engine/installation/#supported-platforms)’ta Docker’ı kolayca yapılandırmanızı sağlayan paketler sağlar.

Azure bulut Kabuk her adımı tamamlamak için gereken Docker bileşenleri Bu öğretici içermez. Bu nedenle, Azure CLI ve Docker geliştirme ortamı yerel bir yüklemesini öneririz.

## <a name="get-application-code"></a>Uygulama kodunu alma

Bu öğreticideki örnek, [Node.js](http://nodejs.org) ile yapılmış basit bir web uygulaması içerir. Uygulama bir statik HTML sayfası sunar ve şöyle görünür:

![Tarayıcıda gösterilen öğretici uygulama][aci-tutorial-app]

Örneği indirmek için git kullanma:

```bash
git clone https://github.com/Azure-Samples/aci-helloworld.git
```

## <a name="build-the-container-image"></a>Kapsayıcı görüntüsünü oluşturma

Örnek deposunda sağlanan Dockerfile, kapsayıcının nasıl yapılandırıldığını gösterir. Kapsayıcılarla kullanmaya uygun küçük bir dağıtım olan [Alpine Linux](https://alpinelinux.org/) tabanlı [resmi bir Node.js görüntüsünden][dockerhub-nodeimage] başlatılır. Ardından uygulama dosyalarını kapsayıcıya kopyalar, Node Package Manager’ı kullanarak bağımlılıkları yükler ve son olarak uygulamayı başlatır.

```Dockerfile
FROM node:8.2.0-alpine
RUN mkdir -p /usr/src/app
COPY ./app/* /usr/src/app/
WORKDIR /usr/src/app
RUN npm install
CMD node /usr/src/app/index.js
```

Kapsayıcı görüntüsünü oluşturmak için `docker build` komutunu kullanın ve görüntüyü *aci-tutorial-app* olarak etiketleyin:

```bash
docker build ./aci-helloworld -t aci-tutorial-app
```

Çıktı `docker build` komuttur (okunabilirlik için kesilmiş) aşağıdakine benzer:

```bash
Sending build context to Docker daemon  119.3kB
Step 1/6 : FROM node:8.2.0-alpine
8.2.0-alpine: Pulling from library/node
88286f41530e: Pull complete
84f3a4bf8410: Pull complete
d0d9b2214720: Pull complete
Digest: sha256:c73277ccc763752b42bb2400d1aaecb4e3d32e3a9dbedd0e49885c71bea07354
Status: Downloaded newer image for node:8.2.0-alpine
 ---> 90f5ee24bee2
...
Step 6/6 : CMD node /usr/src/app/index.js
 ---> Running in f4a1ea099eec
 ---> 6edad76d09e9
Removing intermediate container f4a1ea099eec
Successfully built 6edad76d09e9
Successfully tagged aci-tutorial-app:latest
```

Yerleşik görüntüyü görmek için `docker images` komutunu kullanın:

```bash
docker images
```

Çıktı:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

## <a name="run-the-container-locally"></a>Kapsayıcıyı yerel olarak çalıştırma

Kapsayıcıyı Azure Container Instances’a dağıtmayı denemeden önce yerel olarak çalıştırarak çalışır durumda olduğunu doğrulayın. `-d` anahtarı kapsayıcının arka planda çalışmasını sağlar. `-p` ise işleminizdeki rastgele bağlantı noktalarından birini kapsayıcının 80 numaralı bağlantı noktasına eşlemenizi sağlar.

```bash
docker run -d -p 8080:80 aci-tutorial-app
```

Kapsayıcının çalışır durumda olduğunu doğrulamak için tarayıcıda http://localhost:8080 adresine gidin.

![Uygulamayı tarayıcıda yerel olarak çalıştırma][aci-tutorial-app-local]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Container Instances’a dağıtılabilir bir kapsayıcı görüntüsü oluşturdunuz. Aşağıdaki adımlar tamamlandı:

> [!div class="checklist"]
> * Uygulama kaynağı github'dan kopyalanamıyor
> * Uygulama kaynağı oluşturulan kapsayıcı görüntüleri
> * Kapsayıcı yerel olarak test

Kapsayıcı görüntülerini bir Azure Container Registry’de depolama hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Container Registry’ye görüntüleri gönderme](./container-instances-tutorial-prepare-acr.md)

<!-- LINKS -->
[dockerhub-nodeimage]: https://store.docker.com/images/node

<!--- IMAGES --->
[aci-tutorial-app]:./media/container-instances-quickstart/aci-app-browser.png
[aci-tutorial-app-local]: ./media/container-instances-tutorial-prepare-app/aci-app-browser-local.png