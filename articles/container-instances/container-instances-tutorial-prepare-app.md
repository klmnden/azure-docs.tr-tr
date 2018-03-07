---
title: "Azure Container Instances öğreticisi - Uygulamanızı hazırlama"
description: "Azure Container Instances öğreticisi, 1/3. Bölüm - Azure Container Instances’a dağıtılacak uygulamayı hazırlama"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 5012412ec642a04102836274caea253635376efb
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="create-container-for-deployment-to-azure-container-instances"></a>Azure Container Instances’a dağıtılacak kapsayıcıyı oluşturma

Azure Container Instances, Docker kapsayıcılarının herhangi bir sanal makine sağlama veya herhangi bir üst düzey hizmet benimsenmesi gerekmeden Azure altyapısına dağıtılmasını sağlar. Bu öğreticide, Node.js içinde küçük bir web uygulaması oluşturur ve bu uygulamayı Azure Container Instances kullanılarak çalıştırılabilir bir kapsayıcıda paketlersiniz.

Serinin ilk bölümündeki bu makalede şunları yapacaksınız:

> [!div class="checklist"]
> * Uygulama kaynak kodunu GitHub’dan kopyalama
> * Uygulama kaynağından kapsayıcı görüntüsü oluşturma
> * Görüntüyü yerel bir Docker ortamında test etme

Sonraki öğreticilerde, görüntünüzü Azure Container Registry’ye yükler ve Azure Container Instances’a dağıtırsınız.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğretici için Azure CLI 2.0.23 veya sonraki bir sürümü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme][azure-cli-install].

Bu öğreticide kapsayıcılar, kapsayıcı görüntüleri ve temel docker komutları gibi temel `docker` kavramları hakkında bilgi sahibi olduğunuz varsayılmıştır. Gerekirse kapsayıcı temelleri hakkında bilgi için bkz. [Docker ile çalışmaya başlama][docker-get-started].

Bu öğreticiyi tamamlamak için yerel olarak yüklü bir Docker geliştirme ortamı gerekir. Docker [Mac][docker-mac], [Windows][docker-windows] veya [Linux][docker-linux]'ta Docker'ı kolayca yapılandırmanızı sağlayan paketler sağlar.

Azure Cloud Shell, bu öğreticideki her adımı tamamlamak için gerekli olan Docker bileşenlerini içermez. Bu öğreticiyi tamamlamak için, yerel bilgisayarınıza Azure CLI ve Docker geliştirme ortamını yüklemeniz gerekir.

## <a name="get-application-code"></a>Uygulama kodunu alma

Bu öğreticideki örnek, [Node.js][nodejs] ile yapılmış basit bir web uygulaması içerir. Uygulama bir statik HTML sayfası sunar ve şöyle görünür:

![Tarayıcıda gösterilen öğretici uygulama][aci-tutorial-app]

Örneği indirmek için git kullanma:

```bash
git clone https://github.com/Azure-Samples/aci-helloworld.git
```

## <a name="build-the-container-image"></a>Kapsayıcı görüntüsünü oluşturma

Örnek deposunda sağlanan Dockerfile, kapsayıcının nasıl yapılandırıldığını gösterir. Kapsayıcılarla kullanmaya uygun küçük bir dağıtım olan [Alpine Linux][alpine-linux] tabanlı [resmi bir Node.js görüntüsünden][docker-hub-nodeimage] başlatılır. Ardından uygulama dosyalarını kapsayıcıya kopyalar, Node Package Manager’ı kullanarak bağımlılıkları yükler ve son olarak uygulamayı başlatır.

```Dockerfile
FROM node:8.9.3-alpine
RUN mkdir -p /usr/src/app
COPY ./app/ /usr/src/app/
WORKDIR /usr/src/app
RUN npm install
CMD node /usr/src/app/index.js
```

Kapsayıcı görüntüsünü oluşturmak için [docker build][docker-build] komutunu kullanın ve görüntüyü *aci-tutorial-app* olarak etiketleyin:

```bash
docker build ./aci-helloworld -t aci-tutorial-app
```

[docker build][docker-build] komutunun çıktısı aşağıdakine benzer (okunabilirliği artırmak için kesilmiştir):

```bash
Sending build context to Docker daemon  119.3kB
Step 1/6 : FROM node:8.9.3-alpine
8.9.3-alpine: Pulling from library/node
88286f41530e: Pull complete
84f3a4bf8410: Pull complete
d0d9b2214720: Pull complete
Digest: sha256:c73277ccc763752b42bb2400d1aaecb4e3d32e3a9dbedd0e49885c71bea07354
Status: Downloaded newer image for node:8.9.3-alpine
 ---> 90f5ee24bee2
...
Step 6/6 : CMD node /usr/src/app/index.js
 ---> Running in f4a1ea099eec
 ---> 6edad76d09e9
Removing intermediate container f4a1ea099eec
Successfully built 6edad76d09e9
Successfully tagged aci-tutorial-app:latest
```

Oluşturulan görüntüyü görmek için [docker images][docker-images] komutunu kullanın:

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
> * GitHub’dan uygulama kaynağı kopyalandı
> * Uygulama kaynağından kapsayıcı görüntüsü oluşturuldu
> * Kapsayıcı yerel olarak test edildi

Kapsayıcı görüntülerini bir Azure Container Registry’de depolama hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Container Registry’ye görüntüleri gönderme](./container-instances-tutorial-prepare-acr.md)

<!--- IMAGES --->
[aci-tutorial-app]:./media/container-instances-quickstart/aci-app-browser.png
[aci-tutorial-app-local]: ./media/container-instances-tutorial-prepare-app/aci-app-browser-local.png

<!-- LINKS - External -->
[alpine-linux]: https://alpinelinux.org/
[docker-build]: https://docs.docker.com/engine/reference/commandline/build/
[docker-get-started]: https://docs.docker.com/get-started/
[docker-hub-nodeimage]: https://store.docker.com/images/node
[docker-images]: https://docs.docker.com/engine/reference/commandline/images/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/
[nodejs]: http://nodejs.org

<!-- LINKS - Internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
