---
title: Yükleme ve kapsayıcıları - görüntü işleme çalıştırın
titlesuffix: Azure Cognitive Services
description: İndirme, yükleme ve bu izlenecek yol öğreticide görüntü işleme için kapsayıcıları çalıştırmak nasıl.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: article
ms.date: 05/28/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 10e5060c06e1ebc591c7245ae588b5352a3328ca
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66302841"
---
# <a name="install-and-run-recognize-text-containers"></a>Yükleme ve metni tanı kapsayıcıları çalıştırma

Görüntü işleme metni tanı kısmı, bir Docker kapsayıcısı da sunulur. Algılayıp farklı yüzey ve arka planlar, giriş ve posterler kartvizitler gibi çeşitli nesne görüntülerdeki yazılı metni Ayıkla verir.  
> [!IMPORTANT]
> Metni Tanı kapsayıcı şu anda yalnızca İngilizce ile çalışır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Metni Tanı kapsayıcıları kullanmadan önce aşağıdaki gereksinimleri karşılaması gerekir:

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Docker Altyapısı'nın kurulu ihtiyacınız bir [ana bilgisayar](#the-host-computer). Docker üzerinde Docker ortamını yapılandıran paketler sağlar [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Docker ve kapsayıcı temelleri hakkında bilgi için bkz: [Docker'a genel bakış](https://docs.docker.com/engine/docker-overview/).<br><br> Docker, kapsayıcılar ile bağlanma ve faturalama verileri Azure'a göndermek izin verecek şekilde yapılandırılmalıdır. <br><br> **Windows üzerinde**, Docker de Linux kapsayıcıları destekler şekilde yapılandırılmalıdır.<br><br>|
|Docker ile aşinalık | Bir temel kavramlarını Docker kayıt defterleri, havuzları, kapsayıcılar ve kapsayıcı görüntülerinin yanı sıra temel bilgi gibi olmalıdır `docker` komutları.| 
|Azure `Cognitive Services` kaynak |Kapsayıcı kullanabilmeniz için şunlara sahip olmalısınız:<br><br>A _Bilişsel Hizmetler_ anahtar Azure kaynak ve ilişkili faturalama, fatura uç noktası URI'si. Her iki değer kaynağın genel bakış ve anahtarları sayfalarında kullanılabilir ve kapsayıcı başlatma için gereklidir. Eklemenize gerek `vision/v2.0` BILLING_ENDPOINT_URI aşağıda gösterildiği gibi uç nokta URI'si yönlendirme. <br><br>**{BILLING_KEY}** : kaynak anahtarı<br><br>**{BILLING_ENDPOINT_URI}** : uç nokta URI'si örnektir: `https://westus.api.cognitive.microsoft.com/vision/v2.0`|


## <a name="request-access-to-the-private-container-registry"></a>Özel kapsayıcı kayıt defterine erişim isteği

[!INCLUDE [Request access to private preview](../../../includes/cognitive-services-containers-request-access.md)]

### <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]


### <a name="container-requirements-and-recommendations"></a>Kapsayıcı gereksinimleri ve önerileri

Aşağıdaki tabloda, en düşük ve önerilen CPU Çekirdeği ve her bir metin tanıma kapsayıcısı için ayrılacak bellek açıklanmaktadır.

| Kapsayıcı | Minimum | Önerilen |TPS<br>(Minimum, maksimum)|
|-----------|---------|-------------|--|
|Metin tanıma|1 çekirdek, 8 GB bellek, 0,5 TPS|2 Çekirdek, 8 GB bellek, 1 TPS|0.5, 1|

* Her çekirdeğe en az 2.6 gigahertz (GHz) olması ya da daha hızlı.
* TPS - saniye başına işlem

Çekirdek ve bellek karşılık `--cpus` ve `--memory` parçası olarak kullanılan ayarları `docker run` komutu.

## <a name="get-the-container-image-with-docker-pull"></a>İle kapsayıcı görüntüsünü Al `docker pull`

Kapsayıcı görüntülerini metin tanımak için kullanılabilir. 

| Kapsayıcı | Depo |
|-----------|------------|
|Metin tanıma | `containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:latest` |

Kullanım [ `docker pull` ](https://docs.docker.com/engine/reference/commandline/pull/) komutu, kapsayıcı görüntüsü indirilemedi.


### <a name="docker-pull-for-the-recognize-text-container"></a>Docker isteği metni tanı kapsayıcısı için

```
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:latest
```

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

## <a name="how-to-use-the-container"></a>Kapsayıcı kullanma

Kapsayıcı açıldığında [ana bilgisayar](#the-host-computer), kapsayıcı ile çalışmak için aşağıdaki işlemi kullanın.

1. [Kapsayıcıyı çalıştırmak](#run-the-container-with-docker-run), gerekli faturalama ayarları. Daha fazla [örnekler](computer-vision-resource-container-config.md) , `docker run` komutu kullanılabilir. 
1. [Kapsayıcının tahmini uç nokta sorgu](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>Kapsayıcı ile çalıştırma `docker run`

Kullanım [docker run](https://docs.docker.com/engine/reference/commandline/run/) kapsayıcıyı çalıştırmak için komutu. Komutu şu parametreleri kullanır:

| Yer tutucu | Değer |
|-------------|-------|
|{BILLING_KEY} | Bu anahtar kapsayıcısı başlatmak için kullanılır ve Azure'da kullanılabilir `Cognitive Services` anahtarlar sayfasında.  |
|{BILLING_ENDPOINT_URI} | Fatura uç noktası URI değeri. Örnek verilmiştir: `https://westus.api.cognitive.microsoft.com/vision/v2.0`|

Eklemenize gerek `vision/v2.0` BILLING_ENDPOINT_URI aşağıda gösterildiği gibi uç nokta URI'si yönlendirme.

Bu parametreleri aşağıdaki örnekte kendi değerlerinizle değiştirin `docker run` komutu.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Bu komut:

* Tanı kapsayıcı kapsayıcı görüntüsünü çalıştırır.
* Bir CPU Çekirdeği ve 4 gigabayt (GB) bellek ayırır.
* 5000 numaralı TCP bağlantı noktasını kullanıma sunar ve sahte TTY için kapsayıcı ayırır.
* Bunu çıktıktan sonra kapsayıcı otomatik olarak kaldırır. Kapsayıcı görüntüsü ana bilgisayarda kullanılabilir durumda kalır. 

Daha fazla [örnekler](./computer-vision-resource-container-config.md#example-docker-run-commands) , `docker run` komutu kullanılabilir. 

> [!IMPORTANT]
> `Eula`, `Billing`, Ve `ApiKey` kapsayıcıyı çalıştırmak için seçenekler belirtilmelidir; Aksi takdirde, kapsayıcı başlatılamıyor.  Daha fazla bilgi için [faturalama](#billing).

[!INCLUDE [Running multiple containers on the same host](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]


## <a name="query-the-containers-prediction-endpoint"></a>Sorgu kapsayıcının tahmini uç noktası

Kapsayıcı, REST tabanlı sorgu tahmin uç nokta API'leri sağlar. 

Ana bilgisayarını kullanmak `https://localhost:5000`, kapsayıcı API'leri için.

### <a name="asynchronous-text-recognition"></a>Zaman uyumsuz metin tanıma

Kullanabileceğiniz `POST /vision/v2.0/recognizeText` ve `GET /vision/v2.0/textOperations/*{id}*` işlemlerini zaman uyumsuz olarak yazdırılan metin, görüntü işleme hizmeti bu karşılık gelen REST işlemlerini nasıl kullandığı için benzer resim tanımak için uyumlu bir şekilde. Metni Tanı kapsayıcı yazdırılan metin, el yazısı olmayan metin şu anda yalnızca tanır. böylece `mode` normalde görüntü işleme hizmeti işlemi metni tanı kapsayıcı tarafından göz ardı edilir için belirtilen parametre.

### <a name="synchronous-text-recognition"></a>Zaman uyumlu metin tanıma

Kullanabileceğiniz `POST /vision/v2.0/recognizeTextDirect` görüntüdeki basılı metin eş zamanlı olarak tanıyacak şekilde işlemi. Bu işlem zaman uyumlu olduğundan, bu işlem için istek gövdesi için aynıdır `POST /vision/v2.0/recognizeText` işlemi, ancak yanıt gövdesi için bu işlem tarafından döndürülen aynı `GET /vision/v2.0/textOperations/*{id}*` işlemi.

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]


## <a name="stop-the-container"></a>Kapsayıcı Durdur

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcı içeren bir çıktı çalıştırırsanız [bağlama](./computer-vision-resource-container-config.md#mount-settings) ve günlüğe kaydetme etkin, kapsayıcı başlatma veya kapsayıcı çalıştırma sırasında gerçekleşen sorunları gidermek yararlı olan günlük dosyalarını oluşturur. 


## <a name="billing"></a>Faturalama

Azure için fatura, kullanarak metni tanı kapsayıcıları Gönder bir _metni tanı_ Azure hesabınız kaynaktaki. 

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Bu seçenekler hakkında daha fazla bilgi için bkz. [kapsayıcıları yapılandırma](./computer-vision-resource-container-config.md).

<!--blogs/samples/video coures -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Özet

Bu makalede, kavramlar ve indirme, yükleme ve çalışan metni tanı kapsayıcılar için iş akışı öğrendiniz. Özet:

* Metin sağlayan bir Linux kapsayıcı tanımak için Docker, metin tanıma şifrelenmiş.
* Kapsayıcı görüntülerini azure'da Microsoft kapsayıcı kayıt defteri (MCR) alanından indirilir.
* Docker kapsayıcı görüntüleri çalıştırın.
* Ana kapsayıcısının URI belirterek metni tanı kapsayıcılarında işlemleri çağırmak için REST API veya SDK'sını kullanabilirsiniz.
* Bir kapsayıcı örneği oluşturulurken, fatura bilgilerini belirtmeniz gerekir.

> [!IMPORTANT]
> Bilişsel hizmetler kapsayıcıları, kullanım ölçümü için Azure'a bağlanmadan çalıştırmak için lisanslanmaz. Müşteriler, her zaman faturalandırma bilgileri ölçüm hizmeti ile iletişim kurmak kapsayıcıları etkinleştirmeniz gerekiyor. Bilişsel hizmetler kapsayıcılar, Microsoft müşteri verilerini (örneğin, görüntü veya metin analiz edilen) göndermeyin.

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [kapsayıcıları yapılandırma](computer-vision-resource-container-config.md) yapılandırma ayarları
* Gözden geçirme [görüntü işleme genel bakış](Home.md) yazdırılan ve el yazısı metinleri tanıma hakkında daha fazla bilgi edinmek için  
* Başvurmak [görüntü işleme API'si](//westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) kapsayıcı tarafından desteklenen yöntemleri hakkında ayrıntılar için.
* Başvurmak [sık sorulan sorular (SSS)](FAQ.md) görüntü işleme işlevselliği ile ilgili sorunları gidermek için.
* Daha fazla kullanmanız [Bilişsel Hizmetleri kapsayıcıları](../cognitive-services-container-support.md)
