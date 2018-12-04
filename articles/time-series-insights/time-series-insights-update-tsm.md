---
title: Zaman serisi modeli | Microsoft Docs
description: Zaman serisi modeli anlama
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 11/30/2018
ms.openlocfilehash: 4cb83b61068922002c0c308f4d129a2e069beadd
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856927"
---
# <a name="time-series-model"></a>Zaman serisi modeli

Bu belge ayrıntıları **zaman serisi modeli** Azure zaman serisi öngörüleri (TSI) güncelleştirmenin parçası (TSM). Bu model, özelliklerini ve oluşturmaya ve kendi modelinizi güncelleştirme kullanmaya nasıl başlayacağınızı açıklar.

Geleneksel olarak, IOT cihazlarından toplanan verileri bulmak ve sensörlerden hızlı çözümlemeniz zorlaştıran bağlamsal bilgiler eksik. Bulma ve seçki, Bakım ve kullanıma hazır veri kümeleri hazırlanmalarına yardımcı olması için zaman serisi verilerini iyileştirmesini sağlayarak IOT verilerini analiz etme TSM ana hacktivism kolaylaştırmaktır. **Zaman serisi modelleri** sorgular ve gezinti önemli bir rol, cihaz ve cihaz olmayan varlık bağlama göre ele alınmasına beri Yürüt. Verileri, bunlarda depolanan formülleri yararlanarak TSM powers zaman serisi sorgularını hesaplamalar kalıcı hale.

![TSM][1]

## <a name="key-capabilities"></a>Temel işlevler

Basit ve zaman serisi contextualization yönetmek için kolay bir hale getirmek için hedefle TSM Azure Time Series Insights (Önizleme) içinde aşağıdaki özellikleri sağlar:

* Yazar ve hesaplamalar veya skaler İşlevler, toplama işlemleri yararlanarak verileri dönüştürmek için formülleri yönetme olanağı.
* Gezinti ve zaman serisi telemetri bağlam sağlamak amacıyla başvurusu etkinleştirmek için üst alt öğe ilişkilerini tanımlama.
* Örnek alanları örnekleri bölümü ile ilişkili özellikler tanımlayın ve bunları hiyerarşi oluşturmak için kullanın.

## <a name="times-series-model-key-components"></a>Zaman serisi modelindeki anahtar bileşenleri

TSM üç ana bileşenleri şunlardır:

* **Zaman serisi modeli** *türleri*
* **Zaman serisi modeli** *hiyerarşiler*
* **Zaman serisi modeli** *örnekleri*

## <a name="time-series-model-types"></a>Zaman serisi modeli türleri

**Zaman serisi modeli** *türleri* tanımlayan değişkenleri veya hesaplamalar yapan formüller etkinleştirmeniz ve belirli bir TSI örneği ile ilişkili. Bir türü veya daha fazla değişken olabilir. Örneğin, bir TSI örneği türü olabilir **sıcaklık algılayıcısı**, değişkenleri oluşur: *ortalama sıcaklık*, *min sıcaklık*, ve *max Sıcaklık*. Verileri TSI akan başladığında varsayılan türü oluştururuz. Alınır ve modeli ayarları güncelleştirildi. Varsayılan türleri olayları sayar bir değişkene sahip olur.

## <a name="time-series-model-type-json-request-and-response-example"></a>İstek ve yanıt zaman serisi modeli türü JSON örneği

Bir HTTP POST isteği verilen:

```plaintext
https://YOUR_ENVIROMENT.env.timeseries.azure.com/timeseries/types/$batch?api-version=API_VERSION
```

| Ad | Açıklama | Örnek |
| --- | --- | --- |
| YOUR_ENVIRONMENT  |  Ortamınızın adını  | `environment123` |
| API_VERSION  |  API Belirtimi | `2018-11-01-preview` |

Aşağıdaki JSON gövdesi ve değişken özniteliklere sahip:

| Öznitelik | Gerekli veya isteğe bağlı |
| --- | --- |
| **tür**  |  Gerekli  |
| **Filtre**  |  İsteğe bağlı |
| **value**  | Boş veya belirtilmedi  |
| **ilişkilendirme**  |  İsteğe bağlı |
| **Toplama**  |  Gerekli |

