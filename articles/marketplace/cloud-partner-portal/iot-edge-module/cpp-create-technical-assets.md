---
title: Azure IOT Edge modülü teknik varlıklarınızı oluşturun | Azure Market
description: IOT Edge modülü için teknik varlıkları oluşturun.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: pabutler
ms.openlocfilehash: 9f40e2272388e6f467b8c3d0b01a6639bf652e80
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64942357"
---
# <a name="prepare-your-iot-edge-module-technical-assets"></a>Teknik varlıkları, IOT Edge modülü hazırlama

Bu makalede, IOT Edge modülü teknik varlıklarınızı Azure Market'te yayımlanan önce karşılaması gereken gereksinimleri açıklanır.

## <a name="understanding-iot-edge-modules-and-getting-started"></a>IOT Edge modüllerini anlama ve kullanmaya başlama

IOT Edge modülü bir IOT Edge Cihazınızda çalıştırmak için yaptığı Docker ile uyumlu bir kapsayıcıdır.

- IOT Edge modülleri hakkında daha fazla bilgi için bkz: [anlamak Azure IOT Edge modülleri](https://docs.microsoft.com/azure/iot-edge/iot-edge-modules).
- IOT Edge modülü geliştirme ile çalışmaya başlamak için bkz. [gereksinimleri ve IOT Edge modülleri geliştirmek için Araçlar](https://docs.microsoft.com/azure/iot-edge/module-development).

## <a name="technical-requirements"></a>Teknik gereksinimler

Aşağıdaki teknik gereksinimleri, IOT Edge modülü sertifikalı ve Azure Market'te yayımlanan sırayla karşılanması gerekir.

### <a name="platform-support"></a>Platform desteği

IOT Edge Modülü aşağıdaki platformu seçeneklerden birini desteklemesi gerekir.

#### <a name="tier-1-platforms-supported-by-iot-edge"></a>IOT Edge tarafından desteklenen Katman 1 platformlar

IOT Edge tarafından desteklenen tüm katman 1 platformlarını destekleyen (kaydedildiği şekilde [Azure IOT Edge desteği](https://docs.microsoft.com/azure/iot-edge/support)). Daha iyi bir müşteri deneyimi sağladığından bu seçenek önerilir. Bu ölçütleri karşılayan modülleri büyütmüş. Bu platform seçeneği kullanarak bir modülü gerekir:

- Sağlayan bir `latest` etiketi ve sürüm etiketi (örneğin, `1.0.1`) bildirim etiketleri GitHub ile oluşturulmuş olan [bildirim aracı](https://github.com/estesp/manifest-tool).
- Kullanım [Marketi sekmesinden](./cpp-marketplace-tab.md) bağlantısı eklemek için [sertifikalı cihazları uyumlu IOT Edge](https://aka.ms/iot-edge-certified). Bu bağlantıyı çözümler `https://aka.ms/iot-edge-certified`, burada müşteriler göz atın veya arayın bir Web sitesi sertifikalı cihazlar. Bu Web sitesi de denir [Azure IOT Edge sertifikası](https://catalog.azureiotsolutions.com/) cihaz Kataloğu.

#### <a name="a-subset-of-tier-1-platforms-supported-by-iot-edge"></a>Bir alt kümesini bir IOT Edge tarafından desteklenen Katman 1 platformlar
  
Bir alt kümesini (en az bir tane), IOT Edge tarafından desteklenen Katman 1 platformlar destekler (kaydedildiği şekilde [Azure IOT Edge desteği](https://docs.microsoft.com/azure/iot-edge/support)). Bu platform seçeneği kullanarak bir modülü gerekir:

- Sağlayan bir `latest` etiketi ve sürüm etiketi (örneğin, `1.0.1`) bildirim etiketleri GitHub ile oluşturulmuş olan [bildirim aracı](https://github.com/estesp/manifest-tool) birden fazla platform destekleniyorsa. Bildirim etiketleri yalnızca tek bir platform destekliyorsa isteğe bağlıdır.
- Kullanım [Marketi sekmesinden](./cpp-marketplace-tab.md) en az bir IOT Edge cihazının bağlantı sağlamak için [Azure IOT Edge sertifikası](https://catalog.azureiotsolutions.com/) cihaz Kataloğu.

### <a name="device-dimensions"></a>Cihaz boyutları

IOT Edge modülü boyutları (CPU/RAM/depolama/GPU/vb.) hedeflenen IOT Edge cihazlarında aşağıdaki gereksinimleri karşılaması gerekir:

- Modül gerekir **en az bir IOT Edge sertifikalı iş** aygıtı [Azure IOT Edge sertifikası](https://catalog.azureiotsolutions.com/) cihaz Kataloğu.
- **En düşük donanım gereksinimleri** teklif açıklaması son paragraf olarak belgelenmelidir (altında [Marketi sekmesinden](./cpp-marketplace-tab.md)). İsteğe bağlı olarak önemli ölçüde farklıysa önerilen donanım gereksinimleri de listeleyebilirsiniz. Örneğin, aşağıdaki bölümde, teklif açıklaması sonuna ekleyin:

  ```html
    <p><u>Minimum hardware requirements:</u> Linux x64 and arm32  OS, 1GB of RAM, 500 Mb of storage</p>
  ```

### <a name="configuration"></a>Yapılandırma

Ayrıca, bir IOT Edge cihazına dağıtım olarak modellemeniz mümkün hale getirmek için varsayılan yapılandırma ayarlarını içerir. Kapsayıcı edgeHub ve IOT Hub ile iletişim sağlamak için IOT Edge modülü SDK'sı de.

#### <a name="default-configuration"></a>Varsayılan yapılandırma

IOT Edge modülleri, sağlanan varsayılan ayarlarla başlatabilmek için olmalıdır [bulut iş ortağı Portalı'nın SKU sekmesini](./cpp-skus-tab.md). Aşağıdaki varsayılan ayarlarla kullanılabilir:

- Varsayılan **yolları**
- Varsayılan **çiftinin istenen özelliklerini**
- Varsayılan **ortam değişkenleri**
- Varsayılan **createOptions**

Burada, bir parametre için varsayılan bir değer gerekli (örneğin, bir müşterinin sunucusunun IP adresini) doesn't make Sense bir senaryoda, varsayılan değer olarak bir parametre ekleyin. Bu değer, büyük harf ve köşeli ayraçlar içindeki yer alır. Bu örnek için aşağıdaki varsayılan ortam değişkeni ayarlayabilirsiniz:

```
    ServerIPAddress = <MY_SERVER_IP_ADDRESS>
```

#### <a name="configuration-documentation"></a>Yapılandırma belgeleri

Tüm yapılandırma ayarlarını, bir IOT Edge modülünün açıkça belgelenmelidir (kendi yollar istenen özellikleri, ortam değişkenleri, createOptions ve benzeri ikizi nasıl kullanılacağını.) Belgelerinize bir bağlantı sağlayın veya belgelere teklifini/SKU'yu açıklamanızı parçası olması gerekir.

### <a name="tags-and-versioning"></a>Etiketleri ve sürüm oluşturma

Müşteriler modülü bir kolayca dağıtın ve otomatik olarak (bir senaryoda Geliştirici.) Market güncelleştirmeler almak mümkün olması gerekir Bunlar ayrıca kullanabilecek ve bir tam sürümü (bir üretim senaryosunda.) test ettikten donması olmalıdır

IOT Edge modülleri yayımlanması ve bu müşterinin beklentilerini karşılamak için aşağıdaki gereksinimleri karşılaması gerekir:

- Bir bildirime dahil `latest` tüm desteklenen platformlarda en son sürüme işaret eden etiketi.
- Sürüm etiketleri X.Y.Z, burada X, Y ve Z tamsayılardır biçiminde olmalıdır.
- Gibi bir "Sürüm" etiketi içeren `1.0.1`, tüm desteklenen platformlarda, belirli bir sürümüne işaret eden.
- "Sürüm" etiketleri gibi güncelleştirme `1.0.1`, sabit olması gerekir.

>[!Note]
>İsteğe bağlı olarak, sürüm oluşturma "sürüm çalışırken" etiketleri içerebilir `2.0` ve `1.0`. Bu, birden çok ana sürümü paralel bakımını destekler.

### <a name="telemetry"></a>Telemetri

Modüller modülü IOT SDK'sını kullanarak ayarlamanız gerekir benzersiz modülü tanımlayıcısı `PublisherId.OfferId.SkuId` telemetri amacıyla. Çalışmakta olan modül örnek sayısını belirlemek Azure Marketi benzersiz bir tanımlayıcı sağlar.

 Ayarlamak için aşağıdaki yöntemleri IOT modülü SDK'ları kullanın `ProductInfo` Bu tanımlayıcıya:

- [C\#](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.productinfo?view=azure-dotnet#Microsoft_Azure_Devices_Client_DeviceClient_ProductInfo) 
- [C](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/Iothub_sdk_options.md)
- [Python](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/Iothub_sdk_options.md)
- [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.productinfo?view=azure-java-stable)

Modülü IOT SDK'sını kullanma modüller için daha az kesin ınsights indirilme sayısı gibi bulut iş ortağı Portalı aracılığıyla kullanılabilir.

### <a name="security"></a>Güvenlik

IOT Edge modülleri, ana bilgisayarda olabildiğince az ayrıcalıklı erişim için istemeniz gerekir. [Ayrıcalıklı modülleri](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities) kaçınılmalıdır.

### <a name="module-iot-sdk"></a>Modülü IOT SDK'sı

Modülü IOT SDK'sı dahil olmak üzere sertifika için bir önkoşul değildir. Ancak, modül IOT SDK'sı dahil olmak üzere daha iyi bir kullanıcı deneyimi sağlayabilir. Örneğin, yönlendirme veya iletileri buluta gönderme desteklemek için.

Modülü IOT SDK'sı, çalışan modülü örneklerinin sayısı ile ilgili telemetri verilerini almak için gereklidir.


## <a name="recertification-process"></a>Yeniden sertifikalandırılmasını işlemi

<!-- Add legal time windows-->
İş ortakları kendi modüllerinin gibi etkileyen bir değişiklik olduğunda bildirimi alırsınız:

- IOT Edge tarafından desteklenen Katman 1 işletim sistemi/arch destek matrisi
- IOT modülü SDK'sı
- IOT Edge çalışma zamanı
- IOT Edge modülü sertifika yönergeleri

İş ortakları kendi modüllerinin güncelleştirin ve bunları bulut iş ortağı portalı Aracı'nı kullanma yetkisi gerekir.

## <a name="host-your-iot-edge-module-in-an-azure-container-registry"></a>Bir Azure Container Registry'de, IOT Edge modülü barındırın

Bulut iş ortağı portalını, IOT Edge modülü yüklemek için önce bunu barındırmak ihtiyacınız bir [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) (ACR). Modül bildirim bir etiket tarafından başvurulan görüntüsü etiketlerini dahil olmak üzere yayımlamak istediğiniz tüm etiketleri eklemeniz gerekir.


## <a name="next-steps"></a>Sonraki adımlar

- [IOT Edge modülü teklifinizi oluşturun](./cpp-create-offer.md)
