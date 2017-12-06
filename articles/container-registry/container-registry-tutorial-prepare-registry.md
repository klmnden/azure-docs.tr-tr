---
title: "Azure kapsayıcı kayıt defteri Öğreticisi - bir coğrafi olarak çoğaltılmış Azure kapsayıcı kayıt defteri hazırlama"
description: "Azure kapsayıcı kayıt defteri oluşturma coğrafi çoğaltma yapılandırma, Docker görüntüsünü hazırlamak ve kayıt defterine dağıtın. Bölümü üç bölümlük biri."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: tutorial
ms.date: 10/26/2017
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 9503dc4a1fa8bc7773677de1673fed3abe1c1b51
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="prepare-a-geo-replicated-azure-container-registry"></a>Coğrafi olarak çoğaltılmış Azure kapsayıcı kayıt defteri hazırlama

Azure kapsayıcı kayıt defteri dağıtımlarınız için ağ Kapat tutabilirsiniz azure'da dağıtılan özel bir Docker kayıt defteri ' dir. Bu üç öğretici makaleleri kümesinde Linux kapsayıcısında iki olarak çalışan bir ASP.NET Core web uygulama dağıtmak için coğrafi çoğaltma kullanmayı öğrenin [kapsayıcıları için Web Apps](../app-service/containers/index.yml) örnekleri. Nasıl Azure otomatik olarak görüntünün her Web uygulaması örneğine en yakın coğrafi olarak çoğaltılmış depodan dağıtan görürsünüz.

Bu öğreticide, üç bölümlük birinde Kısım:

> [!div class="checklist"]
> * Coğrafi olarak çoğaltılmış Azure kapsayıcı kayıt defteri oluşturma
> * Uygulama kaynak koduna github'dan kopyalama
> * Bir Docker kapsayıcısı görüntüden Uygulama kaynağı oluşturun
> * Kayıt defterine kapsayıcı görüntü gönderme

Sonraki öğreticilerde, kapsayıcı özel kayıt defterinden iki Azure bölgelerinde çalışan bir web uygulaması dağıtın. Ardından uygulamasındaki kod güncelleştirme ve her iki Web uygulama örnekleri ile tek bir güncelleştirme `docker push` , kayıt defterine.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğretici, Azure CLI Sürüm 2.0.20 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