```JSON
{
    "get": null,
    "put": [
        {
            "name": "SampleType",
            "description": "This is type 2",
            "variables": {
                "Avg Temperature": {
                    "kind": "numeric",
                    "filter": null,
                    "value": { "tsx": "$event.temperature.Double" },
                    "interpolation": "None",
                    "aggregation": {"tsx": "avg($value)"}
                },
                "Count Temperature": {
                    "kind": "aggregate",
                    "filter": null,
                    "value": null,
                    "interpolation": "None",
                    "aggregation": {"tsx": "count()"}
                },
                "Min Temperature": {
                    "kind": "aggregate",
                    "filter": null,
                    "value": null,
                    "interpolation": "None",
                    "aggregation": {"tsx": "min($event.temperature)"}
                },
            }
        }
    ]
}
```

Yanıt:

```JSON
{
    "get": null,
    "put": [
        {
            "timeSeriesType": {
                "id": "fc4f526c-da6e-4b85-87f7-16f6cf9b69be",
                "name": "type2",
                "description": "This is type 2",
                "variables": {
                    "Avg Temperature": {
                        "kind": "numeric",
                        "filter": null,
                        "value": { "tsx": "$event.temperature.Double" },
                        "interpolation": "None",
                        "aggregation": {"tsx": "avg($value)"}
                    },
                    "Count Temperature": {
                        "kind": "aggregate",
                        "filter": null,
                        "value": null,
                        "interpolation": "None",
                        "aggregation": {"tsx": "count()"}
                    },
                    "Min Temperature": {
                        "kind": "aggregate",
                        "filter": null,
                        "value": null,
                        "interpolation": "None",
                        "aggregation": {"tsx": "min($event.temperature)"}
                    }
                }
            },
            "error": null
        }
    ]
}
``````

A **varsayılan** *türü* JSON yanıtı:

```JSON
{
    "modelSettings": {
        "name": "DefaultModel",
        "timeSeriesIdProperties": [
            {
                "name": "someType1",
                "type": "String"
            }
        ],
        "defaultTypeId": "1be09af9-f089-4d6b-9f0b-48018b5f7393"
    }
}
```

## <a name="variables"></a>Değişkenler

Azure TSI türlerinin değişkenleri vardır, bunlar olaylardan değerleri üzerinde hesaplamalar adlandırılır. TSI değişken tanımlar, formül ve hesaplama kuralları içerir. Tür, değer, filtre, azaltma ve sınırları değişken tanımları içerir. Değişkenleri TSM tür tanımında depolanır ve satır içi saklı tanımını geçersiz kılmak için sorgu API'leri aracılığıyla sağlanabilir.

Değişken tanımları için bir gösterge olarak aşağıdaki matris çalışır:

![tablo][2]

### <a name="variable-kind"></a>Değişken türü

Aşağıdaki değişken türleri desteklenir:

* Sayısal – sürekli

### <a name="variable-filter"></a>Değişken filtresi

Değişken filtreleri koşullara göre hesaplama için kabul satır sayısını sınırlamak için bir isteğe bağlı bir filtre yan tümcesi belirtin.

### <a name="variable-value"></a>Değişken değeri

Değişken değerleri ve hesaplamayı kullanılmalıdır. Diyoruz olayları sütunundadır.

### <a name="variable-interpolation"></a>Değişken ilişkilendirme

Bir aralıktaki her bir değer, bir dizi dönüştürme işlemi, bir değişken azaltma çağrılır. Değişken indirimleri Toplu Kaydedilmiş veri kaynağından olabilir veya ilişkilendirme ile ilişkilendirme kullanarak ve örnekleme toplama ya da yeniden oluşturulan sinyalleri sinyalleri reconstructed. İlişkilendirme için değişken sınırları eklenebilir, bunlar arama aralığının dışında olayları dahil etmek hesaplamalar izin verir.

Aşağıdaki değişken ilişkilendirme (Önizleme) Azure TSI destekler: `linear`, `stepright`, ve `none`.

### <a name="variable-aggregation"></a>Değişken toplama

Değişkeni toplu işlevi olan hesaplamayı parçası sağlar. Değişken ilişkilendirme ise `null` veya `none`, TSI normal toplamalar destekleyecek sonra (yani, **min**, **max**, **ortalama**, **Topla** , ve **sayısı**). Değişken ilişkilendirme ise `stepright` veya `linear`, TSI destekleyecek sonra **twmin**, **twmax**, **twavg**, ve **twsum**. Sayısı, ilişkilendirme belirtilemez.

## <a name="time-series-model-hierarchies"></a>Zaman serisi modeli hiyerarşiler

Hiyerarşileri, özellik adları ve aralarındaki ilişkiler belirterek örneklerini düzenleyin. Tek bir hiyerarşi veya birden çok hiyerarşi olabilir. Ayrıca bunlar, verilerinizi geçerli bir parçası olması gerekmez, ancak her örneği için bir hiyerarşi eşlenmesi gerekir. TSM örneği, tek bir hiyerarşi veya birden çok hiyerarşi eşleyebilirsiniz.

Hiyerarşileri tarafından tanımlanan **hiyerarşi kimliği**, **adı**, ve **kaynak**. Yolları Hiyerarşiler varsa, yukarıdan aşağıya üst-alt öğe sırasını oluşturmak için kullanıcının istediği hiyerarşiyi bir yoludur. Üst/alt özellikleri örnek alanları eşleyin.

### <a name="time-series-model-hierarchy-json-request-and-response-example"></a>İstek ve yanıt zaman serisi modeli hiyerarşi JSON örneği

Bir HTTP POST isteği verilen:

```plaintext
https://YOUR_ENVIROMENT.env.timeseries.azure.com/timeseries/hierarchies/$batch?api-version=API_VERSION
```

| Ad | Açıklama | Örnek |
| --- | --- | --- |
| YOUR_ENVIRONMENT  |  Ortamınızın adını  | `environment123` |
| API_VERSION  |  API Belirtimi | `2018-11-01-preview` |

JSON gövdesi ile:

```JSON
{
    "get": null,
    "put": [
        {
            "id": "4c6f1231-f632-4d6f-9b63-e366d04175e3",
            "name": "Location",
            "source": {
                "instanceFieldNames": [
                    "state",
                    "city"
                ]
            }
        }
    ]
}
```

Yanıt:

```JSON
{
    "get": null,
    "put": [
        {
            "hierarchy": {
                "id": "4c6f1231-f632-4d6f-9b63-e366d04175e3",
                "name": "Location",
                "source": {
                    "instanceFieldNames": [
                        "state",
                        "city"
                    ]
                }
            },
            "error": null
        }
    ]
}
```

### <a name="hierarchy-definition-behavior"></a>Hiyerarşi tanımı davranışı

Hiyerarşi H1 "oluşturma", "kat" ve "odası" kendi tanımının bir parçası olarak sahip olduğu aşağıdaki örneği göz önünde bulundurun:

```plaintext
 H1 = [“building”, “floor”, “room”]
