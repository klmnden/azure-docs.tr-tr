---
title: Yükleme ve kapsayıcıları - Personalizer çalıştırın
titleSuffix: Azure Cognitive Services
description: İndirme, yükleme ve kapsayıcıları çalıştırmak için Personalizer nasıl.
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/09/2019
ms.author: edjez
ms.openlocfilehash: a197531a7c78823271c0a5fa5413b76746f63a9a
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65507025"
---
# <a name="install-and-run-personalizer-containers"></a>Yükleme ve Personalizer kapsayıcıları çalıştırma

Aşağıdaki kapsayıcı Personalizer hizmet vardır: 

|İşlev|Özellikler|
|-|-|
|Personalizer|İçeriği ve kullanıcı geçerli bağlamdan en iyi eylemi belirlemek.|

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Personalizer hizmeti kapsayıcıları kullanmadan önce aşağıdaki gereksinimleri karşılaması gerekir:

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Docker Altyapısı'nın kurulu ihtiyacınız bir [ana bilgisayar](#the-host-computer). Docker üzerinde Docker ortamını yapılandıran paketler sağlar [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Docker ve kapsayıcı temelleri hakkında bilgi için bkz: [Docker'a genel bakış](https://docs.docker.com/engine/docker-overview/).<br><br> Docker, kapsayıcılar ile bağlanma ve faturalama verileri Azure'a göndermek izin verecek şekilde yapılandırılmalıdır. <br><br> **Windows üzerinde**, Docker de Linux kapsayıcıları destekler şekilde yapılandırılmalıdır.<br><br>|
|Docker ile aşinalık | Bir temel kavramlarını Docker kayıt defterleri, havuzları, kapsayıcılar ve kapsayıcı görüntülerinin yanı sıra temel bilgi gibi olmalıdır `docker` komutları.| 
|Personalizer Hizmet kaynağı |Bu kapsayıcıların kullanabilmeniz için şunlara sahip olmalısınız:<br><br>A _Personalizer hizmet_ fatura uç noktası URI'si ve ilişkili faturalandırma anahtarı almak için Azure kaynak. Her iki değeri de Azure portalının Personalizer hizmetine genel bakış ve anahtarları sayfalarında kullanılabilir ve kapsayıcı başlatma için gereklidir.<br><br>**{BILLING_KEY}** : kaynak anahtarı<br><br>**{BILLING_ENDPOINT_URI}** : uç nokta URI'si örnektir: `https://westus.api.cognitive.microsoft.com/`|

### <a name="the-host-computer"></a>Ana bilgisayar

**Konak** , docker kapsayıcısı çalıştıran bilgisayardır. Bu, bir bilgisayara şirket içinde veya Azure dahil olmak üzere hizmeti barındıran bir docker olabilir:

* [Azure Kubernetes Service](https://docs.microsoft.com/aks/index.yml)
* [Azure Container Instances](https://docs.microsoft.com/container-instances/index.yml)
* [Kubernetes](https://kubernetes.io/) kümesi dağıtıldı için [Azure Stack](https://docs.microsoft.com/azure-stack/index.yml). Daha fazla bilgi için [Azure Stack dağıtma Kubernetes](https://docs.microsoft.com/azure-stack/user/azure-stack-solution-template-kubernetes-deploy.md).

### <a name="container-requirements-and-recommendations"></a>Kapsayıcı gereksinimleri ve önerileri

Aşağıdaki tabloda, en düşük ve önerilen CPU Çekirdeği ve her bir Personalizer hizmet kapsayıcısı için ayrılacak bellek açıklanmaktadır.

| Kapsayıcı | Minimum | Önerilen |
|-----------|---------|-------------|
|Personalizer | 1 çekirdek, 4 GB bellek | 2 Çekirdek, 8 GB bellek |

Her çekirdeğe en az 2.6 gigahertz (GHz) olması ya da daha hızlı.

Çekirdek ve bellek karşılık `--cpus` ve `--memory` parçası olarak kullanılan ayarları `docker run` komutu.

Kapsayıcıyı Azure Event Hubs'a geçiş bilgilerini azure'da Personalizer sunucusuna boyut sayısı ve ödül çağrılarındaki sırada bağlanabilmesi gerekir ve gerekli yapılandırma ve en yeni makineye yüklemek için Personalizer API için bağlanan APM olmalıdır öğrenme modelleri.

## <a name="get-the-container-image-with-docker-pull"></a>İle kapsayıcı görüntüsünü Al `docker pull`

Kapsayıcı görüntülerini Personalizer hizmeti için kullanılabilir. 

| Kapsayıcı | Depo |
|-----------|------------|
| Personalizer | `mcr.microsoft.com/azure-cognitive-services/personalizer:latest` |

> [!TIP]
> Kullanabileceğiniz [docker görüntüleri](https://docs.docker.com/engine/reference/commandline/images/) indirilen kapsayıcı görüntülerinizi listelemek için komutu. Örneğin, aşağıdaki komut kimliği, havuz ve tablo olarak biçimlendirilmiş her indirilen kapsayıcı görüntüsünün etiketi listeler:
>
>  ```Docker
>  docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
>
>  IMAGE ID            REPOSITORY                                                                TAG
>  ebbee78a6baa        mcr.microsoft.com/azure-cognitive-services/personalizer                 latest
>  ``` 

### <a name="docker-pull-for-the-personalizer-service-container"></a>Docker isteği Personalizer hizmet kapsayıcısı için

```Docker
docker pull mcr.microsoft.com/azure-cognitive-services/personalizer:latest
```

## <a name="how-the-container-works"></a>Kapsayıcı nasıl çalışır?

Personalizer kapsayıcı Personalizer hizmeti istemci bölümünü temsil eder. Kapsayıcı çalıştığında, bu yapılandırma ayarlarını ve modelleri Personalizer hizmetinden Azure bulutunda alır. Kapsayıcı hizmeti, istekleri yanıtlamak için bu bilgileri kullanır. **derece** ve **ödül**. Kapsayıcı Azure bulutunda Personalizer kaynak aynı zamanda bu istekler gönderir. Bu bilgiler, sonra modeli güncelleştirme sıklığı geçerli ayar temel alınarak Personalizer döngü modeli eğitmek için kullanılır. Güncelleştirilmiş model bir kapsayıcıya geri gönderilir. 

![Şirket içi, yerel Personalizer kapsayıcının istemci mimarisi](./media/personalizer-container-howto/personalizer-container-architecture.png)

## <a name="how-to-use-the-container"></a>Kapsayıcı kullanma

Kapsayıcı açıldığında [ana bilgisayar](#the-host-computer), kapsayıcı ile çalışmak için aşağıdaki işlemi kullanın.

1. [Kapsayıcıyı çalıştırmak](#run-the-container-with-docker-run), gerekli faturalama ayarları. Daha fazla [örnekler](personalizer-container-configuration.md#example-docker-run-commands) , `docker run` komutu kullanılabilir. 
1. [Kapsayıcının tahmini uç nokta sorgu](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>Kapsayıcı ile çalıştırma `docker run`

Kullanım [docker run](https://docs.docker.com/engine/reference/commandline/run/) üç kapsayıcı birini çalıştırmak için komutu. Komutu şu parametreleri kullanır:

| Yer tutucu | Value |
|-------------|-------|
|{BILLING_KEY} | Bu anahtar kapsayıcısı başlatmak için kullanılır ve Azure portalının Personalizer hizmet anahtarları sayfasında bulabilirsiniz.  |
|{BILLING_ENDPOINT_URI} | Fatura uç noktası URI değerini Azure portalının Personalizer hizmetine genel bakış sayfasında kullanılabilir.|

Bu parametreleri aşağıdaki örnekte kendi değerlerinizle değiştirin `docker run` komutu.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/personalizer\
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Bu komut:

* Kapsayıcı görüntüsünü Personalizer Service kapsayıcısında çalışır
* Bir CPU Çekirdeği ve 4 gigabayt (GB) bellek ayırır.
* 5000 numaralı TCP bağlantı noktasını kullanıma sunar ve sahte TTY için kapsayıcı ayırır.
* Bunu çıktıktan sonra kapsayıcı otomatik olarak kaldırır. Kapsayıcı görüntüsü ana bilgisayarda kullanılabilir durumda kalır. 

> [!IMPORTANT]
> `Eula`, `Billing`, Ve `ApiKey` kapsayıcıyı çalıştırmak için seçenekler belirtilmelidir; Aksi takdirde, kapsayıcı başlatılamıyor.  Daha fazla bilgi için [faturalama](#billing).

### <a name="run-multiple-containers-on-the-same-host"></a>Aynı ana bilgisayarda birden çok kapsayıcı çalıştırma

Kullanıma sunulan bağlantı noktası olan birden çok kapsayıcı çalıştırmak istiyorsanız, her kapsayıcı farklı bir bağlantı noktası ile çalıştırmak emin olun. Örneğin, ilk kapsayıcı bağlantı noktası 5000 ve ikinci kapsayıcıyı 5001 bağlantı noktası üzerinde çalıştırın.

Değiştirin `<container-registry>` ve `<container-name>` kullandığınız kapsayıcıları değerleri. Bunlar aynı kapsayıcı olması gerekmez. Personalizer kapsayıcısı ve LUIS kapsayıcı birlikte çalışıyor olabilir veya çalışan birden çok Personalizer kapsayıcılar sahip olabilir. 

İlk kapsayıcı 5000 numaralı çalıştırın. 

```bash 
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

İkinci kapsayıcıyı 5001 bağlantı noktası üzerinde çalıştırın.


```bash 
docker run --rm -it -p 5000:5001 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Sonraki her kapsayıcı farklı bir bağlantı noktası olmalıdır. 

## <a name="query-the-containers-prediction-endpoint"></a>Sorgu kapsayıcının tahmini uç noktası

Kapsayıcı, REST tabanlı sorgu tahmin uç nokta API'leri sağlar. 

Ana bilgisayarını kullanmak https://localhost:5000, kapsayıcı API'leri için.

## <a name="stop-the-container"></a>Kapsayıcı Durdur

Burada kapsayıcı çalışıyor, komut satırı ortamında kapsayıcı kapatmak için basın **Ctrl + C**.

## <a name="troubleshoot"></a>Sorun gider

Kapsayıcı içeren bir çıktı çalıştırırsanız [bağlama](personalizer-container-configuration.md#mount-settings) ve günlüğe kaydetme etkin, kapsayıcı başlatma veya kapsayıcı çalıştırma sırasında gerçekleşen sorunları gidermek yararlı olan günlük dosyalarını oluşturur. 

## <a name="container-api-documentation"></a>Kapsayıcı API belgeleri

Kapsayıcı uç noktaları için belgeleri tam bir dizi sağlar hem de bir `Try it now` özelliği. Bu özellik, web tabanlı bir HTML formuna ayarlarınızı girdikten ve herhangi bir kod yazmak zorunda kalmadan sorgu yapmanıza olanak tanır. Sorgunun döndürdüğü CURL komutu bir örnek sağlanır sonra HTTP üst bilgilerini gösterme ve gerekli biçim gövde. 

> [!TIP]
> Okuma [Openapı belirtimi](https://swagger.io/docs/specification/about/), gelen kapsayıcı tarafından desteklenen API işlemleri açıklayan `/swagger` göreli URI'si. Örneğin:
>
>  ```http
>  http://localhost:5000/swagger
>  ```

## <a name="billing"></a>Faturalama

Azure için fatura, kullanarak Personalizer hizmeti kapsayıcıları Gönder bir _Personalizer hizmet_ Azure hesabınız kaynaktaki. 

Bilişsel hizmetler kapsayıcıları, kullanım ölçümü için Azure'a bağlanmadan çalıştırmak için lisanslanmaz. Müşteriler, her zaman faturalandırma bilgileri ölçüm hizmeti ile iletişim kurmak kapsayıcıları etkinleştirmeniz gerekiyor.  

`docker run` Komutu, faturalama amacıyla aşağıdaki değişkenleri kullanır:

| Seçenek | Açıklama |
|--------|-------------|
| `ApiKey` | API anahtarı _Personalizer hizmet_ kaynak faturalandırma bilgileri izlemek için kullanılır. |
| `Billing` | Uç noktası _Personalizer hizmet_ kaynak faturalandırma bilgileri izlemek için kullanılır.|
| `Eula` | Kapsayıcı lisansını kabul ettiğinizi gösterir.<br/>Bu seçenek değeri ayarlanmalıdır `accept`. |

> [!IMPORTANT]
> Seçeneklerin üçünü geçerli değerlerle belirtilmiş olmalı veya kapsayıcı başlatılamıyor.

Bu seçenekler hakkında daha fazla bilgi için bkz. [kapsayıcıları yapılandırma](personalizer-container-configuration.md).

## <a name="summary"></a>Özet

Bu makalede, kavramlar ve indirme, yükleme ve Personalizer hizmeti kapsayıcıları çalıştırmak için iş akışı öğrendiniz. Özet:

* Personalizer Linux kapsayıcıları için Docker, kişiselleştirme Kapsüllenen hizmetidir.
* Kapsayıcı görüntülerini azure'da Microsoft kapsayıcı kayıt defteri (MCR) alanından indirilir.
* Docker kapsayıcı görüntüleri çalıştırın.
* Ana kapsayıcısının URI belirterek Personalizer hizmet kapsayıcılarında işlemleri çağırmak için REST API veya SDK'sını kullanabilirsiniz.
* Bir kapsayıcı örneği oluşturulurken, fatura bilgilerini belirtmeniz gerekir.

> [!IMPORTANT]
> Bilişsel hizmetler kapsayıcıları, kullanım ölçümü için Azure'a bağlanmadan çalıştırmak için lisanslanmaz. Müşteriler, her zaman faturalandırma bilgileri ölçüm hizmeti ile iletişim kurmak kapsayıcıları etkinleştirmeniz gerekiyor. Kapsayıcılar ayrıca istek verileri azure'da karşılık gelen hizmet çevrimiçi eğitim amaçları için gönderir ve alırsınız Personalizer Azure düzenli aralıklarla modellerinden güncelleştirildi.

## <a name="next-steps"></a>Sonraki adımlar

[Docker kapsayıcısını yapılandırma `Docker Run` komutu](personalizer-container-configuration.md)