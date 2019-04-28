---
title: Dijital İkizlerini nesne modelleri ve uzamsal zeka graf anlama | Microsoft Docs
description: İnsanlar, yerler ve cihazlar arasındaki ilişkileri modellemek için Azure Digital Twins’i kullanın
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: alinast
ms.openlocfilehash: e7efe1a8632643e2a299b6c9a1b1407414deee4b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60925894"
---
# <a name="understand-digital-twins-object-models-and-spatial-intelligence-graph"></a>Dijital İkizlerini nesne modelleri ve uzamsal zeka graf anlama

Azure dijital İkizlerini fiziksel ortamları ve ilişkili cihazlar, algılayıcılar ve kişiler kapsamlı sanal temsillerini güç katan bir Azure IOT hizmetidir. Geliştirme, etki alanına özgü kavramları yararlı modellerini düzenleyerek artırır. Modelleri ardından bir uzamsal zeka grafik içinde yer. Bu kavramlar, kişiler, boşluk ve cihazlar arasındaki etkileşimler ve ilişkiler faithfully model.

Dijital İkizlerini nesne modelleri, etki alanına özgü kavramları, kategoriler ve özellikler açıklanmaktadır. Modelleri, kendi ihtiyaçlarınıza göre istediğiniz şekilde özelleştirilebilmesine isteyen kullanıcıların önceden tanımlanmıştır. Birlikte, bunlar dijital nesne modelleri oluşturan İkizlerini önceden bir _ontology_. Akıllı bir yapı 's ontology bölgeler, mekanlar, Katlar, ofisleri, bölgeleri, Konferans salonu ve odak odaları açıklar. Çeşitli power istasyonları, şalt, enerji kaynakları ve müşterilerin bir enerji kılavuz ontology açıklar. Dijital İkizlerini nesne modelleri ve ontolojileri, çeşitli senaryolar ve ihtiyaçlarınızı özelleştirilebilir.

Dijital İkizlerini nesne modelleri ve yerinde bir ontology ile doldurabilirsiniz bir _uzamsal graf_. Alanları, cihazları ve bir IOT çözümü için uygun olan kişiler arasında birçok ilişki sanal temsillerini uzamsal grafiklerdir. Bu diyagram bir akıllı yapı 's ontology kullanan bir uzamsal grafiği örneği gösterilmektedir.

![Dijital İkizlerini uzamsal grafik oluşturma][1]

<a id="model"></a>

Uzamsal grafik alanları, cihazlar, algılayıcılar ve kullanıcıların bir araya getirir. Her birlikte gerçek dünyada modellerin bir şekilde bağlıdır. Bu örnekte, mekan 43 her birçok farklı alanlara sahip dört Katlar yok. Kullanıcılar kendi iş istasyonları ile ilişkili ve graf kısımlarına erişim verilir. Yönetici bir ziyaretçi yalnızca belirli bir yapı verileri görüntüleme haklarına sahip olmakla birlikte, uzamsal grafiğe, değişiklik yapmak için haklara sahip olur.

## <a name="digital-twins-object-models"></a>Dijital İkizlerini nesne modelleri

Bu kategoride nesnelerin dijital İkizlerini nesne modellerini destekler:

- **Alanları** sanal veya fiziksel konumlar, örneğin: `Tenant`, `Customer`, `Region`, ve `Venue`.
- **Cihazları** sanal veya fiziksel donanım, örneğin, parçalarıdır `AwesomeCompany Device` ve `Raspberry Pi 3`.
- **Algılayıcılar** olayları, örneğin, algılamak nesneler `AwesomeCompany Temperature Sensor` ve `AwesomeCompany Presence Sensor`.
- **Kullanıcılar** occupants ve özelliklerini tanımlar.

Diğer nesnelerin kategorileri şunlardır:

- **Kaynakları** için bir alanı eklenmiş ve genellikle uzamsal grafiğinde, örneğin, nesneler tarafından kullanılan Azure kaynakları temsil `IoTHub`.
- **Blobları** nesnelerine (örneğin, boşluk, cihazlar, algılayıcılar ve kullanıcılar) eklenir. MIME türü ve meta dosyaları olarak örneğin kullanıldıklarından `maps`, `pictures`, ve `manuals`.
- **Türler Genişletilmiş** belirli özelliklere sahip varlıklar örneğin büyütmek Genişletilebilir numaralandırmalar olan `SpaceType` ve `SpaceSubtype`.
- **Ontolojileri** genişletilmiş türleri kümesi örneğin temsil `Default`, `Building`, `BACnet`, ve `EnergyGrid`.
- **Özellik anahtarları ve değerleri** alanları, cihazlar, algılayıcılar ve kullanıcıların özel özellikleri sahiptir. Bunlar gibi yerleşik özelliklere yanı sıra kullanılabilir `DeltaProcessingRefreshTime` anahtar olarak ve `10` değer olarak.
- **Rolleri** kullanıcılara ve cihazlara uzamsal grafiğinde, örneğin, atanan izinleri gruplarıdır `Space Administrator`, `User Administrator`, ve `Device Administrator`.
- **Rol atamaları** olan rol ve uzamsal grafikteki bir nesne arasındaki ilişki. Örneğin, bir kullanıcı veya hizmet sorumlusu uzamsal grafik alanında yönetme izni verilebilir.
- **Güvenlik anahtarı depoları** cihazın güvenli bir şekilde dijital çiftleri ile iletişim kurmasına izin vermek için belirtilen alanı nesnesi altında hiyerarşideki tüm cihazlar için güvenlik anahtarları sağlar.
- **Kullanıcı tanımlı işlevleri** (UDF) izin içinde uzamsal grafik işleme özelleştirilebilir algılayıcı telemetri. Örneğin, bir UDF yapabilirsiniz:
  - Bir algılayıcı değere ayarlayın.
  - Özel mantığı üzerinde sensör okumaları tabanlı gerçekleştirmek ve çıkış için bir alanı ayarlayın.
  - Meta veriler için bir alanı ekleyin.
  - Önceden tanımlanmış koşullar karşılandığında bildirimleri gönderin. Şu anda, UDF'ler JavaScript dilinde yazılabilir.
- **Matchers** olan hangi UDF'ler belirlemek nesneleri için belirli bir telemetri iletisi yürütülür.
- **Uç noktaları** burada telemetri iletilerini ve dijital İkizlerini olayları yönlendirilir, örneğin, konumlar `Event Hub`, `Service Bus`, ve `Event Grid`.

<a id="graph"></a>

## <a name="spatial-intelligence-graph"></a>Uzamsal zeka grafı

Uzamsal grafik, alanları, cihazlar ve kişiler dijital İkizlerini nesne modelinde tanımlı hiyerarşik grafiğidir. Uzamsal grafiği, devralma, filtreleme, geçiş, ölçeklenebilirlik ve genişletilebilirliği destekler. Yönetebilir ve uzamsal grafınızı REST API'lerden oluşan bir koleksiyon ile etkileşim.

Aboneliğinizde bir dijital İkizlerini hizmet dağıtırsanız, kök düğümün genel yönetici olur. Yapının tamamını tam erişim otomatik olarak verilen. Graftaki alanları alanı API'sini kullanarak sağlayın. Hizmetleri, sensör ve cihaz API'si algılayıcı API'sini kullanarak kullanarak sağlayın. [Açık kaynak Araçları](https://github.com/Azure-Samples/digital-twins-samples-csharp) Ayrıca toplu grafikte sağlamak kullanılabilir.

**Devralma Grafiği**. Devralma izinleri ve altındaki tüm düğümler için bir üst düğümden düzen özellikleri için geçerlidir. Örneğin, belirli bir düğüm üzerinde bir kullanıcıya rol atandığında, kullanıcının bu rolün izinleri verilen düğüm ve her düğüm altındaki ' var. Her bir özellik anahtarı ve belirli bir düğümde tanımlanan genişletilmiş türü, o düğümü altındaki tüm düğümler tarafından devralınır.