```

Örnek alanları üzerinde bağlı olarak, hiyerarşisi öznitelikleri ve değerleri görüntülenir gösterir: 

| Zaman serisi kimliği | Örnek alanları |
| --- | --- |
| ID1 | "oluşturma" = "1000"kat "", "10", "odası" = "55" =  |
| ID2 | "oluşturma" = "1000", "odası" = "55" |
| ID3 |  "kat" = "10" |
| ID4 | "oluşturma" = "1000"kat "", "10" =  |
| ID5 | |

Yukarıdaki örnekte, UI/UX, H1 hiyerarşisinde bir parçası olarak ıd1 gösterir sırasında rest altında sınıflandırılan `Unparented Instances` belirtilen veri hiyerarşisine uymayan olduğundan.

## <a name="time-series-model-instances"></a>Zaman serisi modeli örnekleri

Zaman serisi örneklerdir. Çoğu durumda bu olacaktır *DeviceID* veya *AssetID* varlık ortamdaki diğer bir deyişle benzersiz tanımlayıcısı. Örneğiniz kendileriyle ilişkili tanımlayıcı bilgiler örnek özelliklerini çağrılır. En az örnek özelliklerini hiyerarşisi bilgileri içerir. Bunlar, üretici, operatör ya da son hizmet tarihi gibi kullanışlı, açıklayıcı verileri de içerebilir.

Örnekleri tarafından tanımlanan *timeSeriesId*, *TypeID*, *HierarchyId*, ve *instanceFields*. Her örneği için tek bir eşler *türü*ve bir veya daha fazla hiyerarşi. Örnekleri hiyerarşiden, ek sırasında tüm özellikler devralır *instanceFields* daha fazla örnek özellik tanımı eklenebilir.

*instanceFields* örneği ve bir örneğini tanımlayan statik verilerin özellikleridir. Arama işlemleri gerçekleştirmek için dizin de desteklerken hiyerarşi veya hiyerarşi olmayan özellik değerlerini tanımlarlar.

## <a name="time-series-model-instance-json-request-and-response-example"></a>İstek ve yanıt zaman serisi modeli örnek JSON örneği

Bir HTTP POST isteği verilen:

```plaintext
https://YOUR_ENVIROMENT.env.timeseries.azure.com/timeseries/instances/$batch?api-version=API_VERSION
```

| Ad | Açıklama | Örnek |
| --- | --- | --- |
| YOUR_ENVIRONMENT  |  Ortamınızın adını  | `environment123` |
| API_VERSION  |  API Belirtimi | `2018-11-01-preview` |

JSON gövdesi ile:

```JSON
{
    "get": null,
    "put": [
        {
            "name": "SampleName",
            "typeId": "1be09af9-f089-4d6b-9f0b-48018b5f7393",
            "timeSeriesId": [
                "samplePartitionKeyValueOne"
            ],
            "description": "floor 100",
            "hierarchyIds": [
                "e37a4666-9650-42e6-a6d2-788f12d11158"
            ],
            "instanceFields": {
                "state": "California",
                "city": "Los Angeles"
            }
        }
    ]
}
```

Yanıt:

```JSON
{
    "get": null,
    "put": [
        {
            "instance": null,
            "error": null
        },
        {
            "instance": null,
            "error": null
        }
    ]
}
```

## <a name="time-series-model-settings-example"></a>Zaman serisi modeli ayarları örneği

Bir HTTP POST isteği verilen:

```plaintext
https://YOUR_ENVIROMENT.env.timeseries.azure.com/timeseries/modelSettings?api-version=API_VERSION
```

| Ad | Açıklama | Örnek |
| --- | --- | --- |
| YOUR_ENVIRONMENT  |  Ortamınızın adını  | `environment123` |
| API_VERSION  |  API Belirtimi | `2018-11-01-preview` |

Yanıt:

```JSON
{
    "modelSettings": {
        "name": "DefaultModel",
        "timeSeriesIdProperties": [
            {
                "name": "someType1",
                "type": "String"
            }
        ],
        "defaultTypeId": "1be09af9-f089-4d6b-9f0b-48018b5f7393"
    }
}
```

## <a name="time-series-model-limits"></a>Zaman serisi modeli sınırları

| Parametre |   Sınırlar |
| --- | --- |
| Model varlıklar (türleri, hiyerarşiler ve örnekler) için nesne boyutu|  32 KB özelliklerini içerir |
| Azure portalı yapılandırılan TSID özelliği olarak izin verilen anahtarlar |   En fazla 3 |
| Bir ortamda en fazla # türleri |    1000|
| Türü değişkenler maksimum sayısı |    50|
| Bir ortamda en fazla # hiyerarşiler|   32|
| En fazla hiyerarşisi derinliği | 32|
| En fazla # 1 örneğiyle ilişkili hiyerarşiler   |21|
| Bir ortamda en fazla # örnekleri |    500,000|
| Örnek alanları örnek başına en fazla sayısı |   50|
| Saniye başına model nesneleri upsert/güncelleştirme/silme işlemi   |saniye başına 100|
| Zaman serisi modeli API'si istek boyutu: Batch |  8 MB veya 1000 model nesnelerini (hangisi önce gerçekleşirse)|
| Zaman serisi modeli istek boyutu: arama ve önerin   | 32 KB|
| Zaman serisi modeli API'si istek boyutu: Batch    | 32 MB|
| Arama/Öner 32 MB'dir|  32 MB|

[!INCLUDE [tsi-update-docs](../../includes/time-series-insights-update-documents.md)]

## <a name="next-steps"></a>Sonraki adımlar

Okuma [Azure TSI (Önizleme) depolama ve giriş](./time-series-insights-update-storage-ingress.md).

Yeni hakkında okuyun [zaman serisi modeli](./time-series-insights-update-tsm.md).

<!-- Images -->
[1]: media/v2-update-tsm/tsm.png
[2]: media/v2-update-tsm/table.png
