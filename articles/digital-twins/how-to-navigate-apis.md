---
title: Azure dijital İkizlerini API'leri gidin | Microsoft Docs
description: Bilgi Azure dijital İkizlerini yönetim API'leri sorgulama ortak desenler nasıl.
author: kingdomofends
manager: philmea
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 1/7/2019
ms.author: v-adgera
ms.openlocfilehash: 1c5549b903e9a4768d81d601c42e621864522781
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67462015"
---
# <a name="how-to-use-azure-digital-twins-management-apis"></a>Azure dijital İkizlerini yönetim API'leri kullanma

Azure dijital İkizlerini yönetim API'leri IOT uygulamalarınız için güçlü işlevleri sağlar. Bu makalede API yapısında gezinmeniz gösterilmektedir.  

## <a name="api-summary"></a>API özeti

Aşağıdaki liste, dijital İkizlerini API'leri bileşenlerini gösterir.

* [/ boşluk](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Spaces): Bu API'ler, kurulumunuzu fiziksel konumlarda ile etkileşim kurun. Bunlar, oluşturma, silme ve fiziksel konumlarınıza biçiminde dijital eşlemeleri yönetmek yardımcı bir [uzamsal graf](concepts-objectmodel-spatialgraph.md#spatial-intelligence-graph).

* [/Devices](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Devices): Bu API'ler kurulumunuzu aygıtları ile etkileşim kurun. Bu cihazlar, algılayıcılar, bir veya daha fazla yönetebilirsiniz. Örneğin, bir cihaz, telefonunuz veya bir Raspberry Pi algılayıcı pod ya da bir Lora ağ geçidi vb.

* [/Sensors](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Sensors): Bu API'ler, cihazlarınızı ve fiziksel konumlarınıza ilişkili sensörlerden ile iletişim kurmanıza yardımcı olur. Algılayıcılar, kaydedin ve uzamsal ortamınızı yönetmek için kullanılabilen ortam değerleri gönderebilirsiniz.  

* [/Resources](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Resources): Bu API'ler dijital İkizlerini Örneğiniz için bir IOT hub gibi kaynakları ayarlamanıza yardımcı olur.

* [/ türleri](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Types): Bu API'ler, genişletilmiş türleri bu nesnelere belirli özellikleri eklemek için dijital İkizlerini nesnelerinizi ilişkilendirmek izin verir. Bu tür kolay filtreleme ve kullanıcı Arabirimi ve telemetri verilerinizi işleyen özel işlevler nesnelerin gruplama sağlar. Genişletilmiş tür örnekleri *DeviceType*, *sınıfın*, *SensorDataType*, *SpaceType*, *SpaceSubType* , *SpaceBlobType*, *SpaceResourceType*ve benzeri.

