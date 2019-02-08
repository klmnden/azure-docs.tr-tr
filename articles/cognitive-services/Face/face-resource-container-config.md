---
title: Kapsayıcıları yapılandırma
titlesuffix: Face - Azure Cognitive Services
description: Kapsayıcılar için yapılandırma ayarları.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: diberry
ms.openlocfilehash: bb5bea5fb74c48e4568898d77763d442db5b41c7
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55870546"
---
# <a name="configure-face-docker-containers"></a>Yüz tanıma Docker kapsayıcıları yapılandırın

**Yüz** kapsayıcı çalışma zamanı ortamı kullanarak yapılandırılmış `docker run` komut bağımsız değişkenleri. Bu kapsayıcı birkaç isteğe bağlı ayarları ile birlikte gerekli birkaç ayar vardır. Birkaç [örnekler](#example-docker-run-commands) komutu kullanılabilir. Kapsayıcı özgü ayarları faturalandırma ayarlarıdır. 

Kapsayıcı ayarları [hiyerarşik](#hierarchical-settings) ve ayarlanabilir [ortam değişkenlerini](#environment-variable-settings) ya da docker [komut satırı bağımsız değişkenleri](#command-line-argument-settings).

## <a name="configuration-settings"></a>Yapılandırma ayarları

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> [ `ApiKey` ](#apikey-setting), [ `Billing` ](#billing-setting), Ve [ `Eula` ](#eula-setting) ayarları birlikte kullanılır ve bunları; Aksi takdirde, tüm üç için geçerli değerler sağlamanız gerekir kapsayıcınızı başlatılamıyor. Bir kapsayıcı örneği oluşturmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için bkz. [faturalama](face-how-to-install-containers.md#billing).

## <a name="apikey-configuration-setting"></a>ApiKey yapılandırma ayarı

`ApiKey` Ayar kapsayıcısı için fatura bilgileri izlemek için kullanılan Azure kaynak anahtarını belirtir. ApiKey için bir değer belirtmeniz gerekir ve değer için geçerli bir anahtar olmalıdır _yüz_ için belirtilen kaynak [ `Billing` ](#billing-setting) yapılandırma ayarı.

Bu ayar, aşağıdaki yerinde bulunabilir:

* Azure portalı: **Yüzünün** kaynak yönetimi altında **anahtarları**

## <a name="applicationinsights-setting"></a>Applicationınsights ayarı

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Yapılandırma ayarı faturalama

`Billing` Ayar uç noktası URI'si belirtir, _yüz_ azure'da kaynak kapsayıcısı için fatura bilgileri ölçmek için kullanılır. Bu yapılandırma ayarı için bir değer belirtmeniz gerekir ve değeri geçerli bir uç noktası URI'si olmalıdır için bir _yüz_ azure'da kaynak.

Bu ayar, aşağıdaki yerinde bulunabilir:

* Azure portalı: **Yüzünün** etiketli genel bakış `Endpoint`

|Gerekli| Ad | Veri türü | Açıklama |
|--|------|-----------|-------------|
|Evet| `Billing` | Dize | Faturalandırma uç noktası URI'si<br><br>Örnek:<br>`Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0` |

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

Yüz tanıma kapsayıcıları giriş kullanmıyorum veya eğitim veya hizmeti verilerini depolamak için çıkış bağlar. 

Konak bağlama konumu söz dizimi konak işletim sistemine göre değişir. Ayrıca, [ana bilgisayar](face-how-to-install-containers.md#the-host-computer)'s bağlama konumu Docker hizmet hesabı tarafından kullanılan izinler arasında bir çakışma nedeniyle erişilebilir olmayabilir ve konak yeri izinleri bağlayın. 

|İsteğe bağlı| Ad | Veri türü | Açıklama |
|-------|------|-----------|-------------|
|İzin verilmedi| `Input` | String | Yüz tanıma kapsayıcıları bu kullanmayın.|
|İsteğe bağlı| `Output` | String | Çıkış bağlama hedefi. Varsayılan değer `/output` şeklindedir. Bu günlükler konumdur. Bu, kapsayıcı günlükleri içerir. <br><br>Örnek:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="hierarchical-settings"></a>Hiyerarşik ayarları

[!INCLUDE [Container shared configuration hierarchical settings](../../../includes/cognitive-services-containers-configuration-shared-hierarchical-settings.md)]

## <a name="example-docker-run-commands"></a>Örnek docker komutlarını çalıştırın 

Aşağıdaki örnekler, yazma ve kullanma göstermek için yapılandırma ayarlarını kullanır. `docker run` komutları.  Kapsayıcıyı çalıştıran sonra dek çalıştırmaya devam [Durdur](face-how-to-install-containers.md#stop-the-container) bu.

* **Satır devamlılığı karakteri**: Aşağıdaki bölümlerde Docker komutları ters eğik çizgi kullanın `\`, satır devamı karakteri olarak. Bu konak işletim sisteminin gereksinimlerine göre kaldırın veya değiştirin. 
* **Bağımsız değişken sırası**: Docker kapsayıcıları ile çok iyi bilmiyorsanız, bağımsız değişkenlerin sırası değiştirmeyin.

Yerine {_argument_name_} kendi değerlerinizle:

| Yer tutucu | Değer | Biçim veya örnek |
|-------------|-------|---|
|{BILLING_KEY} | Yüz tanıma kaynak uç noktası anahtarı. |xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx|
|{BILLING_ENDPOINT_URI} | Bölge dahil olmak üzere faturalandırma uç nokta değeri.|`https://westcentralus.api.cognitive.microsoft.com/face/v1.0`|

> [!IMPORTANT]
> `Eula`, `Billing`, Ve `ApiKey` kapsayıcıyı çalıştırmak için seçenekler belirtilmelidir; Aksi takdirde, kapsayıcı başlatılamıyor.  Daha fazla bilgi için [faturalama](face-how-to-install-containers.md#billing).
> ApiKey değer **anahtar** Azure yüz kaynak anahtarlar sayfasındaki. 

## <a name="face-container-docker-examples"></a>Kapsayıcı Docker örnekler yüz tanıma

Aşağıdaki Docker için yüz kapsayıcı verilebilir. 

### <a name="basic-example"></a>Temel örnek 

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
  containerpreview.azurecr.io/microsoft/cognitive-services-face \
  Eula=accept \
  Billing={BILLING_ENDPOINT_URI} \
  ApiKey={BILLING_KEY} 
  ```

### <a name="logging-example-with-command-line-arguments"></a>Komut satırı bağımsız değişkenleri ile günlük örnek

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face \
  Eula=accept \
  Billing={BILLING_ENDPOINT_URI} ApiKey={BILLING_KEY} \
  Logging:Console:LogLevel=Information
  ```

### <a name="logging-example-with-environment-variable"></a>Günlük örnek ortam değişkeni

  ```Docker
  SET Logging:Console:LogLevel=Information
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face \
  Eula=accept \
  Billing={BILLING_ENDPOINT_URI} \
  ApiKey={BILLING_KEY}
  ```

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [yükleme ve kapsayıcıları çalıştırın](face-how-to-install-containers.md)
