---
title: Docker kapsayıcı ayarları
titleSuffix: Language Understanding - Azure Cognitive Services
description: LUIS kapsayıcı çalışma zamanı ortamı kullanılarak yapılandırılan `docker run` komut bağımsız değişkenleri. LUIS birkaç isteğe bağlı ayarları ile birlikte gerekli birkaç ayar vardır.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: diberry
ms.openlocfilehash: e05998f74223ead6bb4e94b86469e51791e0263f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60599353"
---
# <a name="configure-language-understanding-docker-containers"></a>Dil anlama Docker kapsayıcıları yapılandırın 

**Language Understanding** (LUIS) kapsayıcı çalışma zamanı ortamı kullanarak yapılandırılmış `docker run` komut bağımsız değişkenleri. LUIS birkaç isteğe bağlı ayarları ile birlikte gerekli birkaç ayar vardır. Birkaç [örnekler](#example-docker-run-commands) komutu kullanılabilir. Giriş kapsayıcısı özgü ayarlar şunlardır [bağlama ayarları](#mount-settings) ve fatura ayarlar. 

## <a name="configuration-settings"></a>Yapılandırma ayarları

Bu kapsayıcı, aşağıdaki yapılandırma ayarları vardır:

|Gerekli|Ayar|Amaç|
|--|--|--|
|Evet|[ApiKey](#apikey-setting)|Fatura bilgileri izlemek için kullanılır.|
|Hayır|[ApplicationInsights](#applicationinsights-setting)|Eklemenizi sağlayan [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) kapsayıcınızı telemetri desteği.|
|Evet|[Billing](#billing-setting)|Azure'daki hizmet kaynağının uç nokta URI'sini belirtir.|
|Evet|[Eula](#eula-setting)| Kapsayıcı lisansını kabul ettiğinizi gösterir.|
|Hayır|[Fluentd](#fluentd-settings)|Günlük yazma ve isteğe bağlı olarak ölçüm verileri Fluentd sunucusuna.|
|Hayır|[HTTP Ara sunucusu](#http-proxy-credentials-settings)|Bir HTTP Proxy'si Giden istekleri yapmak için yapılandırın.|
|Hayır|[Logging](#logging-settings)|Kapsayıcınız için ASP.NET Core günlük kaydı desteği sunar. |
|Evet|[Mounts](#mount-settings)|Ana bilgisayardaki verileri okuyup kapsayıcıya, kapsayıcıdaki verileri okuyup ana bilgisayara yazar.|

> [!IMPORTANT]
> [ `ApiKey` ](#apikey-setting), [ `Billing` ](#billing-setting), Ve [ `Eula` ](#eula-setting) ayarları birlikte kullanılır ve bunları; Aksi takdirde, tüm üç için geçerli değerler sağlamanız gerekir kapsayıcınızı başlatılamıyor. Bir kapsayıcı örneği oluşturmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için bkz. [faturalama](luis-container-howto.md#billing).

## <a name="apikey-setting"></a>ApiKey ayarı

`ApiKey` Ayar kapsayıcısı için fatura bilgileri izlemek için kullanılan Azure kaynak anahtarını belirtir. ApiKey için bir değer belirtmeniz gerekir ve değer için geçerli bir anahtar olmalıdır _Bilişsel Hizmetler_ için belirtilen kaynak [ `Billing` ](#billing-setting) yapılandırma ayarı.

Bu ayar, aşağıdaki konumlarda bulunabilir:

* Azure portalı: **Bilişsel Hizmetler** kaynak yönetimi altında **anahtarları**
* LUIS portalı: **Anahtarları ve uç nokta ayarları** sayfası. 

Başlangıç veya geliştirme tuşuna kullanmayın. 

## <a name="applicationinsights-setting"></a>Applicationınsights ayarı

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-setting"></a>Faturalandırma ayarı

`Billing` Ayar uç noktası URI'si belirtir, _Bilişsel Hizmetler_ azure'da kaynak kapsayıcısı için fatura bilgileri ölçmek için kullanılır. Bu yapılandırma ayarı için bir değer belirtmeniz gerekir ve değeri geçerli bir uç noktası URI'si olmalıdır için bir _Bilişsel Hizmetler_ azure'da kaynak. Kapsayıcı yaklaşık her 10 ila 15 dakika kullanım raporları.

Bu ayar, aşağıdaki konumlarda bulunabilir:

* Azure portalı: **Bilişsel Hizmetler** etiketli genel bakış `Endpoint`
* LUIS portalı: **Anahtarları ve uç nokta ayarları** URI uç noktasının bir parçası olarak bir sayfa.

Dahil etmeyi unutmayın `luis/v2.0` aşağıdaki tabloda gösterildiği gibi URL yönlendirme:


|Gerekli| Ad | Veri türü | Açıklama |
|--|------|-----------|-------------|
|Evet| `Billing` | Dize | Faturalandırma uç noktası URI'si<br><br>Örnek:<br>`Billing=https://westus.api.cognitive.microsoft.com/luis/v2.0` |

## <a name="eula-setting"></a>EULA'yı ayarlama

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Fluentd ayarları


[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>HTTP proxy kimlik bilgileri ayarları

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

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

## <a name="example-docker-run-commands"></a>Örnek docker komutlarını çalıştırın

Aşağıdaki örnekler, yazma ve kullanma göstermek için yapılandırma ayarlarını kullanır. `docker run` komutları.  Kapsayıcıyı çalıştıran sonra dek çalıştırmaya devam [Durdur](luis-container-howto.md#stop-the-container) bu.


* **Satır devamlılığı karakteri**: Aşağıdaki bölümlerde docker komutları ters eğik çizgi kullanın `\`, satır devamı karakteri olarak. Bu konak işletim sisteminin gereksinimlerine göre kaldırın veya değiştirin. 
* **Bağımsız değişken sırası**: Docker kapsayıcıları ile çok iyi bilmiyorsanız, bağımsız değişkenlerin sırası değiştirmeyin.

Dahil etmeyi unutmayın `luis/v2.0` aşağıdaki tabloda gösterildiği gibi URL yönlendirme.

Yerine {_argument_name_} kendi değerlerinizle:

| Yer tutucu | Değer | Biçim veya örnek |
|-------------|-------|---|
|{ENDPOINT_KEY} | Eğitilen LUIS uygulama uç noktası anahtarı. |xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx|
|{BILLING_ENDPOINT} | Azure'da faturalandırma uç nokta değerinde kullanılabilir `Cognitive Services` genel bakış sayfası. |https://westus.api.cognitive.microsoft.com/luis/v2.0|

> [!IMPORTANT]
> `Eula`, `Billing`, Ve `ApiKey` kapsayıcıyı çalıştırmak için seçenekler belirtilmelidir; Aksi takdirde, kapsayıcı başlatılamıyor.  Daha fazla bilgi için [faturalama](luis-container-howto.md#billing).
> ApiKey değer **anahtarı** anahtarları ve uç noktaları sayfasında LUIS portalda ve Azure üzerinde kullanılabilir `Cognitive Services` kaynak anahtarlar sayfasında. 

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

### <a name="logging-example"></a>Günlük örnek 

Aşağıdaki komut günlüğe kaydetme düzeyini ayarlar `Logging:Console:LogLevel`, günlüğe kaydetme düzeyini yapılandırmak için [ `Information` ](https://msdn.microsoft.com). 

```bash
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis:latest \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={ENDPOINT_KEY} \
Logging:Console:LogLevel:Default=Information
```

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [yükleme ve kapsayıcıları çalıştırın](luis-container-howto.md)
* Başvurmak [sorun giderme](troubleshooting.md) LUIS işlevselliği ile ilgili sorunları gidermek için.
* Daha fazla kullanmanız [Bilişsel Hizmetleri kapsayıcıları](../cognitive-services-container-support.md)
