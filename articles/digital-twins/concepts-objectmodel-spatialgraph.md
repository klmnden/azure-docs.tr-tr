---
title: Dijital İkizlerini anlama modelleri ve uzamsal zeka graf nesnesi | Microsoft Docs
description: Kişiler, yerler ve cihazlar arasındaki ilişkileri modellemek için Azure dijital İkizlerini kullanma
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: alinast
ms.openlocfilehash: c1d66e0b58567244f8c1406ee258c9311994ff20
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50215115"
---
# <a name="understanding-digital-twins-object-models-and-spatial-intelligence-graph"></a>Dijital İkizlerini nesne modelleri ve uzamsal zeka graf anlama

Azure dijital İkizlerini fiziksel ortamları ve ilişkili cihazlar, algılayıcılar ve kişiler kapsamlı sanal temsillerini güç katan bir Azure IOT hizmetidir. Geliştirme, kendileri bir uzamsal zeka grafik içinde yer yararlı modelleri halinde düzenleme etki alanına özgü kavramları artırır. Bu kavramlar, kişiler, boşluk ve cihazlar arasındaki etkileşimler ve ilişkiler faithfully model.

_Dijital İkizlerini nesne modellerini_ etki alanına özgü kavramları, kategoriler ve özellikler açıklanmaktadır. Modelleri, kendi ihtiyaçlarınıza göre istediğiniz şekilde özelleştirilebilmesine olanak isteyen kullanıcılar tarafından önceden tanımlanmıştır. Bu önceden tanımlanmış bir dijital İkizlerini nesne modellerini birlikte oluşturan bir _Ontology_. Akıllı bir yapı 's ontology bölgeler, mekanlar, Katlar, ofisleri, bölgeleri, Konferans salonu ve odak odaları açıklayan. Bir enerji kılavuz ontology çeşitli power istasyonları, şalt, enerji kaynakları ve müşteriler açıklayan. Bu dijital İkizlerini nesne modelleri ve ontolojileri, çeşitli senaryolar ve gereksinimlerine Azure dijital İkizlerini özelleştirme izin verin.

İle _dijital İkizlerini nesne modellerini_ ve _Ontology_ yerinde bir doldurabilirsiniz bir _uzamsal graf_. Birçok ilişki arasındaki boşluk, cihazlar ve kişiler bir IOT çözümü için ilgili sanal temsillerini uzamsal grafiklerdir. Aşağıdaki diyagramda, akıllı bir yapı 's ontology kullanarak uzamsal grafik örneği gösterilmektedir.

![Dijital İkizlerini uzamsal grafik oluşturma][1]

<a id="model"></a>

Uzamsal grafik alanları, cihazlar, algılayıcılar ve kullanıcıların bir araya getirir. Her birlikte gerçek dünyada modellerin bir şekilde bağlantılıdır: 43 mekan sahip birden çok farklı alanları her dört Katlar. Kullanıcılar kendi iş istasyonları ile ilişkili ve graf kısımlarına erişim verilir.  Örneğin, bir yönetici hakları ziyaretçisi yalnızca belirli bir yapı verileri görüntüleme haklarına olabilirken, uzamsal grafiğe değişiklik yapmak için gerekir.

## <a name="digital-twins-object-models"></a>Dijital İkizlerini nesne modelleri

Bu kategoride nesnelerin dijital İkizlerini nesne modellerini destekler:

- **Alanları** sanal veya fiziksel konumlar, örneğin: `Tenant`, `Customer`, `Region`, `Venue`.
- **Cihazları** sanal veya fiziksel donanım, örneğin parçalarıdır `AwesomeCompany Device`, `Raspberry Pi 3`.
- **Algılayıcılar** olayları, örneğin algılamak nesneler `AwesomeCompany Temperature Sensor`, `AwesomeCompany Presence Sensor`.
- **Kullanıcılar** occupants ve özelliklerini tanımlar.

Diğer nesnelerin kategorileri şunlardır:

- **Kaynakları** için bir alanı eklenmiş ve genellikle uzamsal Graph'te nesneleri tarafından örneğin kullanılmak üzere Azure kaynaklarını temsil `IoTHub`.
- **Blobları** nesnelerine (örneğin, boşluk, cihazlar, algılayıcılar ve kullanıcılar) bağlı olan ve MIME türü ve meta dosyaları olarak örneğin kullanılan `maps`, `pictures`, `manuals`.
- **Türler Genişletilmiş** belirli özelliklere sahip varlıklar örneğin büyütmek Genişletilebilir numaralandırmalar olan `SpaceType`, `SpaceSubtype`.
- **Ontolojileri** genişletilmiş türleri kümesi örneğin temsil `Default`, `Building`, `BACnet`, `EnergyGrid`.
- **Özellik anahtarları ve değerleri** alanları, cihazlar, algılayıcılar ve kullanıcıların özel özellikleri sahiptir. Bunlar gibi yerleşik özelliklere ek olarak, kullanılabilir `DeltaProcessingRefreshTime` anahtar olarak ve `10` değer olarak.
- **Rolleri** kullanıcılara ve cihazlara uzamsal grafikte, örneğin atanan izinleri gruplarıdır `Space Administrator`, `User Administrator`, `Device Administrator`.
- **Rol atamaları** olan rol ve örneğin bir kullanıcı ya da bir uzamsal grafik alanında yönetmek için bir hizmet asıl izin verme uzamsal grafikteki bir nesne arasındaki ilişki.
- **Güvenlik anahtarı depoları** dijital İkizlerini hizmetiyle güvenli iletişim kurabilmesi cihazın izin vermek için belirli bir alanı nesnesi hiyerarşideki tüm cihazlar için güvenlik anahtarları sağlar.
- **Kullanıcı tanımlı işlevler** veya **UDF'ler** özelleştirilebilir algılayıcı telemetri içinde uzamsal grafik işleme izin verin. Örneğin, bir UDF algılayıcı değerini ayarlayın, sensör okumaları üzerinde temel özel mantığı gerçekleştirmek ve çıkış için bir alanı ayarlayın, meta verileri için bir alanı ekleme ve önceden tanımlanmış koşullar karşılandığında bildirimleri göndermek. Şu anda UDF'ler JavaScript dilinde yazılabilir.
- **Matchers** hangi UDF'ler belirlemek nesneleri için belirli bir telemetri iletisi yürütülecek olan.
- **Uç noktaları** burada telemetri iletilerini ve dijital İkizlerini olayları yönlendirilir, örneğin Konumlar `Event Hub`, `Service Bus`, `Event Grid`.

<a id="graph"></a>

## <a name="spatial-intelligence-graph"></a>Uzamsal zeka grafı

**Uzamsal grafik** hiyerarşik Graph alanları, cihazlar ve kişiler tanımlanan **dijital İkizlerini nesne modeli**. Uzamsal destekledikleri _devralma_, _filtreleme_, _geçiş_, _ölçeklenebilirlik_, ve _genişletilebilirliği_ . Kullanıcılar, yönetin ve bunların uzamsal graph REST API'leri (aşağıya bakın) koleksiyonu ile etkileşim.

Dijital İkizlerini hizmet aboneliklerinde dağıtır kullanıcı otomatik olarak tüm yapısına yönelik tam erişim verme kök düğümü genel Yöneticisi olur. Bu kullanıcı ardından boşluk API kullanarak grafiği boşluk sağlayabilirsiniz. Cihazları cihaz API'si kullanılarak sağlanan, algılayıcılar algılayıcı API vb. kullanarak sağlanan. Ayrıca sunuyoruz [açık kaynak Araçları](https://github.com/Azure-Samples/digital-twins-samples-csharp) toplu grafikte sağlamak için.

Graf _devralma_ izinleri ve altındaki tüm düğümler için bir üst düğümden düzen özellikleri için geçerlidir. Örneğin, belirli bir düğümde bir kullanıcı için rol atandığında kullanıcı bu rolün izinleri verilen düğüm ve her düğüm altındaki sahip olur. Ayrıca, her bir özellik anahtarı ve belirli bir düğümde tanımlanan genişletilmiş tür o düğümü altındaki tüm düğümler tarafından devralınır.