**Graf filtreleme**. Filtreleme, istek sonuçları daraltmak için kullanılır. Kimlikleri, adı, türleri, alt türleri, üst alanı ve ilişkili alanları göre filtreleyebilirsiniz. Algılayıcı veri türleri, özellik anahtarları ve değerleri de filtreleyebilirsiniz *çapraz*, *minLevel*, *maxLevel*ve diğer OData filtre parametreleri.

**Graf geçiş**. Uzamsal grafik derinliği ve onun üzerinden çapraz geçiş yapabilirsiniz. Derinlik için parametreleri kullanarak yukarıdan aşağı veya aşağıdan yukarıya grafiği çapraz geçirin *çapraz*, *minLevel*, ve *maxLevel*. Doğrudan üst boşluk veya avantajlarına için alt öğelerinden birine bağlı eşdüzey düğümleri almak için grafiği çapraz geçirin. Bir nesne sorguladığınızda kullanarak söz konusu nesne ilişkileri olan tüm ilgili nesneleri alabilirsiniz *içerir* alma API'leri parametresi.

**Graf ölçeklenebilirlik**. Gerçek iş yüklerinizi işleyebilmesi için graf ölçeklenebilirlik, dijital İkizlerini güvence altına alır. Dijital İkizlerini Gayrimenkul, altyapı, cihazlar, algılayıcılar, telemetri ve diğer, büyük Portföyleri temsil etmek için kullanılabilir.

**Graf genişletilebilirlik**. Genişletilebilirlik, temel alınan dijital İkizlerini nesne modelleri ile yeni türler ve ontolojileri özelleştirmek için kullanın. Genişletilebilir özellikler ve değerler ile dijital İkizlerini verilerinizi de zenginleştirilebilir.

### <a name="spatial-intelligence-graph-management-apis"></a>Uzamsal zeka graf Yönetimi API'leri

Gelen dijital İkizlerini dağıttıktan sonra [Azure portalında](https://portal.azure.com), [Swagger](https://swagger.io/tools/swagger-ui/) yönetim API'leri URL'sini otomatik olarak oluşturulur. Azure portalında görüntülenen **genel bakış** bölümü aşağıdaki biçime sahip.

```plaintext
https://YOUR_INSTANCE_NAME.YOUR_LOCATION.azuresmartspaces.net/management/swagger
```

| Ad | Şununla değiştir |
| --- | --- |
| YOUR_INSTANCE_NAME | Dijital İkizlerini örneğinizin adı |
| YOUR_LOCATION | Örneğiniz üzerinde barındırılıyorsa hangi sunucu bölge |

 Bu görüntüde tam URL'si biçiminde görünür.

![Dijital İkizlerini portalında yönetim API][2]

Uzamsal zeka grafiklerini kullanma hakkında daha fazla bilgi için Azure dijital İkizlerini yönetim API'leri önizlemesi ziyaret edin.

> [!TIP]
> Swagger önizlemesi API özelliği göstermek için sağlanmıştır.
> Konumunda barındırılan [docs.westcentralus.azuresmartspaces.net/management/swagger](https://docs.westcentralus.azuresmartspaces.net/management/swagger).

Daha fazla bilgi edinin [Swagger'ı kullanmayı](how-to-use-swagger.md).

Tüm API çağrıları kullanarak SSO'yu [OAuth](https://docs.microsoft.com/azure/active-directory/develop/v1-protocols-oauth-code). API'leri izleyin [Microsoft REST API'si yönergelerinin kuralları](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md). Çoğu koleksiyonları döndüren API'leri destekler [OData](https://www.odata.org/getting-started/basic-tutorial/#queryData) sistem sorgu seçenekleri.

## <a name="next-steps"></a>Sonraki adımlar

- Cihaz bağlantısı ve telemetri iletilerini göndermek için dijital İkizleri hakkında bilgi edinmek için [Azure dijital İkizlerini cihaz bağlantısı ve telemetri girişi](concepts-device-ingress.md).

- Yönetim API sınırlamaları ve kısıtlamalar hakkında bilgi edinmek için [Azure dijital İkizlerini API yönetimi ve sınırlamalar](concepts-service-limits.md).

<!-- Images -->
[1]: media/concepts/digital-twins-spatial-graph-building.png
[2]: media/concepts/digital-twins-spatial-graph-management-api-url.png