* [/ontologies](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#/Ontologies): Bu API'ler topluluklarıdır genişletilmiş türlerinin ontolojileri, yönetmenize yardımcı olur. Ontolojileri, bu gruplar fiziksel alan temsil ettikleri göre nesne türleri için adlar sağlayın. Örneğin, *BACnet* ontology belirli adları sağlar *algılayıcı türleri*, *veri türleri*, *datasubtypes*ve *dataunittypes*. Ontolojileri yönetilebilir ve hizmet tarafından oluşturulur. Kullanıcılar, yükleme ve ontolojileri kaldırın. Bir ontology yüklendiğinde, ilişkili tür adları tüm etkin ve uzamsal graftaki sağlanması hazırdır. 

* [/propertyKeys](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#/PropertyKeys): Bu API'ler için özel özellikler oluşturmak için kullanabileceğiniz, *alanları*, *cihazları*, *kullanıcılar*, ve *algılayıcılar*. Bu özellikler, anahtar/değer çiftleri oluşturulur. Bu özellikler veri türünü ayarlayarak tanımlayabileceğiniz kendi *PrimitiveDataType*. Örneğin, adında bir özellik tanımlayabilirsiniz *BasicTemperatureDeltaProcessingRefreshTime* türü *uint* sensörlerden ve her biri, algılayıcılar için sonra Ata bu özellik için bir değer. Özellik oluşturulurken bu değerleri kısıtlamaları ekleyebilirsiniz *Min* ve *en fazla* izin verilen değerler'hem de aralık *ValidationData*.

* [/matchers](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#/Matchers): Bu API'ler, gelen cihaz verilerinizden değerlendirmek istediğiniz koşulları belirtmenizi sağlar. Bkz: [bu makalede](concepts-user-defined-functions.md#matchers) daha fazla bilgi için. 

* [/userDefinedFunctions](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#/UserDefinedFunctions): Bu API'ler, oluşturmanıza izin verir, delete veya update ne zaman yürütecek bir özel işlev koşulları tarafından tanımlanan *matchers* , kurulumundan gelen verileri işlemek için oluşur. Bkz: [bu makalede](concepts-user-defined-functions.md#user-defined-functions) olarak da adlandırılan bu özel işlevler hakkında daha fazla bilgi için *kullanıcı tanımlı işlevleri*. 

* [/endpoints](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#/Endpoints): Bu API'ler, dijital İkizlerini çözümünüzün veri depolama ve analiz için diğer Azure hizmetleriyle iletişim kurabilmesi uç noktaları oluşturmanıza imkan tanır. Okuma [bu makalede](concepts-events-routing.md) daha fazla bilgi için. 

* [/keyStores](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#/KeyStores): Bu API'ler, alanları için güvenlik anahtar depoları yönetmenizi sağlar. Bu depolar güvenlik anahtarların bir koleksiyonu tutmak ve kolayca son geçerli anahtarlarını almak sağlar.

* [/ Kullanıcılar](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Users): Bu API'ler, kullanıcıları gerektiğinde bu kişiler bulmak için boşluk ile ilişkilendirmek izin verir. 

* [/ Sistem](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/System): Bu API'ler varsayılan türleri, alanları ve algılayıcılar gibi Sistem genelindeki ayarları yönetmenizi sağlar. 

* [/roleAssignments](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/RoleAssignments): Bu API'ler, kullanıcı kimliği, kullanıcı tanımlı işlev kodu vb. gibi varlıkları rollerini ilişkilendirmek izin verir. Her rol ataması ilişkilendirmek için varlık türü, ilişkilendirilecek rolü kimliği, Kiracı kimliği ve varlık ile bu ilişkiyi erişip kaynak sayısı üst sınırı tanımlayan yol varlık Kimliğini içerir. Okuma [bu makalede](security-role-based-access-control.md) daha fazla bilgi için.


## <a name="api-navigation"></a>API Gezinti

Dijital İkizlerini API'leri, filtreleme ve aşağıdaki parametreleri kullanarak grafiğinize uzamsal gezinme destekler:

- **spaceId**: API verilen alanı kimliğe göre sonuçları filtreleyen Ayrıca, bir Boole bayrağı **useParentSpace** geçerlidir [/boşluk](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Spaces) verilen yer kimliği geçerli alanı yerine üst alanına başvuruyor gösteren API'leri. 

- **minLevel** ve **maxLevel**: Kök alanları düzeyinde 1 olarak değerlendirilir. Boşluk üst düzeyde boşluk *n* düzeyindedir *n + 1*. Bu değerler ayarlandığında, belirli düzeylerde sonuçlarını filtreleyebilirsiniz. Bu kapsamlı değerler ayarlandığında. Cihazlar, algılayıcılar ve diğer nesneler, bunların en yakın alan aynı seviyede olacak şekilde değerlendirilir. Tüm nesneleri belirli bir düzeyde almak için hem ayarlamak **minLevel** ve **maxLevel** aynı değere.

- **minRelative** ve **maxRelative**: Bu filtreler verildiğinde, karşılık gelen belirli yer kimliği düzeyine göre düzeyidir:
   - Göreli düzeyi *0* belirli alan kimliği ile aynı düzeyde gibidir
   - Göreli düzeyi *1* verilen alanı kimliğinin alt olarak aynı düzeyde alanları temsil eder Göreli düzeyi *n* alanları tarafından belirtilen alandan daha düşük temsil *n* düzeyleri.
   - Göreli düzeyi *-1* üst alanı belirtilen alan ile aynı düzeyde alanları temsil eder.

- **Çapraz**: Aşağıdaki değerleri belirtildiği gibi belirtilen alan kimliğinden iki yönde çapraz sağlar.
   - **Hiçbiri**: Bu varsayılan değeri belirli bir alanı kimliği için filtreler
   - **Aşağı**: Bu, belirli bir alanı kimliği ve alt öğeleri filtreler. 
   - **Yukarı**: Bu, verilen alanı kimliği ve alt öğelerinden göre filtreler. 
   - **Yayılma**: Belirtilen alan kimliği ile aynı düzeyde uzamsal grafiğin yatay bir kısmı bu filtreler Bu ya da gereken **minRelative** veya **maxRelative** true olarak ayarlanmalıdır. 


### <a name="examples"></a>Örnekler

Aşağıdaki listede gezinmeyi bazı örnekler gösterilmektedir [/devices](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Devices) API'leri. Unutmayın yer tutucu `YOUR_MANAGEMENT_API_URL` URI biçiminde dijital İkizlerini API'leri başvurduğu `https://YOUR_INSTANCE_NAME.YOUR_LOCATION.azuresmartspaces.net/management/api/v1.0/`burada `YOUR_INSTANCE_NAME` Azure dijital İkizlerini örneğinizin adı ve `YOUR_LOCATION` örneğinizin barındırıldığı bölgedir.

- `YOUR_MANAGEMENT_API_URL/devices?maxLevel=1` kök alanları için eklenen tüm cihazları döndürür.
- `YOUR_MANAGEMENT_API_URL/devices?minLevel=2&maxLevel=4` Düzey 2, 3 veya 4 alanları için bağlı olan tüm cihazları döndürür.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId` doğrudan mySpaceId için eklenen tüm cihazları döndürür.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId&traverse=Down` mySpaceId veya alt öğelerinden birine bağlı tüm cihazları döndürür.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId&traverse=Down&minLevel=1&minRelative=true` bağlı alt öğeleri, mySpaceId, mySpaceId hariç tüm cihazları döndürür.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId&traverse=Down&minLevel=1&minRelative=true&maxLevel=1&maxRelative=true` mySpaceId yakınındaki alt öğeleri için eklenen tüm cihazları döndürür.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId&traverse=Up&maxLevel=-1&maxRelative=true` mySpaceId üst öğelerinden birine bağlı tüm cihazları döndürür.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId&traverse=Down&maxLevel=5` düzeyinde değerinden küçük veya 5'e eşit olan alt öğeleri mySpaceId bağlı tüm cihazları döndürür.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId&traverse=Span&minLevel=0&minRelative=true&maxLevel=0&maxRelative=true` mySpaceId ile aynı düzeyde olan alanlarına eklenen tüm cihazları döndürür.


## <a name="odata-support"></a>OData desteği
Çoğu /spaces üzerinde bir GET çağrısı gibi koleksiyonları döndüren API desteği genel aşağıdaki alt [OData](https://www.odata.org/getting-started/basic-tutorial/#queryData) sistem sorgu seçenekleri:  

* **$filter**
* **$orderby** 
* **$top**
* **$skip** -koleksiyonunun tamamını görüntülemek istiyorsanız, size tüm küme tek bir çağrı olarak istek ve ardından disk belleği uygulamanızda gerçekleştirin. 

$Count, $ gibi diğer sorgu seçeneklerini genişletin, $search, desteklenmeyen dikkat edin.

### <a name="examples"></a>Örnekler

Aşağıdaki listede, OData'nın sistem sorgu seçenekleri kullanarak sorguları bazı örnekler gösterilmektedir:

- `YOUR_MANAGEMENT_API_URL/devices?$top=3&$orderby=Name desc`
- `YOUR_MANAGEMENT_API_URL/keystores?$filter=endswith(Description,’space’)`
- `YOUR_MANAGEMENT_API_URL/propertykeys?$filter=Scope ne ‘Spaces’`
- `YOUR_MANAGEMENT_API_URL/resources?$filter=Size gt ‘M’`
- `YOUR_MANAGEMENT_API_URL/users?$top=4&$filter=endswith(LastName,’k’)&$orderby=LastName`
- `YOUR_MANAGEMENT_API_URL/spaces?$orderby=Name desc&$top=3&$filter=substringof('Floor’,Name)`
 

## <a name="next-steps"></a>Sonraki adımlar

Bazı ortak API sorgu desenleri öğrenmek için [ortak görevler için sorgu Azure dijital İkizlerini API'leri nasıl](how-to-query-common-apis.md).