Bu öğreticide kapsayıcılar, kapsayıcı görüntüleri ve temel docker komutları gibi temel Docker kavramları hakkında bilgi sahibi olduğunuz varsayılmıştır. Gerekirse kapsayıcı temelleri hakkında bilgi için bkz. [Docker ile çalışmaya başlama]( https://docs.docker.com/get-started/).

Bu öğreticiyi tamamlamak için Docker geliştirme ortamı gerekir. Docker, [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) veya [Linux](https://docs.docker.com/engine/installation/#supported-platforms)’ta Docker’ı kolayca yapılandırmanızı sağlayan paketler sağlar.

Azure bulut Kabuk her adımı tamamlamak için gereken Docker bileşenleri Bu öğretici içermez. Bu nedenle, Azure CLI ve Docker geliştirme ortamı yerel bir yüklemesini öneririz.

> [!IMPORTANT]
> Azure kapsayıcı kayıt defteri coğrafi çoğaltma özelliği şu anda kullanımda **Önizleme**. Önizlemeler kullanılabilir hale getirilir size kabul ettiğiniz koşuluyla [ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Bu özellik bazı yönleri genel kullanılabilirlik (GA) önce değişebilir.
>

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

[Azure Portal](http://portal.azure.com) oturum açın.

Seçin **yeni** > **kapsayıcıları** > **Azure kapsayıcı kayıt defteri**.

![Azure portalında bir kapsayıcı kayıt defteri oluşturma][tut-portal-01]

Yeni kayıt defteri aşağıdaki ayarlarla yapılandırın:

* **Kayıt defteri adı**: Genel Azure içinde benzersizdir ve 5-50 alfasayısal karakterler içeren bir kayıt defteri adı oluşturma
* **Kaynak grubu**: **Yeni Oluştur** > `myResourceGroup`
* **Konum**:`West US`
* **Yönetici kullanıcı**: `Enable` (görüntüleri çekmesini kapsayıcıları için Web uygulaması için gerekli)
* **SKU**: `Premium` (coğrafi çoğaltma için gerekli)

Seçin **oluşturma** ACR örneği dağıtmak için.

![Azure portalında bir kapsayıcı kayıt defteri oluşturma][tut-portal-02]

Bu öğreticinin geri kalanını, kullandığımız `<acrName>` kapsayıcı için bir yer tutucu olarak **kayıt defteri adı** seçtiğiniz.

> [!TIP]
> Azure kapsayıcı kayıt defterleri, birden çok kapsayıcı konak genelinde kullanılan genellikle uzun süreli kaynaklar olduğundan, kendi kaynak grubunda kaydınız oluşturmanızı öneririz. Coğrafi olarak çoğaltılmış registries ve Web kancalarını yapılandırırken bu ek kaynaklar aynı kaynak grubunda yer alır.
>

## <a name="configure-geo-replication"></a>Coğrafi çoğaltmayı yapılandırma

Premium kayıt defteri sahip olduğunuza göre coğrafi çoğaltma yapılandırabilirsiniz. İki bölgede çalıştırmak için sonraki öğreticide yapılandırın, web uygulamanızı daha sonra kendi kapsayıcı görüntüleri yakın kayıt defterinden çeker.

Yeni kapsayıcı kaydınız seçin ve portal Azure gidin **çoğaltmalar** altında **Hizmetleri**:

![Azure portal kapsayıcısı kayıt defterinde UI çoğaltmalar][tut-portal-03]

Bir harita Azure bölgeleri coğrafi çoğaltma için kullanılabilir gösteren yeşil Altıgenler gösteren görüntülenir:

 ![Azure Portalı'nda Bölge eşleme][tut-map-01]

Yeşil Altıgene seçerek Doğu ABD bölgesinde, kayıt defteri çoğaltma ve ardından **oluşturma** altında **çoğaltma oluşturma**:

 ![Azure portalında çoğaltma kullanıcı Arabirimi oluşturma][tut-portal-04]

Çoğaltma tamamlandıktan sonra portal yansıtır *hazır* hem bölgeler için. Kullanım **yenileme** çoğaltma durumunu yenile düğmesine; veya bunu oluşturulan ve eşitlenen çoğaltmalar için bir dakika sürebilir.

![Azure portalında çoğaltma durumunu kullanıcı Arabirimi][tut-portal-05]

## <a name="container-registry-login"></a>Kapsayıcı kayıt defteri oturum açma

Coğrafi çoğaltma yapılandırdıktan, bir kapsayıcı yansıması oluştur ve kayıt defterine gönderme. İlk ACR örneğinizi görüntüleri göndermeden önce oturum gerekir. İle [temel, standart ve Premium SKU'ları](container-registry-skus.md), Azure kimlik bilgilerinizi kullanarak kimliğini doğrulayabilir.

Kullanım [az acr oturum açma](https://docs.microsoft.com/en-us/cli/azure/acr#az_acr_login) doğrulamak ve kayıt için kimlik bilgilerini önbelleğe komutu. Değiştir `<acrName>` önceki adımlarda oluşturduğunuz kayıt defteri adı.

```azurecli
az acr login --name <acrName>
```

Komut döndürür `Login Succeeded` tamamlandığında.

## <a name="get-application-code"></a>Uygulama kodunu alma

Bu öğreticideki örnek ile yerleşik bir küçük bir web uygulaması içeren [ASP.NET Core](http://dot.net). Uygulama görüntüyü Azure kapsayıcı kayıt defteri tarafından dağıtıldığı bölge görüntüleyen bir HTML sayfası görevi görür.

![Tarayıcıda gösterilen öğretici uygulama][tut-app-01]

Yerel bir dizine örneği indirmek için Git kullanın ve `cd` dizine:

```bash
git clone https://github.com/Azure-Samples/acr-helloworld.git
cd acr-helloworld
```

## <a name="update-dockerfile"></a>Dockerfile güncelleştir

Aşağıdaki örnekte dahil Dockerfile kapsayıcı nasıl yapılandırıldığını gösterir. Resmi başlatır [aspnetcore](https://store.docker.com/community/images/microsoft/aspnetcore) görüntü, uygulama dosyaları kapsayıcıya kopyaları bağımlılıkları yükler, resmi kullanarak çıktı derler [aspnetcore yapı](https://store.docker.com/community/images/microsoft/aspnetcore-build) görüntüsü ve son olarak, en iyi duruma getirilmiş aspnetcore görüntü oluşturur.

Dockerfile konumundadır `./AcrHelloworld/Dockerfile` kopyalanan kaynak.

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

Uygulamada *acr helloworld* görüntü kapsayıcısı dağıtıldığı kayıt defterindeki oturum açma sunucusu hakkında bilgi için DNS sorgulayarak bölge belirlemek dener. Kayıt defterindeki oturum açma sunucusu URL'sini belirtmelisiniz `DOCKER_REGISTRY` Dockerfile ortam değişkeni.

İlk olarak, kayıt defterindeki oturum açma sunucu URL'SİYLE almak `az acr show` komutu. Değiştir `<acrName>` önceki adımlarda oluşturduğunuz kayıt defteri adı.

```azurecli
az acr show --name <acrName> --query "{acrLoginServer:loginServer}" --output table
```

Çıktı:

```bash
AcrLoginServer
-----------------------------
uniqueregistryname.azurecr.io
```

Ardından, güncelleştirme `DOCKER_REGISTRY` satır, kayıt defterindeki oturum açma sunucusu URL'si. Bu örnekte, bizim örnek kayıt defteri adını gösterecek biçimde satırı güncelleştiriyoruz *uniqueregistryname*:

```dockerfile
ENV DOCKER_REGISTRY uniqueregistryname.azurecr.io
```

## <a name="build-container-image"></a>Kapsayıcı yansıması oluştur

Kayıt URL'si ile Dockerfile güncelleştirdik, kullanabileceğiniz `docker build` kapsayıcı görüntüsü oluşturulamadı. Görüntüyü oluşturun ve yeniden değiştirme URL'si özel kayıt defteri ile etiketlemek için aşağıdaki komutu çalıştırın `<acrName>` , kayıt defteri adı:

```bash
docker build . -f ./AcrHelloworld/Dockerfile -t <acrName>.azurecr.io/acr-helloworld:v1
```

(Burada kesilmiş gösterilen) Docker görüntü yerleşik gibi birkaç satırlık bir çıktı görüntülenir:

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

Kullanım `docker images` yerleşik bir görüntü görmek için komutu:

```bash
docker images
```

Çıktı:

```bash
REPOSITORY                                      TAG    IMAGE ID        CREATED               SIZE
uniqueregistryname.azurecr.io/acr-helloworld    v1     01ac48d5c8cf    About a minute ago    284MB
...
```

## <a name="push-image-to-azure-container-registry"></a>Azure kapsayıcı kayıt defterine görüntü gönderme

Son olarak, `docker push` iletmek için komutu *acr helloworld* kaydınız görüntüye. Değiştir `<acrName>` kayıt adı.

```bash
docker push <acrName>.azurecr.io/acr-helloworld:v1
```

Coğrafi çoğaltma için kayıt defterinizi yapılandırdıktan çünkü görüntünüzü otomatik olarak her ikisi de çoğaltılır *Batı ABD* ve *Doğu ABD* bu tek bölgesiyle `docker push` komutu.

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

Bu öğreticide, bir kapsayıcı görüntüsü yerleşik bir özel, coğrafi olarak çoğaltılmış kapsayıcı kayıt defteri oluşturulur ve ardından bu görüntüyü kayıt defterine gönderilir. Bu öğreticide adımları izleyerek:

> [!div class="checklist"]
> * Coğrafi olarak çoğaltılmış Azure kapsayıcı kayıt defteri oluşturuldu
> * Kopyalanan uygulama kaynak koduna github'dan
> * Uygulama kaynağı Docker kapsayıcısı görüntüden yerleşik
> * Kayıt defterine kapsayıcı görüntü gönderilir

Birden çok Web görüntüleri yerel olarak hizmet vermek için coğrafi çoğaltma'yı kullanarak uygulamaları için kapsayıcı örnekleri için kapsayıcı dağıtma hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Azure kapsayıcı kayıt defterinden Web uygulaması dağıtma](container-registry-tutorial-deploy-app.md)

<!-- IMAGES -->
[tut-portal-01]: ./media/container-registry-tutorial-prepare-registry/tut-portal-01.png
[tut-portal-02]: ./media/container-registry-tutorial-prepare-registry/tut-portal-02.png
[tut-portal-03]: ./media/container-registry-tutorial-prepare-registry/tut-portal-03.png
[tut-portal-04]: ./media/container-registry-tutorial-prepare-registry/tut-portal-04.png
[tut-portal-05]: ./media/container-registry-tutorial-prepare-registry/tut-portal-05.png
[tut-app-01]: ./media/container-registry-tutorial-prepare-registry/tut-app-01.png
[tut-map-01]: ./media/container-registry-tutorial-prepare-registry/tut-map-01.png
