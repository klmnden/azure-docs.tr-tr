---
title: "Azure Container Instances öğreticisi - Uygulamanızı hazırlama | Azure Docs"
description: "Azure Container Instances’a dağıtılacak uygulamayı hazırlama"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: 
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2017
ms.author: seanmck
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: c30998a77071242d985737e55a7dc2c0bf70b947
ms.openlocfilehash: 07ad1a6edbcb4d6160b37b4923586e23058f3c04
ms.contentlocale: tr-tr
ms.lasthandoff: 08/02/2017

---

# <a name="create-container-for-deployment-to-azure-container-instances"></a>Azure Container Instances’a dağıtılacak kapsayıcıyı oluşturma

Azure Container Instances, Docker kapsayıcılarının herhangi bir sanal makine sağlama veya herhangi bir üst düzey hizmet benimsenmesi gerekmeden Azure altyapısına dağıtılmasını sağlar. Bu öğreticide, Node.js içinde basit bir web uygulaması oluşturacak ve bu uygulamayı Azure Container Instances kullanılarak çalıştırılabilir bir kapsayıcıda paketleyeceksiniz. Şu konular yer almaktadır:

> [!div class="checklist"]
> * GitHub’dan uygulama kaynağını kopyalama  
> * Uygulama kaynağından kapsayıcı görüntüsü oluşturma
> * Görüntüleri yerel bir Docker ortamında test etme

Sonraki öğreticilerde, görüntülerinizi Azure Container Registry’ye yükleyecek ve Azure Container Instances’a dağıtacaksınız.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğreticide kapsayıcılar, kapsayıcı görüntüleri ve temel docker komutları gibi temel Docker kavramları hakkında bilgi sahibi olduğunuz varsayılmıştır. Gerekirse kapsayıcı temelleri hakkında bilgi için bkz. [Docker ile çalışmaya başlama]( https://docs.docker.com/get-started/). 

Bu öğreticiyi tamamlamak için Docker geliştirme ortamı gerekir. Docker, [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) veya [Linux](https://docs.docker.com/engine/installation/#supported-platforms)’ta Docker’ı kolayca yapılandırmanızı sağlayan paketler sağlar.

## <a name="get-application-code"></a>Uygulama kodunu alma

Bu öğreticideki örnek, [Node.js](http://nodejs.org) ile yapılmış basit bir web uygulaması içerir. Uygulama bir statik HTML sayfası sunar ve şöyle görünür:

![Tarayıcıda gösterilen öğretici uygulama][aci-tutorial-app]

Örneği indirmek için git kullanma:

```bash
git clone https://github.com/Azure-Samples/aci-helloworld.git
```

## <a name="build-the-container-image"></a>Kapsayıcı görüntüsünü oluşturma

Örnek deposunda sağlanan Dockerfile, kapsayıcının nasıl yapılandırıldığını gösterir. Kapsayıcılarla kullanmaya uygun küçük bir dağıtım olan [Alpine Linux](https://alpinelinux.org/) tabanlı [resmi bir Node.js görüntüsünden][dockerhub-nodeimage] başlatılır. Ardından uygulama dosyalarını kapsayıcıya kopyalar, Node Package Manager’ı kullanarak bağımlılıkları yükler ve son olarak uygulamayı başlatır.

```
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
> * GitHub’dan uygulama kaynağını kopyalama  
> * Uygulama kaynağından kapsayıcı görüntüsü oluşturma
> * Kapsayıcıyı yerel olarak test etme

Kapsayıcı görüntülerini bir Azure Container Registry’de depolama hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Container Registry’ye görüntüleri gönderme](./container-instances-tutorial-prepare-acr.md)

<!-- LINKS -->
[dockerhub-nodeimage]: https://hub.docker.com/r/library/node/tags/8.2.0-alpine/

<!--- IMAGES --->
[aci-tutorial-app]:./media/container-instances-quickstart/aci-app-browser.png
[aci-tutorial-app-local]: ./media/container-instances-tutorial-prepare-app/aci-app-browser-local.png
