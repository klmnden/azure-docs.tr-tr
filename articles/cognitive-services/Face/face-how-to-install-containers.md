---
title: Yükleme, kapsayıcıları çalıştırın
titlesuffix: Face - Azure Cognitive Services
description: İndirme, yükleme ve bu izlenecek yol öğreticide yüz için kapsayıcıları çalıştırmak nasıl.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: article
ms.date: 04/16/2019
ms.author: diberry
ms.openlocfilehash: 5028a05fe74f1d19ed5e43ac797df87bbe3382e8
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59680732"
---
# <a name="install-and-run-face-containers"></a>Yükleme ve yüz kapsayıcıları çalıştırma

Yüz tanıma, görüntülerdeki İnsan yüzlerini algılar ve yüz yer işareti (örneğin, noses ve gözler), cinsiyet, geçerlilik süresi ve diğer makine tahmin yüz özellikleri gibi öznitelikleri tanımlayan yüz tanıma, adlı Docker için standartlaştırılmış bir Linux kapsayıcı sağlar. Yüz algılama ek olarak, iki yüzün aynı görüntü ya da farklı görüntüleri bir güven puanı kullanarak aynı olduğundan veya bir benzeyen olmadığını görmek için bir veritabanında yüzleri karşılaştırın veya aynı yüz zaten kontrol edebilirsiniz. Bu gibi durumlarda, benzer yüzlerden de paylaşılan visual nitelikler kullanarak gruplar halinde düzenleyebilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Yüz tanıma API'si kapsayıcıları kullanmadan önce aşağıdaki gereksinimleri karşılaması gerekir:

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Docker Altyapısı'nın kurulu ihtiyacınız bir [ana bilgisayar](#the-host-computer). Docker üzerinde Docker ortamını yapılandıran paketler sağlar [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Docker ve kapsayıcı temelleri hakkında bilgi için bkz: [Docker'a genel bakış](https://docs.docker.com/engine/docker-overview/).<br><br> Docker, kapsayıcılar ile bağlanma ve faturalama verileri Azure'a göndermek izin verecek şekilde yapılandırılmalıdır. <br><br> **Windows üzerinde**, Docker de Linux kapsayıcıları destekler şekilde yapılandırılmalıdır.<br><br>|
|Docker ile aşinalık | Bir temel kavramlarını Docker kayıt defterleri, havuzları, kapsayıcılar ve kapsayıcı görüntülerinin yanı sıra temel bilgi gibi olmalıdır `docker` komutları.| 
|Azure `Cognitive Services` kaynak |Kapsayıcı kullanabilmeniz için şunlara sahip olmalısınız:<br><br>A _Bilişsel Hizmetler_ anahtar Azure kaynak ve ilişkili faturalama, fatura uç noktası URI'si. Her iki değer kaynağın genel bakış ve anahtarları sayfalarında kullanılabilir ve kapsayıcı başlatma için gereklidir. Eklemenize gerek `face/v1.0` BILLING_ENDPOINT_URI aşağıda gösterildiği gibi uç nokta URI'si yönlendirme. <br><br>**{BILLING_KEY}** : kaynak anahtarı<br><br>**{BILLING_ENDPOINT_URI}** : uç nokta URI'si örnektir: `https://westus.api.cognitive.microsoft.com/face/v1.0`|


## <a name="request-access-to-the-private-container-registry"></a>Özel kapsayıcı kayıt defterine erişim isteği

[!INCLUDE [Request access to private preview](../../../includes/cognitive-services-containers-request-access.md)]

### <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]


### <a name="container-requirements-and-recommendations"></a>Kapsayıcı gereksinimleri ve önerileri

Aşağıdaki tabloda, en düşük ve önerilen CPU Çekirdeği ve her bir yüz tanıma API'si kapsayıcısı için ayrılacak bellek açıklanmaktadır.

| Kapsayıcı | Minimum | Önerilen | TPS<br>(Minimum, maksimum)|
|-----------|---------|-------------|--|
|Yüz | 1 çekirdek, 2 GB bellek | 1 çekirdek, 4 GB bellek |10, 20|

* Her çekirdeğe en az 2.6 gigahertz (GHz) olması ya da daha hızlı.
* TPS - saniye başına işlem

Çekirdek ve bellek karşılık `--cpus` ve `--memory` parçası olarak kullanılan ayarları `docker run` komutu.

## <a name="get-the-container-image-with-docker-pull"></a>İle kapsayıcı görüntüsünü Al `docker pull`

Yüz tanıma API'si için kapsayıcı görüntülerini kullanılabilir. 

| Kapsayıcı | Depo |
|-----------|------------|
| Yüz | `containerpreview.azurecr.io/microsoft/cognitive-services-face:latest` |

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-face-container"></a>Docker isteği yüz kapsayıcısı için

```
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-face:latest
```

## <a name="how-to-use-the-container"></a>Kapsayıcı kullanma

Kapsayıcı açıldığında [ana bilgisayar](#the-host-computer), kapsayıcı ile çalışmak için aşağıdaki işlemi kullanın.

1. [Kapsayıcıyı çalıştırmak](#run-the-container-with-docker-run), gerekli faturalama ayarları. Daha fazla [örnekler](./face-resource-container-config.md#example-docker-run-commands) , `docker run` komutu kullanılabilir. 
1. [Kapsayıcının tahmini uç nokta sorgu](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>Kapsayıcı ile çalıştırma `docker run`

Kullanım [docker run](https://docs.docker.com/engine/reference/commandline/run/) üç kapsayıcı birini çalıştırmak için komutu. Komutu şu parametreleri kullanır:

| Yer tutucu | Değer |
|-------------|-------|
|{BILLING_KEY} | Bu anahtar kapsayıcısı başlatmak için kullanılır ve Azure'da kullanılabilir `Cognitive Services` anahtarlar sayfasında.  |
|{BILLING_ENDPOINT_URI} | Azure'da faturalandırma uç noktası URI değeri kullanılabilir `Cognitive Services` genel bakış sayfası. Örnek verilmiştir: `https://westus.api.cognitive.microsoft.com/face/v1.0`|

Eklemenize gerek `face/v1.0` önceki BILLING_ENDPOINT_URI örnekte gösterildiği gibi uç nokta URI'si yönlendirme. 

Bu parametreleri aşağıdaki örnekte kendi değerlerinizle değiştirin `docker run` komutu.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-face \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Bu komut:

* Yüz tanıma kapsayıcı kapsayıcı görüntüsünü çalıştırır.
* Bir CPU Çekirdeği ve 4 gigabayt (GB) bellek ayırır.
* 5000 numaralı TCP bağlantı noktasını kullanıma sunar ve sahte TTY için kapsayıcı ayırır.
* Bunu çıktıktan sonra kapsayıcı otomatik olarak kaldırır. Kapsayıcı görüntüsü ana bilgisayarda kullanılabilir durumda kalır. 

Daha fazla [örnekler](./face-resource-container-config.md#example-docker-run-commands) , `docker run` komutu kullanılabilir. 

> [!IMPORTANT]
> `Eula`, `Billing`, Ve `ApiKey` kapsayıcıyı çalıştırmak için seçenekler belirtilmelidir; Aksi takdirde, kapsayıcı başlatılamıyor.  Daha fazla bilgi için [faturalama](#billing).

[!INCLUDE [Running multiple containers on the same host](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]


## <a name="query-the-containers-prediction-endpoint"></a>Sorgu kapsayıcının tahmini uç noktası

Kapsayıcı, REST tabanlı sorgu tahmin uç nokta API'leri sağlar. 

Ana bilgisayarını kullanmak `https://localhost:5000`, kapsayıcı API'leri için.


<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Kapsayıcı Durdur

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcı içeren bir çıktı çalıştırırsanız [bağlama](./face-resource-container-config.md#mount-settings) ve günlüğe kaydetme etkin, kapsayıcı başlatma veya kapsayıcı çalıştırma sırasında gerçekleşen sorunları gidermek yararlı olan günlük dosyalarını oluşturur. 


## <a name="billing"></a>Faturalandırma

Azure için fatura, kullanarak yüz tanıma API'si kapsayıcıları Gönder bir _yüz tanıma API'si_ Azure hesabınız kaynaktaki. 

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Bu seçenekler hakkında daha fazla bilgi için bkz. [kapsayıcıları yapılandırma](./face-resource-container-config.md).

## <a name="summary"></a>Özet

Bu makalede, kavramlar ve indirme, yükleme ve yüz tanıma API'si kapsayıcıları çalıştırmak için iş akışı öğrendiniz. Özet:

* Yüz tanıma API'si, üç Linux kapsayıcıları için Docker, anahtar ifade ayıklama, dil algılama ve yaklaşım analizi Kapsüllenen sağlar.
* Kapsayıcı görüntülerini azure'da Microsoft kapsayıcı kayıt defteri (MCR) alanından indirilir.
* Docker kapsayıcı görüntüleri çalıştırın.
* Yüz tanıma API'si-kapsayıcılarında işlemleri ana kapsayıcısının URI belirterek çağırmak için REST API veya SDK'sını kullanabilirsiniz.
* Bir kapsayıcı örneği oluşturulurken, fatura bilgilerini belirtmeniz gerekir.

> [!IMPORTANT]
> Bilişsel hizmetler kapsayıcıları, kullanım ölçümü için Azure'a bağlanmadan çalıştırmak için lisanslanmaz. Müşteriler, her zaman faturalandırma bilgileri ölçüm hizmeti ile iletişim kurmak kapsayıcıları etkinleştirmeniz gerekiyor. Bilişsel hizmetler kapsayıcılar, Microsoft müşteri verilerini (örneğin, görüntü veya metin analiz edilen) göndermeyin.

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [kapsayıcıları yapılandırma](face-resource-container-config.md) yapılandırma ayarları
* Gözden geçirme [yüz genel bakış](Overview.md) algılama ve yüz tanımlama hakkında daha fazla bilgi edinmek için  
* Başvurmak [yüz tanıma API'si](//westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) kapsayıcı tarafından desteklenen yöntemleri hakkında ayrıntılar için.
* Başvurmak [sık sorulan sorular (SSS)](FAQ.md) yüz işlevselliği ile ilgili sorunları gidermek için.
* Daha fazla kullanmanız [Bilişsel Hizmetleri kapsayıcıları](../cognitive-services-container-support.md)
