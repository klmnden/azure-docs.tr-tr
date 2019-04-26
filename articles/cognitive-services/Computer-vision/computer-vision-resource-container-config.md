---
title: Kapsayıcı - görüntü işleme yapılandırın
titlesuffix: Azure Cognitive Services
description: Görüntü işleme metin tanıma kapsayıcılar için çeşitli ayarları yapılandırın.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 42c08864c6908e92a7ecea336f8b1bd0606760db
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60203558"
---
# <a name="configure-recognize-text-docker-containers"></a>Tanı metin Docker kapsayıcıları yapılandırın

**Metni tanı** kapsayıcı çalışma zamanı ortamı kullanarak yapılandırılmış `docker run` komut bağımsız değişkenleri. Bu kapsayıcı birkaç isteğe bağlı ayarları ile birlikte gerekli birkaç ayar vardır. Birkaç [örnekler](#example-docker-run-commands) komutu kullanılabilir. Kapsayıcı özgü ayarları faturalandırma ayarlarıdır. 

## <a name="configuration-settings"></a>Yapılandırma ayarları

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> [ `ApiKey` ](#apikey-configuration-setting), [ `Billing` ](#billing-configuration-setting), Ve [ `Eula` ](#eula-setting) ayarları birlikte kullanılır ve bunları; Aksi takdirde, tüm üç için geçerli değerler sağlamanız gerekir kapsayıcınızı başlatılamıyor. Bir kapsayıcı örneği oluşturmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için bkz. [faturalama](computer-vision-how-to-install-containers.md).

## <a name="apikey-configuration-setting"></a>ApiKey yapılandırma ayarı

`ApiKey` Ayar belirtir Azure `Cognitive Services` kapsayıcısı için fatura bilgileri izlemek için kullanılan kaynak anahtarı. ApiKey için bir değer belirtmeniz gerekir ve değer için geçerli bir anahtar olmalıdır _Bilişsel Hizmetler_ için belirtilen kaynak [ `Billing` ](#billing-configuration-setting) yapılandırma ayarı.

Bu ayar, aşağıdaki yerinde bulunabilir:

* Azure portalı: **Bilişsel Hizmetler** kaynak yönetimi altında **anahtarları**

## <a name="applicationinsights-setting"></a>Applicationınsights ayarı

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Yapılandırma ayarı faturalama

`Billing` Ayar uç noktası URI'si belirtir, _Bilişsel Hizmetler_ azure'da kaynak kapsayıcısı için fatura bilgileri ölçmek için kullanılır. Bu yapılandırma ayarı için bir değer belirtmeniz gerekir ve değeri geçerli bir uç noktası URI'si olmalıdır için bir _Bilişsel Hizmetler_ azure'da kaynak. Kapsayıcı yaklaşık her 10 ila 15 dakika kullanım raporları.

Bu ayar, aşağıdaki yerinde bulunabilir:

* Azure portalı: **Bilişsel Hizmetler** etiketli genel bakış `Endpoint`

Eklemeyi unutmayın `vision/v1.0` aşağıdaki tabloda gösterildiği gibi uç nokta URI'si yönlendirme. 

|Gerekli| Ad | Veri türü | Açıklama |
|--|------|-----------|-------------|
|Evet| `Billing` | Dize | Faturalandırma uç noktası URI'si<br><br>Örnek:<br>`Billing=https://westcentralus.api.cognitive.microsoft.com/vision/v1.0` |

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

Görüntü işleme kapsayıcıları giriş kullanmayın ya da eğitim veya hizmeti verilerini depolamak için çıkış bağlar. 

Konak bağlama konumu söz dizimi konak işletim sistemine göre değişir. Ayrıca, [ana bilgisayar](computer-vision-how-to-install-containers.md#the-host-computer)'s bağlama konumu Docker hizmet hesabı tarafından kullanılan izinler arasında bir çakışma nedeniyle erişilebilir olmayabilir ve konak yeri izinleri bağlayın. 

|İsteğe bağlı| Ad | Veri türü | Açıklama |
|-------|------|-----------|-------------|
|İzin verilmedi| `Input` | String | Bilgisayar işleme kapsayıcıları bu kullanmayın.|
|İsteğe bağlı| `Output` | String | Çıkış bağlama hedefi. Varsayılan değer `/output` şeklindedir. Bu günlükler konumdur. Bu, kapsayıcı günlükleri içerir. <br><br>Örnek:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Örnek docker komutlarını çalıştırın 

Aşağıdaki örnekler, yazma ve kullanma göstermek için yapılandırma ayarlarını kullanır. `docker run` komutları.  Kapsayıcıyı çalıştıran sonra dek çalıştırmaya devam [Durdur](computer-vision-how-to-install-containers.md#stop-the-container) bu.

* **Satır devamlılığı karakteri**: Aşağıdaki bölümlerde Docker komutları ters eğik çizgi kullanın `\`, satır devamı karakteri olarak. Bu konak işletim sisteminin gereksinimlerine göre kaldırın veya değiştirin. 
* **Bağımsız değişken sırası**: Docker kapsayıcıları ile çok iyi bilmiyorsanız, bağımsız değişkenlerin sırası değiştirmeyin.

Eklemeyi unutmayın `vision/v1.0` aşağıdaki tabloda gösterildiği gibi uç nokta URI'si yönlendirme. 

Yerine {_argument_name_} kendi değerlerinizle:

| Yer tutucu | Değer | Biçim veya örnek |
|-------------|-------|---|
|{BILLING_KEY} | Bilişsel hizmetler kaynak uç noktası anahtarı. |xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx|
|{BILLING_ENDPOINT_URI} | Bölge dahil olmak üzere faturalandırma uç nokta değeri.|`https://westcentralus.api.cognitive.microsoft.com/vision/v1.0`|

> [!IMPORTANT]
> `Eula`, `Billing`, Ve `ApiKey` kapsayıcıyı çalıştırmak için seçenekler belirtilmelidir; Aksi takdirde, kapsayıcı başlatılamıyor.  Daha fazla bilgi için [faturalama](computer-vision-how-to-install-containers.md#billing).
> ApiKey değer **anahtarı** Azure `Cognitive Services` kaynak anahtarlar sayfasında. 

## <a name="recognize-text-container-docker-examples"></a>Metin kapsayıcı Docker örnekler tanıması

Aşağıdaki Docker için tanı metin kapsayıcı verilebilir. 

### <a name="basic-example"></a>Temel örnek 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
  containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text \
  Eula=accept \
  Billing={BILLING_ENDPOINT_URI} \
  ApiKey={BILLING_KEY} 
  ```

### <a name="logging-example"></a>Günlük örnek 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
  containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text \
  Eula=accept \
  Billing={BILLING_ENDPOINT_URI} \
  ApiKey={BILLING_KEY} \
  Logging:Console:LogLevel:Default=Information
  ```

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [yükleme ve kapsayıcıları çalıştırın](computer-vision-how-to-install-containers.md)
