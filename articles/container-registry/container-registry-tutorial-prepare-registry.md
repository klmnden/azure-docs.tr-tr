---
title: Azure Container Registry öğreticisi - Coğrafi çoğaltmalı Azure Container Registry’yi hazırlama
description: Bir Azure Container Registry oluşturun, coğrafi çoğaltma yapılandırın, bir Docker görüntüsü hazırlayın ve bunu kayıt defterine dağıtın. Üç bölümden oluşan bir serinin birinci bölümü.
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: tutorial
ms.date: 10/26/2017
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 3463acc3db3dae9633635aaf7410d876aacf9b38
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="tutorial-prepare-a-geo-replicated-azure-container-registry"></a>Öğretici: Coğrafi çoğaltmalı Azure Container Registry’yi hazırlama

Azure Container Registry, ağı dağıtımlarınıza yakın tutabileceğiniz, Azure’da dağıtılan özel bir Docker kayıt defteridir. Bu üç öğretici makalesinde, Linux kapsayıcısında çalıştırılan bir ASP.NET Core web uygulamasını iki [Kapsayıcılar için Web Apps](../app-service/containers/index.yml) örneğine dağıtmak için coğrafi çoğaltmanın nasıl kullanılacağını öğreneceksiniz. Azure’ın görüntüyü otomatik olarak en yakın coğrafi çoğaltmalı depodan her bir Web Uygulaması örneğine nasıl dağıttığını göreceksiniz.

Bu öğreticide, üç bölümden oluşan bir serinin birinci bölümü:

> [!div class="checklist"]
> * Coğrafi çoğaltmalı Azure Container Registry oluşturma
> * Uygulama kaynak kodunu GitHub’dan kopyalama
> * Uygulama kaynağından bir Docker kapsayıcısı görüntüsü oluşturma
> * Kapsayıcı görüntüsünü kayıt defterinize gönderme

Sonraki öğreticilerde, kapsayıcıyı özel kayıt defterinizden iki Azure bölgesinde çalıştırılan bir web uygulamasına dağıtacaksınız. Daha sonra uygulamadaki kodu güncelleştirecek ve her iki Web Uygulaması örneğini kayıt defterinize yönelik tek bir `docker push` ile güncelleştireceksiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğretici için Azure CLI 2.0.20 veya sonraki bir sürümü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

