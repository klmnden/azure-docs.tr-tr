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
ms.date: 04/16/2019
ms.author: diberry
ms.openlocfilehash: 4152cf90d9de2eda15a798fbf6b5b4aa4f5646f7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60815392"
---
# <a name="configure-face-docker-containers"></a>Yüz tanıma Docker kapsayıcıları yapılandırın

**Yüz** kapsayıcı çalışma zamanı ortamı kullanarak yapılandırılmış `docker run` komut bağımsız değişkenleri. Bu kapsayıcı birkaç isteğe bağlı ayarları ile birlikte gerekli birkaç ayar vardır. Birkaç [örnekler](#example-docker-run-commands) komutu kullanılabilir. Kapsayıcı özgü ayarları faturalandırma ayarlarıdır. 

## <a name="configuration-settings"></a>Yapılandırma ayarları

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> [ `ApiKey` ](#apikey-configuration-setting), [ `Billing` ](#billing-configuration-setting), Ve [ `Eula` ](#eula-setting) ayarları birlikte kullanılır ve bunları; Aksi takdirde, tüm üç için geçerli değerler sağlamanız gerekir kapsayıcınızı başlatılamıyor. Bir kapsayıcı örneği oluşturmak için bu yapılandırma ayarlarını kullanma hakkında daha fazla bilgi için bkz. [faturalama](face-how-to-install-containers.md#billing).

## <a name="apikey-configuration-setting"></a>ApiKey yapılandırma ayarı

`ApiKey` Ayar kapsayıcısı için fatura bilgileri izlemek için kullanılan Azure kaynak anahtarını belirtir. ApiKey için bir değer belirtmeniz gerekir ve değer için geçerli bir anahtar olmalıdır _Bilişsel Hizmetler_ için belirtilen kaynak [ `Billing` ](#billing-configuration-setting) yapılandırma ayarı.

Bu ayar, aşağıdaki yerinde bulunabilir:

* Azure portalı: **Bilişsel Hizmetler** kaynak yönetimi altında **anahtarları**

## <a name="applicationinsights-setting"></a>Applicationınsights ayarı

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Yapılandırma ayarı faturalama

`Billing` Ayar uç noktası URI'si belirtir, _Bilişsel Hizmetler_ azure'da kaynak kapsayıcısı için fatura bilgileri ölçmek için kullanılır. Bu yapılandırma ayarı için bir değer belirtmeniz gerekir ve değeri geçerli bir uç noktası URI'si olmalıdır için bir _Bilişsel Hizmetler_ azure'da kaynak. Kapsayıcı yaklaşık her 10 ila 15 dakika kullanım raporları.

Bu ayar, aşağıdaki yerinde bulunabilir:

* Azure portalı: **Bilişsel Hizmetler** etiketli genel bakış `Endpoint`

Eklemeyi unutmayın _yüz_ örnekte gösterilen şekilde URI uç noktasına yönlendirme. 

|Gerekli| Ad | Veri türü | Açıklama |
|--|------|-----------|-------------|
|Evet| `Billing` | Dize | Faturalandırma uç noktası URI'si<br><br>Örnek:<br>`Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0` |

<!-- specific to face only -->

## <a name="cloudai-configuration-settings"></a>CloudAI yapılandırma ayarları

Yapılandırma ayarlarında `CloudAI` bölümü kapsayıcınıza benzersiz kapsayıcı özgü seçenekler sağlar. Aşağıdaki ayarları ve nesneleri yüz kapsayıcıda desteklenir `CloudAI` bölümü

| Ad | Veri türü | Açıklama |
|------|-----------|-------------|
| `Storage` | Nesne | Yüz tanıma kapsayıcı tarafından kullanılan depolama senaryo. Depolama senaryoları ve için ilişkili ayarları hakkında daha fazla bilgi için `Storage` nesne, bkz: [depolama senaryo ayarları](#storage-scenario-settings) |

### <a name="storage-scenario-settings"></a>Depolama senaryosu ayarları

Yüz tanıma kapsayıcı, blob, önbellek, meta verileri ve sıra veri, ne saklanan bağlı olarak depolar. Örneğin, eğitim dizinleri ve sonuçları bir büyük kişi grubu için blob veri depolanır. Yüz tanıma kapsayıcı ile etkileşim kurulurken iki farklı depolama senaryoları ve bu tür verilerin depolama sağlar:

* Bellek  
  Tüm dört veri türlerini bellekte depolanır. Dağılımı ya da bunların kalıcıdır. Yüz tanıma kapsayıcı durdurulmuş veya kaldırılmış, tüm bu kapsayıcı için depolama birimindeki verileri bozulur.  
  Yüz tanıma kapsayıcısı için varsayılan depolama senaryosu budur.
* Azure  
  Yüz tanıma kapsayıcı, bu dört veri türleri arasında kalıcı depolama dağıtmak için Azure depolama ve Azure Cosmos DB kullanır. BLOB ve kuyruk verileri Azure Depolama tarafından işlenir. Meta verileri ve önbellek verilerini Azure Cosmos DB tarafından işlenir. Yüz tanıma kapsayıcı durdurulmuş veya kaldırılmış, tüm veriler bu kapsayıcı için depolama alanında kalır Azure depolama ve Azure Cosmos DB içinde depolanan.  
  Azure depolama senaryo tarafından kullanılan kaynakları aşağıdaki ek gereksinimlere sahiptir.
  * Azure depolama kaynağı StorageV2 hesap türü kullanmalısınız.
  * Azure Cosmos DB kaynak MongoDB için Azure Cosmos DB'nin API'sini kullanmanız gerekir

Tarafından yönetilen depolama senaryolar ve ilişkili yapılandırma ayarları `Storage` altında nesne `CloudAI` yapılandırma bölümü. Aşağıdaki yapılandırma ayarları kullanılabilir `Storage` nesnesi:

| Ad | Veri türü | Açıklama |
|------|-----------|-------------|
| `StorageScenario` | Dize | Kapsayıcı tarafından desteklenen depolama senaryo. Aşağıdaki değerler kullanılabilir<br/>`Memory` -Varsayılan değer. Kapsayıcı, tek bir düğüm, geçici kullanım için kalıcı olmayan, olmayan dağıtılmış ve bellek içi depolama kullanır. Bu kapsayıcı için depolama kapsayıcısı durdurulmuş veya kaldırılmış olmadığını yok edilir.<br/>`Azure` -Kapsayıcı, depolama için Azure kaynaklarını kullanır. Bu kapsayıcı için depolama kapsayıcısı durdurulmuş veya kaldırılmış olmadığını kalıcı hale getirilir.|
| `ConnectionStringOfAzureStorage` | Dize | Kapsayıcı tarafından kullanılan Azure depolama kaynağı için bağlantı dizesi.<br/>Bu ayar, yalnızca aşağıdaki durumlarda geçerlidir `Azure` için belirtilen `StorageScenario` yapılandırma ayarı. |
| `ConnectionStringOfCosmosMongo` | Dize | Kapsayıcı tarafından kullanılan Azure Cosmos DB kaynak için MongoDB bağlantı dizesi.<br/>Bu ayar, yalnızca aşağıdaki durumlarda geçerlidir `Azure` için belirtilen `StorageScenario` yapılandırma ayarı. |

Örneğin, aşağıdaki komutu, Azure depolama senaryosu belirtir ve örnek bağlantı dizeleri yüz kapsayıcı verilerini depolamak için kullanılan Azure depolama ve Cosmos DB kaynakları sağlar.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789 CloudAI:Storage:StorageScenario=Azure CloudAI:Storage:ConnectionStringOfCosmosMongo="mongodb://samplecosmosdb:0123456789@samplecosmosdb.documents.azure.com:10255/?ssl=true&replicaSet=globaldb" CloudAI:Storage:ConnectionStringOfAzureStorage="DefaultEndpointsProtocol=https;AccountName=sampleazurestorage;AccountKey=0123456789;EndpointSuffix=core.windows.net"
  ```

Depolama senaryosu alanından ayrı olarak yönetilir takar giriş ve çıkış takar. Tek bir kapsayıcı için bu özellikleri birleşimi belirtebilirsiniz. Örneğin, aşağıdaki komutu için Docker bağlama bağlama tanımlar `D:\Output` klasörü çıkış bağlama olarak konak makinedeki sonra JSON biçiminde çıktı bağlama için günlük dosyalarını kaydetme yüz kapsayıcı görüntüsünden bir kapsayıcı oluşturur. Komut ayrıca Azure depolama senaryosu belirtir ve örnek bağlantı dizeleri yüz kapsayıcı verilerini depolamak için kullanılan Azure depolama ve Cosmos DB kaynakları sağlar.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 --mount type=bind,source=D:\Output,destination=/output containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789 Logging:Disk:Format=json CloudAI:Storage:StorageScenario=Azure CloudAI:Storage:ConnectionStringOfCosmosMongo="mongodb://samplecosmosdb:0123456789@samplecosmosdb.documents.azure.com:10255/?ssl=true&replicaSet=globaldb" CloudAI:Storage:ConnectionStringOfAzureStorage="DefaultEndpointsProtocol=https;AccountName=sampleazurestorage;AccountKey=0123456789;EndpointSuffix=core.windows.net"
  ```

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

## <a name="example-docker-run-commands"></a>Örnek docker komutlarını çalıştırın 

Aşağıdaki örnekler, yazma ve kullanma göstermek için yapılandırma ayarlarını kullanır. `docker run` komutları.  Kapsayıcıyı çalıştıran sonra dek çalıştırmaya devam [Durdur](face-how-to-install-containers.md#stop-the-container) bu.

* **Satır devamlılığı karakteri**: Aşağıdaki bölümlerde Docker komutları ters eğik çizgi kullanın `\`, satır devamı karakteri olarak. Bu konak işletim sisteminin gereksinimlerine göre kaldırın veya değiştirin. 
* **Bağımsız değişken sırası**: Docker kapsayıcıları ile çok iyi bilmiyorsanız, bağımsız değişkenlerin sırası değiştirmeyin.

Yerine {_argument_name_} kendi değerlerinizle:

| Yer tutucu | Değer | Biçim veya örnek |
|-------------|-------|---|
|{BILLING_KEY} | Bilişsel hizmetler kaynak uç noktası anahtarı. |xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx|
|{BILLING_ENDPOINT_URI} | Bölge ve yüz yönlendirme de dahil olmak üzere faturalandırma uç nokta değeri.|`https://westcentralus.api.cognitive.microsoft.com/face/v1.0`|

> [!IMPORTANT]
> `Eula`, `Billing`, Ve `ApiKey` kapsayıcıyı çalıştırmak için seçenekler belirtilmelidir; Aksi takdirde, kapsayıcı başlatılamıyor.  Daha fazla bilgi için [faturalama](face-how-to-install-containers.md#billing).
> ApiKey değer **anahtarı** Azure `Cognitive Services` kaynak anahtarlar sayfasında. 

## <a name="face-container-docker-examples"></a>Kapsayıcı Docker örnekler yüz tanıma

Aşağıdaki Docker için yüz kapsayıcı verilebilir. 

### <a name="basic-example"></a>Temel örnek 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
  containerpreview.azurecr.io/microsoft/cognitive-services-face \
  Eula=accept \
  Billing={BILLING_ENDPOINT_URI} \
  ApiKey={BILLING_KEY} 
  ```

### <a name="logging-example"></a>Günlük örnek 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face \
  Eula=accept \
  Billing={BILLING_ENDPOINT_URI} ApiKey={BILLING_KEY} \
  Logging:Console:LogLevel:Default=Information
  ```

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [yükleme ve kapsayıcıları çalıştırın](face-how-to-install-containers.md)
