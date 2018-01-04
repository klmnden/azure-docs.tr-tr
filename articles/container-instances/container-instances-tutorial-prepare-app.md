---
title: "Azure kapsayıcı örnekleri Öğreticisi - uygulamanızı hazırlama"
description: "Azure kapsayıcı örnekleri öğretici Kısım 1 / 3 - bir uygulama dağıtımı Azure kapsayıcı örnekleri için hazırlama"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: fc16be80e776d1472be775fa32354ba157d16545
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="create-container-for-deployment-to-azure-container-instances"></a>Azure Container Instances’a dağıtılacak kapsayıcıyı oluşturma

Azure Container Instances, Docker kapsayıcılarının herhangi bir sanal makine sağlama veya herhangi bir üst düzey hizmet benimsenmesi gerekmeden Azure altyapısına dağıtılmasını sağlar. Bu öğreticide Node.js içinde küçük web uygulaması oluşturma ve Azure kapsayıcı örneği kullanılarak çalıştırılabilir bir kapsayıcıda paket.

Bu makalede, bir dizi Kısım:

> [!div class="checklist"]
> * Uygulama kaynak koduna github'dan kopyalama
> * Uygulama kaynağından bir kapsayıcı görüntüsü oluşturun
> * Yerel bir Docker ortamda test görüntüsü

Sonraki öğreticilerde, Azure kapsayıcı kayıt defterine görüntünüzü karşıya yükleyin ve ardından Azure kapsayıcı örnekleri dağıtın.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğretici, Azure CLI Sürüm 2.0.23 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Gerekirse yükleyin veya yükseltin, bakın [Azure CLI 2.0 yükleme][azure-cli-install].

Bu öğretici Docker kavramları kapsayıcıları, kapsayıcı görüntüler ve basic gibi çekirdek temel bir anlayış varsayar `docker` komutları. Gerekirse, bkz: [Docker ile çalışmaya başlama] [ docker-get-started] kapsayıcı temelleri öncü için.

Bu öğreticiyi tamamlamak için yerel olarak yüklenmiş bir Docker geliştirme ortamı gerekir. Docker sağlar kolayca Docker herhangi yapılandırdığınız paketler [Mac][docker-mac], [Windows][docker-windows], veya [Linux] [ docker-linux] sistem.

Azure bulut Kabuk her adımı tamamlamak için gereken Docker bileşenleri Bu öğretici içermez. Bu öğreticiyi tamamlamak için yerel bilgisayarınızda Azure CLI ve Docker geliştirme ortamı yüklemeniz gerekir.

## <a name="get-application-code"></a>Uygulama kodunu alma

Bu öğreticideki örnek yerleşik basit bir web uygulaması içeren [Node.js][nodejs]. Uygulama bir statik HTML sayfası sunar ve şöyle görünür:

![Tarayıcıda gösterilen öğretici uygulama][aci-tutorial-app]

Örneği indirmek için git kullanma:

```bash
git clone https://github.com/Azure-Samples/aci-helloworld.git
```

## <a name="build-the-container-image"></a>Kapsayıcı görüntüsünü oluşturma

Örnek deposunda sağlanan Dockerfile, kapsayıcının nasıl yapılandırıldığını gösterir. Gelen başlatır bir [resmi Node.js görüntü] [ docker-hub-nodeimage] göre [Alpine Linux][alpine-linux], ile kullanmak için uygun olan küçük bir dağıtımı kapsayıcı. Ardından uygulama dosyalarını kapsayıcıya kopyalar, Node Package Manager’ı kullanarak bağımlılıkları yükler ve son olarak uygulamayı başlatır.

```Dockerfile
FROM node:8.9.3-alpine
RUN mkdir -p /usr/src/app
COPY ./app/* /usr/src/app/
WORKDIR /usr/src/app
RUN npm install
CMD node /usr/src/app/index.js
```

Kullanım [docker yapı] [ docker-build] olarak etiketleme kapsayıcı görüntüsü oluşturmak için komut *aci öğretici uygulama*:

```bash
docker build ./aci-helloworld -t aci-tutorial-app
```

Çıktı [docker yapı] [ docker-build] komuttur (okunabilirlik için kesilmiş) aşağıdakine benzer:

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

Kullanım [docker görüntüleri] [ docker-images] yerleşik bir görüntü görmek için komutu:

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
