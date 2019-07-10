---
title: Kapsayıcıları yükleme ve çalıştırma
titlesuffix: Face - Azure Cognitive Services
description: İndirin, yükleyin ve bu izlenecek yol öğreticide yüz için kapsayıcıları çalıştırın.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: dapine
ms.openlocfilehash: 84960e82e25f4b6cc59324f17ce46de7f9f7ac23
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67704680"
---
# <a name="install-and-run-face-containers"></a>Yükleme ve yüz kapsayıcıları çalıştırma

Azure Bilişsel hizmetler yüz tanıma, görüntülerdeki İnsan yüzlerini algılar Docker için standartlaştırılmış bir Linux kapsayıcı sağlar. Ayrıca, yüz tanıma yer işareti noses ve gözler, cinsiyet, geçerlilik süresi ve diğer makine tahmin yüz özellikleri gibi içeren öznitelikleri tanımlar. Algılama yanı sıra, yüz tanıma aynı görüntü ya da farklı görüntüleri iki yüzün aynı bir güven puanı kullanarak olup olmadığını kontrol edebilirsiniz. Yüz tanıma, yüz benzeyen veya benzer bir yüz zaten mevcut olup olmadığını görmek için bir veritabanında de karşılaştırabilirsiniz. Bu da benzer yüzlerden, paylaşılan visual nitelikler kullanarak gruplar halinde düzenleyebilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Yüz tanıma API'si kapsayıcıları kullanmadan önce aşağıdaki önkoşulları karşılamalıdır.

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Docker altyapısı yüklü olmalıdır bir [ana bilgisayar](#the-host-computer). Docker üzerinde Docker ortamını yapılandıran paketler sağlar [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Docker ve kapsayıcı temelleri hakkında bilgi için bkz: [Docker'a genel bakış](https://docs.docker.com/engine/docker-overview/).<br><br> Docker, kapsayıcılar ile bağlanma ve faturalama verileri Azure'a göndermek izin verecek şekilde yapılandırılmalıdır. <br><br> Windows Docker Linux kapsayıcıları desteklemek için de yapılandırılması gerekir.<br><br>|
|Docker ile aşinalık | Bir temel kavramlarını Docker kayıt defterleri, havuzları, kapsayıcılar ve kapsayıcı görüntüleri gibi ihtiyacınız vardır. Ayrıca temel bilgisine ihtiyacınız `docker` komutları.| 
|Azure `Cognitive Services` kaynak |Kapsayıcı kullanmak için şunlara sahip olmalısınız:<br><br>Bir Azure Bilişsel hizmetler kaynağı ve ilişkili faturalama anahtarının ve fatura uç noktası URI'si. Her iki değer mevcuttur **genel bakış** ve **anahtarları** kaynağın sayfaları. Kapsayıcı başlatın isteniyor. Ekleme `face/v1.0` BILLING_ENDPOINT_URI aşağıda gösterildiği gibi URI, uç noktasına yönlendirme: <br><br>**{BILLING_KEY}** : kaynak anahtarı<br><br>**{BILLING_ENDPOINT_URI}** : uç nokta URI'si örneğidir `https://westus.api.cognitive.microsoft.com/face/v1.0`|

## <a name="request-access-to-the-private-container-registry"></a>Özel kapsayıcı kayıt defterine erişim isteği

[!INCLUDE [Request access to private container registry](../../../includes/cognitive-services-containers-request-access.md)]

### <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Kapsayıcı gereksinimleri ve önerileri

Aşağıdaki tabloda, en düşük ve önerilen CPU Çekirdeği ve her bir yüz tanıma API'si kapsayıcısı için ayrılacak bellek açıklanmaktadır.

| Kapsayıcı | Minimum | Önerilen | Saniye başına işlem<br>(Minimum, maksimum)|
|-----------|---------|-------------|--|
|Yüz | 1 çekirdek, 2 GB bellek | 1 çekirdek, 4 GB bellek |10, 20|

* Her çekirdeğe en az 2,6 GHz olmalıdır veya daha hızlı.
* Saniye başına işlem sayısı (TPS).

Çekirdek ve bellek karşılık `--cpus` ve `--memory` parçası olarak kullanılan ayarları `docker run` komutu.

## <a name="get-the-container-image-with-docker-pull"></a>Docker isteği ile kapsayıcı görüntüsünü Al

Yüz tanıma API'si için kapsayıcı görüntülerini kullanılabilir. 

| Kapsayıcı | Havuz |
|-----------|------------|
| Yüz | `containerpreview.azurecr.io/microsoft/cognitive-services-face:latest` |

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-face-container"></a>Docker isteği yüz kapsayıcısı için

```
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-face:latest
```

## <a name="use-the-container"></a>Kapsayıcı kullanın

Kapsayıcı üzerinde sonra [ana bilgisayar](#the-host-computer), kapsayıcı ile çalışmak için aşağıdaki işlemi kullanın.

1. [Kapsayıcıyı çalıştırmak](#run-the-container-with-docker-run) gerekli faturalama ayarları. Daha fazla [örnekler](./face-resource-container-config.md#example-docker-run-commands) , `docker run` komutu kullanılabilir. 
1. [Kapsayıcının tahmini uç nokta sorgu](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>Kapsayıcı ile docker run çalıştırın

Kullanım [docker run](https://docs.docker.com/engine/reference/commandline/run/) üç kapsayıcı birini çalıştırmak için komutu. Komutu şu parametreleri kullanır.

| Yer tutucu | Değer |
|-------------|-------|
|{BILLING_KEY} | Bu anahtar kapsayıcısı başlatmak için kullanılır ve Azure'da kullanılabilir `Cognitive Services` **anahtarları** sayfası. |
|{BILLING_ENDPOINT_URI} | Azure'da faturalandırma uç noktası URI değeri kullanılabilir `Cognitive Services` **genel bakış** sayfası. `https://westus.api.cognitive.microsoft.com/face/v1.0` bunun bir örneğidir.|

Ekleme `face/v1.0` önceki BILLING_ENDPOINT_URI örnekte gösterildiği gibi URI, uç noktasına yönlendirme. 

Bu parametreleri aşağıdaki kendi değerlerinizle değiştirin `docker run` komut örneği:

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-face \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Bu komut:

* Yüz tanıma kapsayıcı kapsayıcı görüntüsünü çalıştırır.
* Bir CPU Çekirdeği ve 4 GB bellek ayırır.
* 5000 numaralı TCP bağlantı noktasını kullanıma sunar ve kapsayıcı için bir sözde TTY ayırır.
* Bunu çıktıktan sonra kapsayıcı otomatik olarak kaldırır. Kapsayıcı görüntüsü ana bilgisayarda kullanılabilir durumda kalır. 

Daha fazla [örnekler](./face-resource-container-config.md#example-docker-run-commands) , `docker run` komutu kullanılabilir. 

> [!IMPORTANT]
> `Eula`, `Billing`, Ve `ApiKey` seçenekleri belirtilen, kapsayıcıyı çalıştırmak için veya kapsayıcı başlatılamıyor. Daha fazla bilgi için [faturalama](#billing).

[!INCLUDE [Running multiple containers on the same host](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]


## <a name="query-the-containers-prediction-endpoint"></a>Sorgu kapsayıcının tahmini uç noktası

Kapsayıcı, REST tabanlı sorgu tahmin uç nokta API'leri sağlar. 

Ana bilgisayarını kullanmak `https://localhost:5000`, kapsayıcı API'leri için.


<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Kapsayıcı Durdur

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcı içeren bir çıktı çalıştırırsanız [bağlama](./face-resource-container-config.md#mount-settings) ve günlüğe kaydetme etkinleştirildiğinde, başlatmak veya kapsayıcı çalıştırma sırasında gerçekleşen sorunları gidermek yararlı olan günlük dosyaları kapsayıcı oluşturur. 


## <a name="billing"></a>Faturalandırma

Yüz tanıma API'si kapsayıcıları faturalandırma bilgileri, Azure hesabınızda bir yüz tanıma API'si kaynak'ı kullanarak Azure'a gönderin. 

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Bu seçenekler hakkında daha fazla bilgi için bkz. [kapsayıcıları yapılandırma](./face-resource-container-config.md).

<!--blogs/samples/video coures -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Özet

Bu makalede, kavramlar ve indirme, yükleme ve yüz tanıma API'si kapsayıcıları çalıştırmak için iş akışı öğrendiniz. Özet:

* Yüz tanıma API'si üç Linux kapsayıcıları için Docker anahtar ifade ayıklama, dil algılama ve yaklaşım analizi sağlar.
* Kapsayıcı görüntülerini Azure Container Registry'den indirilir.
* Docker kapsayıcı görüntüleri çalıştırın.
* Yüz tanıma API'si-kapsayıcılarında işlemleri ana kapsayıcısının URI belirterek çağırmak için REST API veya SDK'sını kullanabilirsiniz.
* Bir kapsayıcı örneği, faturalandırma bilgileri belirtmeniz gerekir.

> [!IMPORTANT]
> Bilişsel hizmetler kapsayıcıları kullanım ölçümü için Azure'a bağlanmadan çalıştırmak için lisanslı değil. Müşteriler, her zaman faturalandırma bilgileri ölçüm hizmeti ile iletişim kurmak kapsayıcıları etkinleştirmeniz gerekir. Bilişsel hizmetler kapsayıcıları analiz ediliyor, metin ve görüntü gibi müşteri verilerini Microsoft'a gönderme.

## <a name="next-steps"></a>Sonraki adımlar

* Yapılandırma ayarları için bkz. [kapsayıcıları yapılandırma](face-resource-container-config.md).
* Algılayıp yüzleri tanımlamak hakkında daha fazla bilgi için bkz: [yüz genel bakış](Overview.md).
* Kapsayıcı tarafından desteklenen yöntemleri hakkında daha fazla bilgi için bkz: [yüz tanıma API'si](//westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).
* Daha fazla Bilişsel hizmetler kapsayıcı kullanmak için bkz: [Bilişsel hizmetler kapsayıcıları](../cognitive-services-container-support.md).
