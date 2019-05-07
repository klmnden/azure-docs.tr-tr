---
title: Personalizer nasıl çalışır?
titleSuffix: Azure Cognitive Services
description: Docker çalıştırma komutu yapılandırma ayarları ile Personalizer kapsayıcı çalıştırmayı öğrenin.
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: 85344d432c169ce8c4adffd059059660672df02e
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65158556"
---
# <a name="configure-personalizer-containers"></a>Personalizer kapsayıcılar'ı yapılandırma

**Personalizer** kapsayıcı çalışma zamanı ortamı kullanarak yapılandırılmış `docker run` komut bağımsız değişkenleri. Bu kapsayıcı birkaç isteğe bağlı ayarları ile birlikte gerekli birkaç ayar vardır. Birkaç [örnekler](#example-docker-run-commands) komutu kullanılabilir. Kapsayıcı özgü ayarları faturalandırma ayarlarıdır. 

## <a name="configuration-settings"></a>Yapılandırma ayarları

Bu kapsayıcı, aşağıdaki yapılandırma ayarları vardır:

|Gerekli|Ayar|Amaç|
|--|--|--|
|Evet|[ApiKey](#apikey-configuration-setting)|Fatura bilgileri izlemek için kullanılır.|
|Hayır|[ApplicationInsights](#applicationinsights-setting)|Eklemenizi sağlayan [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) kapsayıcınızı telemetri desteği.|
|Evet|[Billing](#billing-configuration-setting)|Azure'daki hizmet kaynağının uç nokta URI'sini belirtir.|
|Evet|[Eula](#eula-setting)| Kapsayıcı lisansını kabul ettiğinizi gösterir.|
|Hayır|[Fluentd](#fluentd-settings)|Günlük yazma ve isteğe bağlı olarak ölçüm verileri Fluentd sunucusuna.|
|Hayır|[HTTP Ara sunucusu](#http-proxy-credentials-settings)|Bir HTTP Proxy'si Giden istekleri yapmak için yapılandırın.|
|Hayır|[Logging](#logging-settings)|Kapsayıcınız için ASP.NET Core günlük kaydı desteği sunar. |
|Hayır|[Mounts](#mount-settings)|Ana bilgisayardaki verileri okuyup kapsayıcıya, kapsayıcıdaki verileri okuyup ana bilgisayara yazar.|

> [!IMPORTANT]
> [ `ApiKey` ](#apikey-configuration-setting), [ `Billing` ](#billing-configuration-setting), Ve [ `Eula` ](#eula-setting) ayarları birlikte kullanılır ve bunları; Aksi takdirde, tüm üç için geçerli değerler sağlamanız gerekir kapsayıcınızı başlatılamıyor. Bir kapsayıcı örneği oluşturmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için bkz. [faturalama](personalizer-container-howto.md#billing).

## <a name="apikey-configuration-setting"></a>ApiKey yapılandırma ayarı

`ApiKey` Ayar kapsayıcısı için fatura bilgileri izlemek için kullanılan Azure kaynak anahtarını belirtir. ApiKey için bir değer belirtmeniz gerekir ve değer için geçerli bir anahtar olmalıdır _Personalizer_ için belirtilen kaynak [ `Billing` ](#billing-configuration-setting) yapılandırma ayarı.

Bu ayar, aşağıdaki yerinde bulunabilir:

* Azure portalı: **Personalizer'ın** kaynak yönetimi altında **anahtarları**

## <a name="applicationinsights-setting"></a>Applicationınsights ayarı

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Yapılandırma ayarı faturalama

`Billing` Ayar uç noktası URI'si belirtir, _Personalizer_ azure'da kaynak kapsayıcısı için fatura bilgileri ölçmek için kullanılır. Bu yapılandırma ayarı için bir değer belirtmeniz gerekir ve değeri geçerli bir uç noktası URI'si olmalıdır için bir _Personalizer_ azure'da kaynak.

Bu ayar, aşağıdaki yerinde bulunabilir:

* Azure portalı: **Personalizer'ın** etiketli genel bakış `Endpoint`

|Gerekli| Ad | Veri türü | Açıklama |
|--|------|-----------|-------------|
|Evet| `Billing` | Dize | Faturalandırma uç noktası URI'si<br><br>Örnek:<br>`Billing=https://westus.api.cognitive.microsoft.com/sts/v1.0` |

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

Kapsayıcıları giriş kullanmayın ya da eğitim veya hizmeti verilerini depolamak için çıkış bağlar. 

Konak bağlama konumu söz dizimi konak işletim sistemine göre değişir. Ayrıca, [ana bilgisayar](personalizer-container-howto.md#the-host-computer)'s bağlama konumu docker hizmet hesabı tarafından kullanılan izinler arasında bir çakışma nedeniyle erişilebilir olmayabilir ve konak yeri izinleri bağlayın. 

|İsteğe bağlı| Ad | Veri türü | Açıklama |
|-------|------|-----------|-------------|
|İzin verilmedi| `Input` | String | Personalizer kapsayıcıları bu kullanmayın.|
|İsteğe bağlı| `Output` | String | Çıkış bağlama hedefi. Varsayılan değer `/output` şeklindedir. Bu günlükler konumdur. Bu, kapsayıcı günlükleri içerir. <br><br>Örnek:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Örnek docker komutlarını çalıştırın 

Aşağıdaki örnekler, yazma ve kullanma göstermek için yapılandırma ayarlarını kullanır. `docker run` komutları.  Kapsayıcıyı çalıştıran sonra dek çalıştırmaya devam [Durdur](personalizer-container-howto.md#stop-the-container) bu.

* **Satır devamlılığı karakteri**: Aşağıdaki bölümlerde Docker komutları ters eğik çizgi kullanın `\`, bir bash kabuğunda satır devamlılığı karakteri olarak. Bu konak işletim sisteminin gereksinimlerine göre kaldırın veya değiştirin. Örneğin, windows için satır devamı karakteri bir şapka olan `^`. Ters eğik çizgi ile giriş işaretini değiştirin. 
* **Bağımsız değişken sırası**: Docker kapsayıcıları ile çok iyi bilmiyorsanız, bağımsız değişkenlerin sırası değiştirmeyin.

Değeri parantez içine değiştirin `{}`, kendi değerlerinizle:

| Yer tutucu | Değer | Biçim veya örnek |
|-------------|-------|---|
|{BILLING_KEY} | Personalizer kaynak uç noktası anahtarı. |xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx|
|{BILLING_ENDPOINT_URI} | Bölge dahil olmak üzere faturalandırma uç nokta değeri.|`https://westus2.api.cognitive.microsoft.com/`|

> [!IMPORTANT]
> `Eula`, `Billing`, Ve `ApiKey` kapsayıcıyı çalıştırmak için seçenekler belirtilmelidir; Aksi takdirde, kapsayıcı başlatılamıyor.  Daha fazla bilgi için [faturalama](personalizer-container-howto.md#billing).
> ApiKey değer **anahtar** Azure Personalizer kaynak anahtarlar sayfasındaki. 

## <a name="personalizer-container-docker-examples"></a>Personalizer kapsayıcı Docker örnekleri

Aşağıdaki Docker Personalizer kapsayıcı için verilebilir. 

### <a name="basic-example"></a>Temel örnek 

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
  mcr.microsoft.com/azure-cognitive-services/[container-name] \
  Eula=accept \
  Billing={BILLING_ENDPOINT_URI} \
  ApiKey={BILLING_KEY} 
  ```

### <a name="logging-example-with-command-line-arguments"></a>Komut satırı bağımsız değişkenleri ile günlük örnek

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
  mcr.microsoft.com/azure-cognitive-services/[container-name] \
  Eula=accept \
  Billing={BILLING_ENDPOINT_URI} ApiKey={BILLING_KEY} \
  Logging:Console:LogLevel:Default=Information
  ```

## <a name="next-steps"></a>Sonraki adımlar

[Docker kapsayıcısı kullanmayı öğrenme](personalizer-container-howto.md)