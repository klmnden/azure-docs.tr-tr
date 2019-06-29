---
title: Kapsayıcıları yükleme ve çalıştırma
titleSuffix: Text Analytics -  Azure Cognitive Services
description: İndirme, yükleme ve bu izlenecek yol öğreticide metin analizi için kapsayıcıları çalıştırmak nasıl.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.author: dapine
ms.openlocfilehash: c4ef58f35b3d038f360ff962c70e92711bc205ce
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446517"
---
# <a name="install-and-run-text-analytics-containers"></a>Yükleme ve metin analizi kapsayıcıları çalıştırma

Metin analizi kapsayıcılar sağlar ham metin üzerinde Gelişmiş doğal dil işleme ve üç ana işlev içerir: yaklaşım analizi, anahtar ifade ayıklama ve dil algılama. Varlık bağlama, bir kapsayıcıda şu anda desteklenmiyor.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Herhangi bir metin analizi kapsayıcıları çalıştırmak için ana bilgisayar ve kapsayıcı ortamlarını olması gerekir.

## <a name="preparation"></a>Hazırlık

Metin analizi kapsayıcıları kullanmadan önce aşağıdaki gereksinimleri karşılaması gerekir:

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Docker Altyapısı'nın kurulu ihtiyacınız bir [ana bilgisayar](#the-host-computer). Docker üzerinde Docker ortamını yapılandıran paketler sağlar [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Docker ve kapsayıcı temelleri hakkında bilgi için bkz: [Docker'a genel bakış](https://docs.docker.com/engine/docker-overview/).<br><br> Docker, kapsayıcılar ile bağlanma ve faturalama verileri Azure'a göndermek izin verecek şekilde yapılandırılmalıdır. <br><br> **Windows üzerinde**, Docker de Linux kapsayıcıları destekler şekilde yapılandırılmalıdır.<br><br>|
|Docker ile aşinalık | Bir temel kavramlarını Docker kayıt defterleri, havuzları, kapsayıcılar ve kapsayıcı görüntülerinin yanı sıra temel bilgi gibi olmalıdır `docker` komutları.| 
|`Cognitive Services` Kaynak |Kapsayıcı kullanabilmeniz için şunlara sahip olmalısınız:<br><br>A [ _Bilişsel Hizmetler_ ](text-analytics-how-to-access-key.md) fatura uç noktası URI'si ve ilişkili faturalandırma anahtarı almak için Azure kaynak. Her iki değeri de Azure portalında Bilişsel hizmetleri genel bakış ve anahtarları sayfalarında kullanılabilir ve kapsayıcı başlatma için gereklidir. Eklemenize gerek `text/analytics/v2.0` BILLING_ENDPOINT_URI aşağıda gösterildiği gibi uç nokta URI'si yönlendirme.<br><br>**{BILLING_KEY}** : kaynak anahtarı<br><br>**{BILLING_ENDPOINT_URI}** : uç nokta URI'si örnektir: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1`|

### <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Host Computer requirements](../../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Kapsayıcı gereksinimleri ve önerileri

Aşağıdaki tabloda açıklanmıştır en düşük ve önerilen CPU çekirdekleri, en az 2.6 gigahertz (GHz) veya daha hızlı ve her bir metin analizi kapsayıcısı için ayrılacak gigabayt (GB) bellek.

| Kapsayıcı | Minimum | Önerilen | TPS<br>(Minimum, maksimum)|
|-----------|---------|-------------|--|
|Anahtar İfade Ayıklama | 1 çekirdek, 2 GB bellek | 1 çekirdek, 4 GB bellek |15, 30|
|Dil Algılama | 1 çekirdek, 2 GB bellek | 1 çekirdek, 4 GB bellek |15, 30|
|Duygu Analizi | 1 çekirdek, 2 GB bellek | 1 çekirdek, 4 GB bellek |15, 30|

* Her çekirdeğe en az 2.6 gigahertz (GHz) olması ya da daha hızlı.
* TPS - saniye başına işlem

Çekirdek ve bellek karşılık `--cpus` ve `--memory` parçası olarak kullanılan ayarları `docker run` komutu.

## <a name="get-the-container-image-with-docker-pull"></a>İle kapsayıcı görüntüsünü Al `docker pull`

Microsoft kapsayıcı kayıt defterinden kapsayıcı görüntülerini metin analizi için kullanılabilir. 

| Kapsayıcı | Havuz |
|-----------|------------|
|Anahtar İfade Ayıklama | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
|Dil Algılama | `mcr.microsoft.com/azure-cognitive-services/language` |
|Duygu Analizi | `mcr.microsoft.com/azure-cognitive-services/sentiment` |

Kullanım [ `docker pull` ](https://docs.docker.com/engine/reference/commandline/pull/) Microsoft kapsayıcı kayıt defterinden bir kapsayıcı görüntüsü indirilemedi komutu.

Metin analizi kapsayıcılar için kullanılabilir etiketler tam bir açıklaması için aşağıdaki kapsayıcıların Docker Hub bakın:

* [Anahtar ifade ayıklama](https://go.microsoft.com/fwlink/?linkid=2018757)
* [Dil algılama](https://go.microsoft.com/fwlink/?linkid=2018759)
* [Yaklaşım analizi](https://go.microsoft.com/fwlink/?linkid=2018654)

Kullanım [ `docker pull` ](https://docs.docker.com/engine/reference/commandline/pull/) komutu, kapsayıcı görüntüsü indirilemedi.


### <a name="docker-pull-for-the-key-phrase-extraction-container"></a>Docker isteği için anahtar tümcecik ayıklama kapsayıcısı

```
docker pull mcr.microsoft.com/azure-cognitive-services/keyphrase:latest
```

### <a name="docker-pull-for-the-language-detection-container"></a>Dil algılama kapsayıcısı için docker isteği

```
docker pull mcr.microsoft.com/azure-cognitive-services/language:latest
```

### <a name="docker-pull-for-the-sentiment-container"></a>Docker isteği yaklaşım kapsayıcısı için

```
docker pull mcr.microsoft.com/azure-cognitive-services/sentiment:latest
```

[!INCLUDE [Tip for using docker list](../../../../includes/cognitive-services-containers-docker-list-tip.md)]


## <a name="how-to-use-the-container"></a>Kapsayıcı kullanma

Kapsayıcı açıldığında [ana bilgisayar](#the-host-computer), kapsayıcı ile çalışmak için aşağıdaki işlemi kullanın.

1. [Kapsayıcıyı çalıştırmak](#run-the-container-with-docker-run), gerekli faturalama ayarları. Daha fazla [örnekler](../text-analytics-resource-container-config.md#example-docker-run-commands) , `docker run` komutu kullanılabilir. 
1. [Kapsayıcının tahmini uç nokta sorgu](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>Kapsayıcı ile çalıştırma `docker run`

Kullanım [docker run](https://docs.docker.com/engine/reference/commandline/run/) üç kapsayıcı birini çalıştırmak için komutu. Komutu şu parametreleri kullanır:

| Yer tutucu | Değer |
|-------------|-------|
|{BILLING_KEY} | Bu anahtar kapsayıcısı başlatmak için kullanılır ve Azure portal üzerinde kullanılabilir `Cognitive Services` anahtarlar sayfasında.  |
|{BILLING_ENDPOINT_URI} | Azure'da faturalandırma uç noktası URI değeri kullanılabilir `Cognitive Services` genel bakış sayfası. <br><br>Örnek:<br>`Billing=https://westus.api.cognitive.microsoft.com/text/analytics/v2.0`|

Eklemenize gerek `text/analytics/v2.0` önceki BILLING_ENDPOINT_URI örnekte gösterildiği gibi uç nokta URI'si yönlendirme.

Bu parametreleri aşağıdaki örnekte kendi değerlerinizle değiştirin `docker run` komutu.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/keyphrase \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Bu komut:

* Bir anahtar tümcecik kapsayıcı kapsayıcı görüntüsünü çalıştırır.
* Bir CPU Çekirdeği ve 4 gigabayt (GB) bellek ayırır.
* 5000 numaralı TCP bağlantı noktasını kullanıma sunar ve sahte TTY için kapsayıcı ayırır.
* Bunu çıktıktan sonra kapsayıcı otomatik olarak kaldırır. Kapsayıcı görüntüsü ana bilgisayarda kullanılabilir durumda kalır. 

Daha fazla [örnekler](../text-analytics-resource-container-config.md#example-docker-run-commands) , `docker run` komutu kullanılabilir. 

> [!IMPORTANT]
> `Eula`, `Billing`, Ve `ApiKey` kapsayıcıyı çalıştırmak için seçenekler belirtilmelidir; Aksi takdirde, kapsayıcı başlatılamıyor.  Daha fazla bilgi için [faturalama](#billing).

[!INCLUDE [Running multiple containers on the same host](../../../../includes/cognitive-services-containers-run-multiple-same-host.md)]

## <a name="query-the-containers-prediction-endpoint"></a>Sorgu kapsayıcının tahmini uç noktası

Kapsayıcı, REST tabanlı sorgu tahmin uç nokta API'leri sağlar. 

Ana bilgisayarını kullanmak `https://localhost:5000`, kapsayıcı API'leri için.

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Kapsayıcı Durdur

[!INCLUDE [How to stop the container](../../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcı içeren bir çıktı çalıştırırsanız [bağlama](../text-analytics-resource-container-config.md#mount-settings) ve günlüğe kaydetme etkin, kapsayıcı başlatma veya kapsayıcı çalıştırma sırasında gerçekleşen sorunları gidermek yararlı olan günlük dosyalarını oluşturur. 

## <a name="billing"></a>Faturalandırma

Azure için fatura, kullanarak metin analizi kapsayıcıları Gönder bir _Bilişsel Hizmetler_ Azure hesabınız kaynaktaki. 

[!INCLUDE [Container's Billing Settings](../../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Bu seçenekler hakkında daha fazla bilgi için bkz. [kapsayıcıları yapılandırma](../text-analytics-resource-container-config.md).

<!--blogs/samples/video coures -->

[!INCLUDE [Discoverability of more container information](../../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Özet

Bu makalede, kavramlar ve indirme, yükleme ve metin analizi kapsayıcıları çalıştırmak için iş akışı öğrendiniz. Özet:

* Metin analizi, anahtar ifade ayıklama, dil algılama ve yaklaşım analizi Kapsüllenen üç Linux kapsayıcıları için Docker, sağlar.
* Kapsayıcı görüntülerini azure'da Microsoft kapsayıcı kayıt defteri (MCR) alanından indirilir.
* Docker kapsayıcı görüntüleri çalıştırın.
* Metin analizi-kapsayıcılarında işlemleri ana kapsayıcısının URI belirterek çağırmak için REST API veya SDK'sını kullanabilirsiniz.
* Bir kapsayıcı örneği oluşturulurken, fatura bilgilerini belirtmeniz gerekir.

> [!IMPORTANT]
> Bilişsel hizmetler kapsayıcıları, kullanım ölçümü için Azure'a bağlanmadan çalıştırmak için lisanslanmaz. Müşteriler, her zaman faturalandırma bilgileri ölçüm hizmeti ile iletişim kurmak kapsayıcıları etkinleştirmeniz gerekiyor. Bilişsel hizmetler kapsayıcılar, Microsoft müşteri verilerini (örneğin, görüntü veya metin analiz edilen) göndermeyin.

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [kapsayıcıları yapılandırma](../text-analytics-resource-container-config.md) yapılandırma ayarları
* Başvurmak [sık sorulan sorular (SSS)](../text-analytics-resource-faq.md) işlevselliği ile ilgili sorunları gidermek için.

