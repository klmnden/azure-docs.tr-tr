---
title: Docker kapsayıcı ayarları
titleSuffix: Language Understanding - Azure Cognitive Services
description: LUIS kapsayıcı çalışma zamanı ortamı kullanılarak yapılandırılan `docker run` komut bağımsız değişkenleri. LUIS birkaç isteğe bağlı ayarları ile birlikte gerekli birkaç ayar vardır.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: diberry
ms.openlocfilehash: e8e838fae0da3a47fe1b3ec8d412f956f5f28034
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2019
ms.locfileid: "53975518"
---
# <a name="configure-language-understanding-docker-containers"></a>Language Understanding docker kapsayıcıları yapılandırın 

Language Understanding (LUIS) kapsayıcı çalışma zamanı ortamı kullanılarak yapılandırılan `docker run` komut bağımsız değişkenleri. LUIS birkaç isteğe bağlı ayarları ile birlikte gerekli birkaç ayar vardır. Birkaç [örnekler](#example-docker-run-commands) komutu kullanılabilir. Giriş kapsayıcısı özgü ayarlar şunlardır [bağlama ayarları](#mount-settings) ve fatura ayarlar. 

Kapsayıcı ayarları [hiyerarşik](#hierarchical-settings) ve ayarlanabilir [ortam değişkenlerini](#environment-variable-settings) ya da docker [komut satırı bağımsız değişkenleri](#command-line-argument-settings).

## <a name="configuration-settings"></a>Yapılandırma ayarları

Bu kapsayıcı, aşağıdaki yapılandırma ayarları vardır:

|Gerekli|Ayar|Amaç|
|--|--|--|
|Evet|[ApiKey](#apikey-setting)|Fatura bilgileri izlemek için kullanılır.|
|Hayır|[ApplicationInsights](#applicationinsights-setting)|Eklemenizi sağlayan [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) kapsayıcınızı telemetri desteği.|
|Evet|[Faturalandırma](#billing-setting)|Azure'da uç nokta hizmet kaynağın URI'sini belirtir.|
|Evet|[EULA'sı](#eula-setting)| Kapsayıcı lisansını kabul ettiğiniz gösterir.|
|Hayır|[Fluentd](#fluentd-settings)|Günlük yazma ve isteğe bağlı olarak ölçüm verileri Fluentd sunucusuna.|
|Hayır|[Günlüğe kaydetme](#logging-settings)|ASP.NET Core günlüğü kapsayıcınız için destek sağlar. |
|Evet|[Bağlar](#mount-settings)|Okuyup veri kapsayıcısı ana bilgisayarından kapsayıcısından geri ana bilgisayara yazma.|

> [!IMPORTANT]
> [ `ApiKey` ](#apikey-setting), [ `Billing` ](#billing-setting), Ve [ `Eula` ](#eula-setting) ayarları birlikte kullanılır ve bunları; Aksi takdirde, tüm üç için geçerli değerler sağlamanız gerekir kapsayıcınızı başlatılamıyor. Bir kapsayıcı örneği oluşturmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için bkz. [faturalama](luis-container-howto.md#billing).

## <a name="apikey-setting"></a>ApiKey ayarı

`ApiKey` Ayar kapsayıcısı için fatura bilgileri izlemek için kullanılan Azure kaynak anahtarını belirtir. ApiKey için bir değer belirtmeniz gerekir ve değer için geçerli bir anahtar olmalıdır _Language Understanding_ için belirtilen kaynak [ `Billing` ](#billing-setting) yapılandırma ayarı.

Bu ayar, aşağıdaki konumlarda bulunabilir:

* Azure portalı: **Language Understanding'i'nın** kaynak yönetimi altında **anahtarları**
* LUIS portalı: **Anahtarları ve uç nokta ayarları** sayfası. 

Başlangıç veya geliştirme tuşuna kullanmayın. 

## <a name="applicationinsights-setting"></a>Applicationınsights ayarı

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-setting"></a>Faturalandırma ayarı

`Billing` Ayar uç noktası URI'si belirtir, _Language Understanding_ azure'da kaynak kapsayıcısı için fatura bilgileri ölçmek için kullanılır. Bu yapılandırma ayarı için bir değer belirtmeniz gerekir ve değeri geçerli bir uç noktası URI'si olmalıdır için bir _Language Understanding_ azure'da kaynak.

Bu ayar, aşağıdaki konumlarda bulunabilir:

* Azure portalı: **Language Understanding'i'nın** etiketli genel bakış `Endpoint`
* LUIS portalı: **Anahtarları ve uç nokta ayarları** URI uç noktasının bir parçası olarak bir sayfa.

|Gereklidir| Ad | Veri türü | Açıklama |
|--|------|-----------|-------------|
|Evet| `Billing` | Dize | Faturalandırma uç noktası URI'si<br><br>Örnek:<br>`Billing=https://westus.api.cognitive.microsoft.com/luis/v2.0` |

## <a name="eula-setting"></a>EULA'yı ayarlama

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Fluentd ayarları


[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="logging-settings"></a>Günlük ayarları
 
[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]


## <a name="mount-settings"></a>Bağlama ayarları

Kullanım bağlama okumak ve kapsayıcı gelen ve giden veri yazmak için bağlar. Bir giriş bağlama belirtin veya çıkış bağlama belirterek `--mount` seçeneğini [docker run](https://docs.docker.com/engine/reference/commandline/run/) komutu. 

LUIS kapsayıcı giriş kullanmaz ya da eğitim veya hizmeti verilerini depolamak için çıkış bağlar. 

Konak bağlama konumu söz dizimi konak işletim sistemine göre değişir. Ayrıca, [ana bilgisayar](luis-container-howto.md#the-host-computer)'s bağlama konumu docker hizmet hesabı tarafından kullanılan izinler arasında bir çakışma nedeniyle erişilebilir olmayabilir ve konak yeri izinleri bağlayın. 

Aşağıdaki tabloda, desteklenen ayarları açıklanmaktadır.

|Gerekli| Ad | Veri türü | Açıklama |
|-------|------|-----------|-------------|
|Evet| `Input` | Dize | Giriş bağlama hedefi. Varsayılan değer `/input` şeklindedir. LUIS paket dosyalarının konumunu budur. <br><br>Örnek:<br>`--mount type=bind,src=c:\input,target=/input`|
|Hayır| `Output` | Dize | Çıkış bağlama hedefi. Varsayılan değer `/output` şeklindedir. Bu günlükler konumdur. Bu, LUIS sorgu ve kapsayıcı günlükleri içerir. <br><br>Örnek:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="hierarchical-settings"></a>Hiyerarşik ayarları

[!INCLUDE [Container shared configuration hierarchical settings](../../../includes/cognitive-services-containers-configuration-shared-hierarchical-settings.md)]


## <a name="example-docker-run-commands"></a>Örnek docker komutlarını çalıştırın

Aşağıdaki örnekler, yazma ve kullanma göstermek için yapılandırma ayarlarını kullanır. `docker run` komutları.  Kapsayıcıyı çalıştıran sonra dek çalıştırmaya devam [Durdur](luis-container-howto.md#stop-the-container) bu.


* **Satır devamlılığı karakteri**: Aşağıdaki bölümlerde docker komutları ters eğik çizgi kullanın `\`, satır devamı karakteri olarak. Bu konak işletim sisteminin gereksinimlerine göre kaldırın veya değiştirin. 
* **Bağımsız değişken sırası**: Docker kapsayıcıları ile çok iyi bilmiyorsanız, bağımsız değişkenlerin sırası değiştirmeyin.

Yerine {_argument_name_} kendi değerlerinizle:

| Yer tutucu | Değer | Biçim veya örnek |
|-------------|-------|---|
|{ENDPOINT_KEY} | Eğitilen LUIS uygulama uç noktası anahtarı. |xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx|
|{BILLING_ENDPOINT} | Fatura uç nokta değerini Azure portalının dil anlama genel bakış sayfasında kullanılabilir.|https://westus.api.cognitive.microsoft.com/luis/v2.0|

> [!IMPORTANT]
> `Eula`, `Billing`, Ve `ApiKey` kapsayıcıyı çalıştırmak için seçenekler belirtilmelidir; Aksi takdirde, kapsayıcı başlatılamıyor.  Daha fazla bilgi için [faturalama](luis-container-howto.md#billing).
> ApiKey değer **anahtar** anahtarları ve uç noktaları sayfasında LUIS portalda ve Azure dil anlama kaynak anahtarlar sayfasında kullanılabilir. 

### <a name="basic-example"></a>Temel örnek

Aşağıdaki örnek, kapsayıcıyı çalıştırmak olası en az sayıda bağımsız değişkenlere sahiptir:

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis:latest \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={ENDPOINT_KEY}
```

> [!Note] 
> Yukarıdaki komut dizini aracınızdan `c:` sürücü Windows üzerinde hiçbir izni çakışmalarını önlemek için. Giriş dizini belirli bir dizini kullanmak istiyorsanız, docker vermeniz gerekebilir hizmet izni. Yukarıdaki komut docker ters eğik çizgi kullanır `\`, satır devamı karakteri olarak. Bu temel kaldırın veya değiştirin, [ana bilgisayar](luis-container-howto.md#the-host-computer) işletim sisteminin gereksinimleri. Docker kapsayıcıları ile çok iyi bilmiyorsanız, bağımsız değişkenlerin sırası değiştirmeyin.


### <a name="applicationinsights-example"></a>Applicationınsights örneği

Aşağıdaki örnek, kapsayıcıyı çalışırken Application Insights'a telemetri göndermek Applicationınsights bağımsız değişken ayarlar:

```bash
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis:latest \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={ENDPOINT_KEY}
InstrumentationKey={INSTRUMENTATION_KEY}
```

### <a name="logging-example-with-command-line-arguments"></a>Komut satırı bağımsız değişkenleri ile günlük örnek

Aşağıdaki komut günlüğe kaydetme düzeyini ayarlar `Logging:Console:LogLevel`, günlüğe kaydetme düzeyini yapılandırmak için [ `Information` ](https://msdn.microsoft.com). 

```bash
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis:latest \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={ENDPOINT_KEY} \
Logging:Console:LogLevel=Information
```

### <a name="logging-example-with-environment-variable"></a>Günlük örnek ortam değişkeni

Adlandırılmış bir ortam değişkeni aşağıdaki komutları kullanma `Logging:Console:LogLevel` günlüğe kaydetme düzeyini yapılandırmak için [ `Information` ](https://msdn.microsoft.com). 

```bash
SET Logging:Console:LogLevel=Information
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis:latest \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={APPLICATION_ID} \
```

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [yükleme ve kapsayıcıları çalıştırın](luis-container-howto.md)
* Başvurmak [sık sorulan sorular (SSS)](luis-resources-faq.md) LUIS işlevselliği ile ilgili sorunları gidermek için.