Graf _filtreleme_ kimlikleri, adı, türleri, alt türleri, üst alanı, ilişkili alanları, algılayıcı veri türleri, özellik anahtarları ve değerleri, istek sonuçları daraltmak kullanıcıların sağlayan *çapraz*,  *minLevel*, *maxLevel*ve diğer OData filtre parametreleri.

Graf _geçiş_ uzamsal grafik derinliği ve onun üzerinden gitmek kullanıcıların sağlar. Derinlik için graf yukarıdan aşağıya veya aşağıdan yukarıya Gezinti parametreleri kullanarak geçiş yapılabilir *çapraz*, *minLevel*, *maxLevel*. Avantajlarına için doğrudan üst boşlukla veya alt öğelerinden birine bağlı eşdüzey düğümleri almak için graph çıkıldığında. Bir nesne sorgulanırken kullanarak nesne ilişkileri olan tüm ilgili nesneleri alabilir *içerir* alma API'leri parametresi.

Azure dijital İkizlerini garanti eder, grafik _ölçeklenebilirlik_, gerçek iş yüklerinizi işleyebilir. Dijital İkizlerini Gayrimenkul, altyapı, cihazlar, algılayıcılar, telemetri ve diğer, büyük Portföyleri temsil etmek için kullanılabilir.

Graf _genişletilebilirlik_ kullanıcıların yeni türler ve ontolojileri ile temel alınan dijital İkizlerini nesne modellerini özelleştirerek olanak tanır. Genişletilebilir özellikler ve değerler ile dijital ikizlerini veri de zenginleştirilebilir.

### <a name="spatial-intelligence-graph-management-apis"></a>Uzamsal zeka graf Yönetimi API'leri

Azure dijital İkizlerini alanından dağıttığınızda [Azure portalında](https://portal.azure.com), [Swagger](https://swagger.io/tools/swagger-ui/) yönetim API'leri URL'sini otomatik olarak oluşturulur ve Azure Portalı'nda kişinin görüntülenen **genelbakış** bölümü aşağıdaki biçime sahip:

```plaintext
https://yourInstanceName.yourLocation.azuresmartspaces.net/management/swagger
```

| Özel öznitelik adı | Değiştirin |
| --- | --- |
| *örneğinizinadı* | Azure dijital İkizlerini örneğinizin adı |
| *yourLocation* | Örneğiniz üzerinde barındırılıyorsa hangi sunucu bölge |

 Tam URL biçimi, aşağıdaki resimde kullanılan görülebilir:

![Dijital İkizlerini portalında yönetim API][2]

Uzamsal zeka grafiklerini kullanarak hakkında ayrıntılı bilgi için Azure dijital İkizlerini yönetim API'leri önizlemesi ziyaret edin.

> [!TIP]
> Swagger önizlemesi API özelliği göstermek için sağlanmıştır.
> Konumunda barındırılan [docs.westcentralus.azuresmartspaces.net/management/swagger](https://docs.westcentralus.azuresmartspaces.net/management/swagger).

Daha fazla bilgi edinin [Swagger'ı kullanmayı](how-to-use-swagger.md).

Tüm API çağrıları kullanarak SSO'yu [OAuth](https://docs.microsoft.com/azure/active-directory/develop/v1-protocols-oauth-code). API'leri izleyin [Microsoft REST API'si yönergelerinin kuralları](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md). Çoğu koleksiyonları döndüren API'leri destekler [OData](http://www.odata.org/getting-started/basic-tutorial/#queryData) sistem sorgu seçenekleri.

## <a name="next-steps"></a>Sonraki adımlar

Cihaz bağlantısı ve dijital İkizlerini Azure hizmetine telemetri iletilerini göndermek nasıl hakkında bilgi edinmek için [Azure dijital İkizlerini cihaz bağlantısı ve telemetri girişi](concepts-device-ingress.md).

Yönetim API sınırlamaları ve kısıtlamalar hakkında bilgi edinmek için [Azure dijital İkizlerini API yönetimi ve sınırlamalar](concepts-service-limits.md).

<!-- Images -->
[1]: media/concepts/digital-twins-spatial-graph-building.png
[2]: media/concepts/digital-twins-spatial-graph-management-api-url.png