Bu öğreticide kapsayıcılar, kapsayıcı görüntüleri ve temel docker komutları gibi temel Docker kavramları hakkında bilgi sahibi olduğunuz varsayılmıştır. Gerekirse kapsayıcı temelleri hakkında bilgi için bkz. [Docker ile çalışmaya başlama]( https://docs.docker.com/get-started/).

Bu öğreticiyi tamamlamak için Docker geliştirme ortamı gerekir. Docker, [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) veya [Linux](https://docs.docker.com/engine/installation/#supported-platforms)’ta Docker’ı kolayca yapılandırmanızı sağlayan paketler sağlar.

Azure Cloud Shell, bu öğreticideki her adımı tamamlamak için gerekli olan Docker bileşenlerini içermez. Bu nedenle, Azure CLI ve Docker geliştirme ortamının yerel bir yüklemesini öneririz.

> [!IMPORTANT]
> Azure Container Registry’nin coğrafi çoğaltma özelliği şu anda **önizleme** aşamasındadır. Önizlemeler, [ek kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.
>

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

[Azure Portal](http://portal.azure.com)’da oturum açın.

**Kaynak oluştur** > **Kapsayıcılar** > **Azure Container Registry** seçeneklerini belirleyin.

![Azure portalında kapsayıcı kayıt defteri oluşturma][tut-portal-01]

Aşağıdaki ayarlarla yeni kayıt defterinizi yapılandırın:

* **Kayıt defteri adı**: Azure’da genel olarak benzersiz olan ve 5-50 alfasayısal karakterden oluşan bir kayıt defteri adı oluşturun
* **Kaynak Grubu**: **Yeni oluştur** > `myResourceGroup`
* **Konum**: `West US`
* **Yönetici kullanıcı**: `Enable` (Kapsayıcılar için Web App’in görüntüleri çekmesi için gereklidir)
* **SKU**: `Premium` (coğrafi çoğaltma için gereklidir)

**Oluştur**’u seçerek ACR örneğini dağıtın.

![Azure portalında kapsayıcı kayıt defteri oluşturma][tut-portal-02]

Bu öğreticinin geri kalan aşamalarında, seçtiğiniz kapsayıcı **Kayıt defteri adı** için yer tutucu olarak `<acrName>` kullanacağız.

> [!TIP]
> Azure Container Registry genellikle birden fazla kapsayıcı konağında kullanılan uzun ömürlü kaynaklar olduğundan, kayıt defterinizi kendi kaynak grubunda oluşturmanızı öneririz. Coğrafi çoğaltmalı kayıt defterleri ve web kancaları yapılandırılırken bu ek kaynaklar aynı kaynak grubuna yerleştirilir.
>

## <a name="configure-geo-replication"></a>Coğrafi çoğaltmayı yapılandırma

Premium kayıt defterine sahip olduğunuza göre coğrafi çoğaltmayı yapılandırabilirsiniz. Sonraki öğreticide iki bölgede çalıştırılacak şekilde yapılandıracağınız web uygulamanız, en yakın kayıt defterinden kapsayıcı görüntülerini çeker.

Azure portalında yeni kapsayıcı kayıt defterinize gidin ve **HİZMETLER** bölümünden **Çoğaltmalar**’ı seçin:

![Azure portalı kapsayıcı kayıt defteri kullanıcı arabirimindeki Çoğaltmalar][tut-portal-03]

Coğrafi çoğaltma için uygun Azure bölgelerinin yeşil altıgenlerle temsil edildiği bir harita görüntülenir:

 ![Azure portalındaki bölge haritası][tut-map-01]

Yeşil altıgeni seçip ardından **Çoğaltma oluştur** bölümünden **Oluştur**’u seçerek Doğu ABD bölgesine kayıt defterinizi çoğaltın:

 ![Azure portalında çoğaltma oluşturmaya yönelik kullanıcı arabirimi][tut-portal-04]

Çoğaltma tamamlandığında portal her iki bölge için de *Hazır* durumunu gösterir. Çoğaltma durumunu yenilemek için **Yenile** düğmesini kullanın. Çoğaltmaların oluşturulması ve eşitlenmesi bir dakika kadar sürebilir.

![Azure portalında çoğaltma durumuna yönelik kullanıcı arabirimi][tut-portal-05]

## <a name="container-registry-login"></a>Kapsayıcı kayıt defterinde oturum açma

Coğrafi çoğaltmayı yapılandırdığınıza göre, bir kapsayıcı görüntüsü oluşturun ve kayıt defterinize gönderin. Görüntüleri göndermeden önce ACR örneğinizde oturum açmanız gerekir. [Temel, Standart ve Premium SKU’lar](container-registry-skus.md) ile Azure kimliğinizi kullanarak kimlik doğrulaması yapabilirsiniz.

Kimlik doğrulaması yapmak ve kayıt defterinize yönelik kimlik bilgilerini önbelleğe almak için [az acr login](https://docs.microsoft.com/cli/azure/acr#az_acr_login) komutunu kullanın. `<acrName>` değerini, önceki adımlarda oluşturduğunuz kayıt defterinin adıyla değiştirin.

```azurecli
az acr login --name <acrName>
```

Bu komut tamamlandığında `Login Succeeded` iletisini döndürür.

## <a name="get-application-code"></a>Uygulama kodunu alma

Bu öğreticideki örnek, [ASP.NET Core](http://dot.net) ile oluşturulmuş küçük bir web uygulamasını içerir. Uygulama, Azure Container Registry tarafından görüntünün dağıtıldığı kaynak bölgeyi görüntüleyen bir HTML sayfası görevi görür.

![Tarayıcıda gösterilen öğretici uygulama][tut-app-01]

Örneği yerel bir dizine ve `cd` öğesini şu dizine indirmek için git kullanın:

```bash
git clone https://github.com/Azure-Samples/acr-helloworld.git
cd acr-helloworld
```

## <a name="update-dockerfile"></a>Dockerfile’ı güncelleştirme

Örnekte bulunan Dockerfile, kapsayıcının nasıl yapılandırıldığını gösterir. Resmi bir [aspnetcore](https://store.docker.com/community/images/microsoft/aspnetcore) görüntüsünden başlatılır, uygulama dosyalarını kapsayıcıya kopyalar, bağımlılıkları yükler, resmi [aspnetcore-build](https://store.docker.com/community/images/microsoft/aspnetcore-build) görüntüsünü kullanarak çıktıyı derler ve son olarak iyileştirilmiş bir aspnetcore görüntüsü oluşturur.

Dockerfile, kopyalanan kaynaktaki `./AcrHelloworld/Dockerfile` dizininde bulunur.

```dockerfile
FROM microsoft/aspnetcore:2.0 AS base
# Update <acrName> with the name of your registry
# Example: uniqueregistryname.azurecr.io
ENV DOCKER_REGISTRY <acrName>.azurecr.io
WORKDIR /app
EXPOSE 80

FROM microsoft/aspnetcore-build:2.0 AS build
WORKDIR /src
COPY *.sln ./
COPY AcrHelloworld/AcrHelloworld.csproj AcrHelloworld/
RUN dotnet restore
COPY . .
WORKDIR /src/AcrHelloworld
RUN dotnet build -c Release -o /app

FROM build AS publish
RUN dotnet publish -c Release -o /app

FROM base AS production
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "AcrHelloworld.dll"]
```

*acr-helloworld* görüntüsündeki uygulama, kayıt defterinin oturum açma sunucusuyla ilgili bilgiler için DNS’yi sorgulayarak kapsayıcısının dağıtıldığı kaynak bölgeyi belirlemeye çalışır. Dockerfile’daki `DOCKER_REGISTRY` ortam değişkeninde kayıt defterinizin oturum açma sunucusu URL’sini belirtmeniz gerekir.

İlk olarak, `az acr show` komutuyla kayıt defterinin oturum açma sunucusu URL’sini alın. `<acrName>` değerini, önceki adımlarda oluşturduğunuz kayıt defterinin adıyla değiştirin.

```azurecli
az acr show --name <acrName> --query "{acrLoginServer:loginServer}" --output table
```

Çıktı:

```bash
AcrLoginServer
-----------------------------
uniqueregistryname.azurecr.io
```

Sonra, `DOCKER_REGISTRY` satırını kayıt defterinizin oturum açma sunucusu URL’si ile güncelleştirin. Bu örnekte, örnek kayıt defteri adımızı (*uniqueregistryname*) yansıtacak şekilde satırı güncelleştiririz:

```dockerfile
ENV DOCKER_REGISTRY uniqueregistryname.azurecr.io
```

## <a name="build-container-image"></a>Kapsayıcı görüntüsü oluşturma

Dockerfile’ı kayıt defterinizin URL’si ile güncelleştirdiğinize göre, kapsayıcı görüntüsünü oluşturmak için `docker build` kullanabilirsiniz. Tekrar `<acrName>` değerini, kayıt defterinizin adıyla değiştirerek görüntüyü oluşturmak ve özel kayıt defterinizin URL’si ile etiketlemek için aşağıdaki komutu çalıştırın:

```bash
docker build . -f ./AcrHelloworld/Dockerfile -t <acrName>.azurecr.io/acr-helloworld:v1
```

Docker görüntüsü oluşturulurken birçok çıktı satırı görüntülenir (burada kesilmiş şekilde gösterilmektedir):

```bash
Sending build context to Docker daemon  523.8kB
Step 1/18 : FROM microsoft/aspnetcore:2.0 AS base
2.0: Pulling from microsoft/aspnetcore
3e17c6eae66c: Pulling fs layer
...
Step 18/18 : ENTRYPOINT dotnet AcrHelloworld.dll
 ---> Running in 6906d98c47a1
 ---> c9ca1763cfb1
Removing intermediate container 6906d98c47a1
Successfully built c9ca1763cfb1
Successfully tagged uniqueregistryname.azurecr.io/acr-helloworld:v1
```

Oluşturulan görüntüyü görmek için `docker images` komutunu kullanın:

```bash
docker images
```

Çıktı:

```bash
REPOSITORY                                      TAG    IMAGE ID        CREATED               SIZE
uniqueregistryname.azurecr.io/acr-helloworld    v1     01ac48d5c8cf    About a minute ago    284MB
...
```

## <a name="push-image-to-azure-container-registry"></a>Azure Container Registry’ye görüntü gönderme

Son olarak, kayıt defterinize *acr-helloworld* görüntüsünü göndermek için `docker push` komutunu kullanın. `<acrName>` değerini kayıt defterinizin adıyla değiştirin.

```bash
docker push <acrName>.azurecr.io/acr-helloworld:v1
```

Coğrafi çoğaltma için kayıt defterinizi yapılandırdığınızdan, bu tek `docker push` komutuyla görüntünüz hem *Batı ABD* hem de *Doğu ABD* bölgesine otomatik olarak çoğaltılır.

Çıktı:

```bash
The push refers to a repository [uniqueregistryname.azurecr.io/acr-helloworld]
cd54739c444b: Pushed
d6803756744a: Pushed
b7b1f3a15779: Pushed
a89567dff12d: Pushed
59c7b561ff56: Pushed
9a2f9413d9e4: Pushed
a75caa09eb1f: Pushed
v1: digest: sha256:0799014f91384bda5b87591170b1242bcd719f07a03d1f9a1ddbae72b3543970 size: 1792
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, özel, coğrafi çoğaltmalı bir kayıt defteri oluşturdunuz, bir kapsayıcı görüntüsü oluşturdunuz ve sonra bu görüntüyü kayıt defterinize gönderdiniz. Bu öğreticideki adımları izleyerek şunları yaptınız:

> [!div class="checklist"]
> * Coğrafi çoğaltmalı Azure Container Registry oluşturdunuz
> * Uygulama kaynak kodunu GitHub’dan kopyaladınız
> * Uygulama kaynağından bir Docker kapsayıcısı görüntüsü oluşturdunuz
> * Kapsayıcı görüntüsünü kayıt defterinize gönderdiniz

Görüntülere yerel olarak hizmet vermek için coğrafi çoğaltma kullanarak kapsayıcınızı birden fazla Kapsayıcılar için Web Apps’e dağıtma hakkında daha fazla bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Azure Container Registry’den web uygulaması dağıtma](container-registry-tutorial-deploy-app.md)

<!-- IMAGES -->
[tut-portal-01]: ./media/container-registry-tutorial-prepare-registry/tut-portal-01.png
[tut-portal-02]: ./media/container-registry-tutorial-prepare-registry/tut-portal-02.png
[tut-portal-03]: ./media/container-registry-tutorial-prepare-registry/tut-portal-03.png
[tut-portal-04]: ./media/container-registry-tutorial-prepare-registry/tut-portal-04.png
[tut-portal-05]: ./media/container-registry-tutorial-prepare-registry/tut-portal-05.png
[tut-app-01]: ./media/container-registry-tutorial-prepare-registry/tut-app-01.png
[tut-map-01]: ./media/container-registry-tutorial-prepare-registry/tut-map-01.png
