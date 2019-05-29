---
title: Konuşma kapsayıcılara yükleme
titleSuffix: Azure Cognitive Services
description: Yükleme ve konuşma kapsayıcıları çalıştırın. Konuşma metin, ses akışları gerçek zamanlı olarak, uygulamalar, Araçlar veya cihazları kullanma veya görüntüleme metne dönüştürür. Metin okuma, giriş metni İnsan benzeri Sentezlenen konuşmaya dönüştürür.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: diberry
ms.openlocfilehash: b620cbb8e51fbe41defb6bdbdc66ba4a7e539aa0
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66306549"
---
# <a name="install-and-run-speech-service-containers"></a>Yükleme ve konuşma hizmeti kapsayıcıları çalıştırma

Konuşma kapsayıcıları hem güçlü bulut özellikleri hem de edge yerleşim yeri yararlanmak için iyileştirilmiş bir konuşma uygulama mimarisi yapı sağlar. Artık desteklediğimiz iki konuşma kapsayıcılardır **konuşma metin** ve **metin okuma**. 

İki konuşma kapsayıcılardır **konuşma metin** ve **metin okuma**. 

|İşlev|Özellikler|En son|
|-|-|--|
|Konuşmayı Metne Dönüştürme| <li>Sürekli, gerçek zamanlı konuşmaları metne dönüştürür.<li>Ses kayıtlarından toplu konuşma dökümü alabilir. <li>Ara sonuçların yanı sıra konuşma sonu algılama, otomatik metin biçimlendirme ve küfür maskeleme özelliklerini destekler. <li>Dökümü alınmış bir konuşmadan kullanıcının amacını anlamak üzere [Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/)'de (LUIS) çağrı yapabilir.\*|1.1.1|
|Metin Okuma| <li>Metni, doğal sesli konuşmaya dönüştürür. <li>Desteklenen birçok dil için birden fazla cinsiyet ve/veya diyalekt sunar. <li>Düz metin girişini veya Konuşma Birleştirme İşaretleme Dilini (SSML) destekler. |1.1.0|

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Konuşma kapsayıcıları kullanmadan önce aşağıdaki gereksinimleri karşılaması gerekir:

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Docker Altyapısı'nın kurulu ihtiyacınız bir [ana bilgisayar](#the-host-computer). Docker üzerinde Docker ortamını yapılandıran paketler sağlar [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Docker ve kapsayıcı temelleri hakkında bilgi için bkz: [Docker'a genel bakış](https://docs.docker.com/engine/docker-overview/).<br><br> Docker, kapsayıcılar ile bağlanma ve faturalama verileri Azure'a göndermek izin verecek şekilde yapılandırılmalıdır. <br><br> **Windows üzerinde**, Docker de Linux kapsayıcıları destekler şekilde yapılandırılmalıdır.<br><br>|
|Docker ile aşinalık | Bir temel kavramlarını Docker kayıt defterleri, havuzları, kapsayıcılar ve kapsayıcı görüntülerinin yanı sıra temel bilgi gibi olmalıdır `docker` komutları.| 
|Konuşma kaynak |Bu kapsayıcıların kullanabilmeniz için şunlara sahip olmalısınız:<br><br>A _konuşma_ fatura uç noktası URI'si ve ilişkili faturalandırma anahtarı almak için Azure kaynak. Her iki değeri de Azure portalında üzerinde kullanılabilir **konuşma** genel bakış ve anahtarları sayfaları ve bu, kapsayıcı başlatma için gerekli.<br><br>**{BILLING_KEY}** : kaynak anahtarı<br><br>**{BILLING_ENDPOINT_URI}** : uç nokta URI'si örnektir: `https://westus.api.cognitive.microsoft.com/sts/v1.0`|

## <a name="request-access-to-the-container-registry"></a>Kapsayıcı kayıt defterine erişim isteği

Önce tamamlamanız ve gönderme gerekir [Bilişsel hizmetler konuşma kapsayıcıları istek formunu](https://aka.ms/speechcontainerspreview/) kapsayıcıya erişim istemek için. 

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

## <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="advanced-vector-extension-support"></a>Gelişmiş vektör uzantısı desteği

**Konak** , docker kapsayıcısı çalıştıran bilgisayardır. Konak desteklemelidir [Gelişmiş vektör uzantıları](https://en.wikipedia.org/wiki/Advanced_Vector_Extensions#CPUs_with_AVX2) (AVX2). Bu destek Linux Konaklara aşağıdaki komutla kontrol edebilirsiniz: 

```console
grep -q avx2 /proc/cpuinfo && echo AVX2 supported || echo No AVX2 support detected
```

### <a name="container-requirements-and-recommendations"></a>Kapsayıcı gereksinimleri ve önerileri

Aşağıdaki tabloda, en düşük ve önerilen CPU Çekirdeği ve her bir konuşma kapsayıcısı için ayrılacak bellek açıklanmaktadır.

| Kapsayıcı | Minimum | Önerilen |
|-----------|---------|-------------|
|bilişsel hizmetler-konuşma-metne dönüştürme | 2 Çekirdek<br>2 GB bellek  | 4 çekirdek<br>4 GB bellek  |
|bilişsel hizmetler-metin-için-konuşma | 1 çekirdek, 0,5 GB bellek| 2 Çekirdek, 1 GB bellek |

* Her çekirdeğe en az 2.6 gigahertz (GHz) olması ya da daha hızlı.


Çekirdek ve bellek karşılık `--cpus` ve `--memory` parçası olarak kullanılan ayarları `docker run` komutu.

**Not**; En düşük ve önerilen Docker sınırları dışına dayalı *değil* ana makine kaynakları. Örneğin, büyük dil modeli ve konuşma metin kapsayıcıları bellek eşlemesi bölümleri olan _önerilen_ dosyanın tamamı bir ek 4-6 GB bellekte uyar. Ayrıca, ilk çalıştırma ya da kapsayıcının belleğe modelleri havuzda olduğundan daha uzun sürebilir.

## <a name="get-the-container-image-with-docker-pull"></a>İle kapsayıcı görüntüsünü Al `docker pull`

Kapsayıcı görüntülerini konuşma için kullanılabilir. 

| Kapsayıcı | Depo |
|-----------|------------|
| bilişsel hizmetler-konuşma-metne dönüştürme | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text:latest` |
| bilişsel hizmetler-metin-için-konuşma | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech:latest` |

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="language-locale-is-in-container-tag"></a>Kapsayıcı etikettir içinde dil yerel ayar

`latest` Etiketi çeken `en-us` yerel ayar ve `jessarus` ses. 

#### <a name="speech-to-text-locales"></a>Konuşma metin yerel ayarlar

Tüm etiketleri dışında `latest` aşağıdaki biçimdedir burada `<culture>` yerel kapsayıcı gösterir:

```
<major>.<minor>.<patch>-<platform>-<culture>-<prerelease>
```

Aşağıdaki etiketi biçimi örneğidir:

```
1.0.0-amd64-en-us-preview
```

Aşağıdaki tabloda desteklenen yerel ayarlar için **konuşma metin** 1.1.1, kapsayıcı sürümü:

|Dil yerel ayar|Tags|
|--|--|
|Çince|`zh-cn`|
|İngilizce |`en-us`<br>`en-gb`<br>`en-au`<br>`en-in`|
|Fransızca  |`fr-ca`<br>`fr-fr`|
|Almanca |`de-de`|
|İtalyanca|`it-it`|
|Japonca|`ja-jp`|
|Korece|`ko-kr`|
|Portekizce|`pt-br`|
|İspanyolca |`es-es`<br>`es-mx`|


#### <a name="text-to-speech-locales"></a>Metin okuma yerel ayarlar

Tüm etiketleri dışında `latest` aşağıdaki biçimdedir burada `<culture>` yerel gösterir ve `<voice>` kapsayıcı sesini gösterir:

```
<major>.<minor>.<patch>-<platform>-<culture>-<voice>-<prerelease>
```

Aşağıdaki etiketi biçimi örneğidir:

```
1.0.0-amd64-en-us-jessarus-preview
```

Aşağıdaki tabloda desteklenen yerel ayarlar için **metin okuma** 1.1.0 içinde kapsayıcı sürümü:

|Dil yerel ayar|Tags|Desteklenen sesleri|
|--|--|--|
|Çince|`zh-cn`|huihuirus<br>kangkang-apollo<br>yaoyao apollo|
|İngilizce |`en-au`|catherine<br>hayleyrus|
|İngilizce |`en-gb`|George apollo<br>hazelrus<br>Susan apollo|
|İngilizce |`en-in`|heera apollo<br>priyarus<br>ravi apollo<br>|
|İngilizce |`en-us`|jessarus<br>benjaminrus<br>jessa24krus<br>zirarus<br>guy24krus|
|Fransızca |`fr-ca`|caroline<br>harmonierus|
|Fransızca |`fr-fr`|hortenserus<br>Julie apollo<br>Paul apollo|
|Almanca |`de-de`|hedda<br>heddarus<br>Stefan apollo|
|İtalyanca |`it-it`|cosimo apollo<br>luciarus|
|Japonca|`ja-jp`|ayumi apollo<br>harukarus<br>ichiro apollo|
|Korece|`ko-kr`|heamirus|
|Portekizce|`pt-br`|Daniel apollo<br>heloisarus|
|İspanyolca |`es-es`|elenarus<br>Gamze apollo<br>Pablo apollo<br>|
|İspanyolca |`es-mx`|hildarus<br>raul apollo|

### <a name="docker-pull-for-the-speech-containers"></a>Konuşma kapsayıcılar için docker isteği

#### <a name="speech-to-text"></a>Konuşmayı Metne Dönüştürme

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text:latest
```

#### <a name="text-to-speech"></a>Metin Okuma

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech:latest
```

## <a name="how-to-use-the-container"></a>Kapsayıcı kullanma

Kapsayıcı açıldığında [ana bilgisayar](#the-host-computer), kapsayıcı ile çalışmak için aşağıdaki işlemi kullanın.

1. [Kapsayıcıyı çalıştırmak](#run-the-container-with-docker-run), gerekli, ancak kullanılmadı faturalama ayarları. Daha fazla [örnekler](speech-container-configuration.md#example-docker-run-commands) , `docker run` komutu kullanılabilir. 
1. [Kapsayıcının tahmini uç nokta sorgu](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>Kapsayıcı ile çalıştırma `docker run`

Kullanım [docker run](https://docs.docker.com/engine/reference/commandline/run/) üç kapsayıcı birini çalıştırmak için komutu. Komutu şu parametreleri kullanır:

**Önizleme sırasında**faturalama ayarları kapsayıcı başlatmak için geçerli olması gerekir, ancak kullanım için faturalandırılır değildir.

| Yer tutucu | Değer |
|-------------|-------|
|{BILLING_KEY} | Bu anahtar kapsayıcısı başlatmak için kullanılır ve Azure portalının konuşma anahtarlar sayfasında bulabilirsiniz.  |
|{BILLING_ENDPOINT_URI} | Fatura uç noktası URI değerini Azure portalının konuşma genel bakış sayfasında kullanılabilir.|

Bu parametreleri aşağıdaki örnekte kendi değerlerinizle değiştirin `docker run` komutu.

### <a name="text-to-speech"></a>Metin Okuma

```bash
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY} 
```

### <a name="speech-to-text"></a>Konuşmayı Metne Dönüştürme

```bash
docker run --rm -it -p 5000:5000 --memory 2g --cpus 2 \
containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY} 
```

Bu komut:

* Bir konuşma kapsayıcı kapsayıcı görüntüsünü çalıştırır.
* 2 CPU Çekirdeği ve 2 gigabayt (GB) bellek ayırır.
* 5000 numaralı TCP bağlantı noktasını kullanıma sunar ve sahte TTY için kapsayıcı ayırır.
* Bunu çıktıktan sonra kapsayıcı otomatik olarak kaldırır. Kapsayıcı görüntüsü ana bilgisayarda kullanılabilir durumda kalır. 

> [!IMPORTANT]
> `Eula`, `Billing`, Ve `ApiKey` kapsayıcıyı çalıştırmak için seçenekler belirtilmelidir; Aksi takdirde, kapsayıcı başlatılamıyor.  Daha fazla bilgi için [faturalama](#billing).

## <a name="query-the-containers-prediction-endpoint"></a>Sorgu kapsayıcının tahmini uç noktası

|Kapsayıcı|Uç Nokta|
|--|--|
|Konuşmayı Metne Dönüştürme|ws: / / localhost:5000/konuşma/tanıma/yazdırma/cognitiveservices/v1|
|Metin Okuma|http://localhost:5000/speech/synthesize/cognitiveservices/v1|

### <a name="speech-to-text"></a>Konuşmayı Metne Dönüştürme

Kapsayıcı websocket tabanlı sorgu uç noktası aracılığıyla erişilen API'ler sağlar [Speech SDK'sı](index.yml).

Varsayılan olarak, Speech SDK'sı çevrimiçi konuşma Hizmetleri kullanır. Kapsayıcı kullanmak için başlatma yöntemi değiştirmeniz gerekir. Aşağıdaki örneklere bakın.

#### <a name="for-c"></a>İçinC#

Bu Azure bulut başlatma çağrısı kullanarak değiştirin:

```C#
var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

Bu çağrı, kapsayıcı uç noktasını kullanarak:

```C#
var config = SpeechConfig.FromEndpoint("ws://localhost:5000/speech/recognition/dictation/cognitiveservices/v1", "YourSubscriptionKey");
```

#### <a name="for-python"></a>Python için

Bu Azure bulut başlatma çağrısı kullanarak değiştirme

```python
speech_config = speechsdk.SpeechConfig(subscription=speech_key, region=service_region)
```

Bu çağrı, kapsayıcı uç noktasını kullanarak:

```python
speech_config = speechsdk.SpeechConfig(subscription=speech_key, endpoint="ws://localhost:5000/speech/recognition/dictation/cognitiveservices/v1")
```

### <a name="text-to-speech"></a>Metin Okuma

Kapsayıcı REST uç noktasını bulunabilir API'leri sağlar [burada](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#text-to-speech-api) ve örnekler bulunabilir [burada](https://azure.microsoft.com/resources/samples/cognitive-speech-tts/).


[!INCLUDE [Validate container is running - Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]


## <a name="stop-the-container"></a>Kapsayıcı Durdur

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcı çalıştırdığınızda, kapsayıcının kullanan **stdout** ve **stderr** başlatılıyor veya kapsayıcı çalıştırma sırasında gerçekleşen sorunları gidermek yararlı olan çıkış bilgiler. 

## <a name="billing"></a>Faturalama

Azure için fatura, kullanarak konuşma kapsayıcıları Gönder bir _konuşma_ Azure hesabınız kaynaktaki. 

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Bu seçenekler hakkında daha fazla bilgi için bkz. [kapsayıcıları yapılandırma](speech-container-configuration.md).

<!--blogs/samples/video coures -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Özet

Bu makalede, kavramlar ve indirme, yükleme ve konuşma kapsayıcıları çalıştırmak için iş akışı öğrendiniz. Özet:

* Konuşma, iki Linux kapsayıcıları için Docker, konuşma metin ve metin okuma için şifreleme sağlar.
* Azure'da özel kapsayıcı kayıt defterinden kapsayıcı görüntülerini indirilir.
* Docker kapsayıcı görüntüleri çalıştırın.
* Ana kapsayıcısının URI belirterek konuşma kapsayıcılarında işlemleri çağırmak için REST API veya SDK'sını kullanabilirsiniz.
* Bir kapsayıcı örneği oluşturulurken, fatura bilgilerini belirtmeniz gerekir.

> [!IMPORTANT]
>  Bilişsel hizmetler kapsayıcıları, kullanım ölçümü için Azure'a bağlanmadan çalıştırmak için lisanslanmaz. Müşteriler, her zaman faturalandırma bilgileri ölçüm hizmeti ile iletişim kurmak kapsayıcıları etkinleştirmeniz gerekiyor. Bilişsel hizmetler kapsayıcılar, Microsoft müşteri verilerini (örneğin, görüntü veya metin analiz edilen) göndermeyin.

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [kapsayıcıları yapılandırma](speech-container-configuration.md) yapılandırma ayarları
* Daha fazla kullanmanız [Bilişsel Hizmetleri kapsayıcıları](../cognitive-services-container-support.md)
