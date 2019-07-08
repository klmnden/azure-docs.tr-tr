---
title: Kapsayıcı - Form tanıyıcı yapılandırın
titleSuffix: Azure Cognitive Services
description: Form ve tablo verilerini ayrıştırmak için Form tanıyıcı kapsayıcının nasıl yapılandırılacağını öğrenin.
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: dapine
ms.openlocfilehash: e4b6a852ece3060ecf866d66d32f213355d99950
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67592658"
---
# <a name="configure-form-recognizer-containers"></a>Form tanıyıcı kapsayıcılar'ı yapılandırma

Azure Form tanıyıcı kapsayıcıları kullanarak güçlü bulut özellikleri hem de edge yerleşim yeri yararlanmak için iyileştirilmiş bir uygulama mimarisi oluşturabilirsiniz.

Form tanıyıcı kapsayıcı çalışma zamanı ortamı kullanarak yapılandırma `docker run` komut bağımsız değişkenleri. Bu kapsayıcı birkaç gerekli olan ayarları ve bazı isteğe bağlı ayarlar. Bazı örnekler için bkz. ["örnek docker komutlarını çalıştırın"](#example-docker-run-commands) bölümü. Kapsayıcı özgü ayarları faturalandırma ayarlarıdır.

## <a name="configuration-settings"></a>Yapılandırma ayarları

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> [ `ApiKey` ](#apikey-configuration-setting), [ `Billing` ](#billing-configuration-setting), Ve [ `Eula` ](#eula-setting) ayarları birlikte kullanılır. Ayarların için geçerli değerler sağlamanız gerekir; Aksi takdirde, kapsayıcınızın başlatılamıyor. Bir kapsayıcı örneği oluşturmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için bkz. [faturalama](form-recognizer-container-howto.md#billing).

## <a name="apikey-configuration-setting"></a>ApiKey yapılandırma ayarı

`ApiKey` Ayar kapsayıcısı için fatura bilgileri izlemek için kullanılan Azure kaynak anahtarını belirtir. ApiKey değeri için geçerli bir anahtar olmalıdır _Form tanıyıcı_ için belirtilen kaynak `Billing` "yapılandırma ayarı fatura" bölümünde.

Bu Azure Portalı'nda ayarlamayı bulabilirsiniz **Form tanıyıcı kaynak yönetimi**altında **anahtarları**.

## <a name="applicationinsights-setting"></a>Applicationınsights ayarı

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Yapılandırma ayarı faturalama

`Billing` Ayar uç noktası URI'si belirtir, _Form tanıyıcı_ kaynak kapsayıcısı için fatura bilgileri ölçmek için kullanılan Azure üzerinde. Bu yapılandırma ayarının değeri geçerli bir uç noktası URI'si olmalıdır için bir _Form tanıyıcı_ azure'da kaynak. Kapsayıcı yaklaşık her 10 ila 15 dakika kullanım raporları.

Bu Azure Portalı'nda ayarlamayı bulabilirsiniz **Form tanıyıcı genel bakış**altında **uç nokta**.

|Gerekli| Ad | Veri türü | Açıklama |
|--|------|-----------|-------------|
|Evet| `Billing` | Dize | Faturalandırma uç noktası URI'si<br><br>Örnek:<br>`Billing=https://westus2.api.cognitive.microsoft.com/` |

## <a name="eula-setting"></a>EULA'yı ayarlama

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Fluentd ayarları

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>HTTP proxy kimlik bilgileri ayarları

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Günlük ayarları

[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]


## <a name="mount-settings"></a>Bağlama ayarları

Kullanım bağlama okumak ve kapsayıcı gelen ve giden veri yazmak için bağlar. Bir giriş bağlama veya bir çıkış bağlama belirterek belirtebilirsiniz `--mount` seçeneğini [ `docker run` komut](https://docs.docker.com/engine/reference/commandline/run/).

Form tanıyıcı kapsayıcı, bir giriş bağlama ve bir çıkış bağlama gerektirir. Giriş bağlama salt okunur olabilir ve eğitim ve puanlama için kullanılan veri erişimi için zorunludur. Çıkış bağlama yazılabilir olması gerekir ve modelleri ve geçici verileri depolamak için kullanırsınız.

Konak bağlama konumu söz dizimi konak işletim sistemine göre değişir. Ayrıca, bağlama konumu [ana bilgisayar](form-recognizer-container-howto.md#the-host-computer) Docker hizmet hesabı izinleri ve ana bilgisayar bağlama konumu izinler arasında bir çakışma nedeniyle erişilebilir olmayabilir.

|İsteğe bağlı| Ad | Veri türü | Açıklama |
|-------|------|-----------|-------------|
|Gerekli| `Input` | String | Giriş bağlama hedefi. Varsayılan değer `/input` şeklindedir.    <br><br>Örnek:<br>`--mount type=bind,src=c:\input,target=/input`|
|Gerekli| `Output` | String | Çıkış bağlama hedefi. Varsayılan değer `/output` şeklindedir.  <br><br>Örnek:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Örnek docker komutlarını çalıştırın

Aşağıdaki örnekler, yazma ve kullanma göstermek için yapılandırma ayarlarını kullanır. `docker run` komutları. Kapsayıcı çalışırken, dek çalıştırmaya devam [durdurmak](form-recognizer-container-howto.md#stop-the-container).

* **Satır devamlılığı karakteri**: Aşağıdaki bölümlerde Docker komutları ters eğik çizgi kullanın (\\) satır devamı karakteri olarak. Konak işletim sisteminin gereksinimlerine bağlı olarak bu karakteri kaldırın veya değiştirin.
* **Bağımsız değişken sırası**: Docker kapsayıcıları ile ilgili bilgi sahibi değilseniz, bağımsız değişkenlerin sırası değiştirmeyin.

Yerine {_argument_name_} kendi değerlerinizle aşağıdaki tabloda:

| Yer tutucu | Değer |
|-------------|-------|
|{BILLING_KEY} | Kapsayıcı başlatmak için kullanılan anahtar. Azure portalında Form tanıyıcı anahtarlar sayfasında kullanılabilir.  |
|{BILLING_ENDPOINT_URI} | Fatura uç noktası URI değeri, Azure portalı Form tanıyıcı genel bakış sayfasında kullanılabilir.|
|{COMPUTER_VISION_API_KEY}| Azure portalında bilgisayar görüntü işleme API'si anahtarlar sayfasında anahtarın kullanılabilir.|
|{COMPUTER_VISION_ENDPOINT_URI}|Fatura uç noktası. Bulut tabanlı bir görüntü işleme kaynak kullanıyorsanız, URI değerini Azure portalı bilgisayar işleme API'sine genel bakış sayfasında kullanılabilir. Kullanıyorsanız, bir *bilişsel hizmetler-tanıma-metin* kapsayıcı, kapsayıcıda geçirilir fatura uç nokta URL'sini kullanın `docker run` komutu.|

> [!IMPORTANT]
> Kapsayıcıyı çalıştırmak üzere belirttiğiniz `Eula`, `Billing`, ve `ApiKey` seçenekleri; Aksi takdirde, kapsayıcı başlatılamıyor. Daha fazla bilgi için [faturalama](#billing-configuration-setting).

> [!NOTE] 
> ApiKey değer **anahtar** Azure Form tanıyıcı kaynak anahtarlar sayfasındaki.

## <a name="form-recognizer-container-docker-examples"></a>Form tanıyıcı kapsayıcı Docker örnekleri

Aşağıdaki Docker için Form tanıyıcı kapsayıcı verilebilir.

### <a name="basic-example-for-form-recognizer"></a>Form tanıyıcı için temel örnek

```Docker
docker run --rm -it -p 5000:5000 --memory 8g --cpus 2 \
--mount type=bind,source=c:\input,target=/input  \
--mount type=bind,source=c:\output,target=/output \
containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY} \
FormRecognizer:ComputerVisionApiKey={COMPUTER_VISION_API_KEY} \
FormRecognizer:ComputerVisionEndpointUri={COMPUTER_VISION_ENDPOINT_URI}
```

### <a name="logging-example-for-form-recognizer"></a>Form tanıyıcı örneğin günlüğe kaydetme

```Docker
docker run --rm -it -p 5000:5000 --memory 8g --cpus 2 \
--mount type=bind,source=c:\input,target=/input  \
--mount type=bind,source=c:\output,target=/output \
containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY} \
FormRecognizer:ComputerVisionApiKey={COMPUTER_VISION_API_KEY} \
FormRecognizer:ComputerVisionEndpointUri={COMPUTER_VISION_ENDPOINT_URI}
Logging:Console:LogLevel:Default=Information
```


## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [yükleme ve çalıştırma kapsayıcıları](form-recognizer-container-howto.md).
