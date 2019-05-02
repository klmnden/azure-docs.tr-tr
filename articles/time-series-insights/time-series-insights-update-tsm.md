---
title: Azure zaman serisi görüşleri'nde zaman serisi modeli Önizleme | Microsoft Docs
description: Azure Time Series Insights anlama zaman serisi modeli.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: eeab01146c938ec118deae08a30af85af4186a2e
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64714069"
---
# <a name="time-series-model"></a>Zaman Serisi Modeli

Bu makalede Azure zaman serisi öngörüleri önizlemesi zaman serisi modeli parçası. Bu model, özelliklerini ve oluşturmaya ve kendi modelinizi güncelleştirme kullanmaya nasıl başlayacağınızı açıklar.

Geleneksel olarak, IOT cihazlarından toplanan verileri bulmak ve sensörlerden hızlı çözümlemeniz zorlaştırır bağlamsal bilgiler eksik. Bulma ve IOT verilerini analiz etme, zaman serisi modelindeki ana hacktivism kolaylaştırmaktır. Bu amaçla, seçki, Bakım ve kullanıma hazır veri kümeleri hazırlanmalarına yardımcı olması için zaman serisi verilerini iyileştirmesini sağlayarak ulaşır. 

Bunlar cihaz ve cihaz olmayan varlıklar bağlama göre ele alınmasına çünkü zaman serisi modelleri sorgu ve gezinti önemli bir rol oynar. Zaman serisi modeli kalıcı veri zaman serisi sorgu hesaplamalar, bunlarda depolanan formülleri avantajlarından yararlanarak çalıştırır.

![TSM][1]

## <a name="key-capabilities"></a>Temel işlevler

Basit ve zaman serisi contextualization yönetmek için kolay bir hale getirmek için hedef ile zaman serisi modelindeki zaman serisi öngörüleri önizlemesinde aşağıdaki özellikleri sağlar. Yardımcı olur:

* Yazar ve hesaplamalar veya formülleri yönetme, skaler işlevler yararlanarak verileri dönüştürme, toplama işlemlerini ve benzeri.

* Gezinti ve başvuru etkinleştirin ve zaman serisi telemetri bağlam sağlamak için üst-alt ilişkileri tanımlayın.

* Örnek bölümü ile ilişkili olan özellikleri tanımlamak *örnek alanları* ve bunları hiyerarşi oluşturmak için kullanın.

## <a name="times-series-model-key-components"></a>Zaman serisi modelindeki anahtar bileşenleri

Zaman serisi modeli üç önemli bileşeni vardır:

* Zaman serisi modeli *türleri*
* Zaman serisi modeli *hiyerarşiler*
* Zaman serisi modeli *örnekleri*

## <a name="time-series-model-types"></a>Zaman serisi modeli türleri

Zaman serisi modeli *türleri* değişkenleri veya hesaplamalar yapan formüller tanımlamanıza yardımcı olur. Belirli bir zaman serisi görüşleri örneğiyle ilişkili türleridir. Bir türü veya daha fazla değişken olabilir. Örneğin, bir zaman serisi görüşleri örneği türü olabilir *sıcaklık algılayıcısı*, değişkenleri oluşur *ortalama sıcaklık*, *min sıcaklık*ve *max sıcaklık*. Time Series Insights akan verileri başladığında varsayılan türü oluştururuz. Varsayılan türü alınır ve modeli ayarları güncelleştirildi. Varsayılan türleri, olayları sayar bir değişkene sahip.

## <a name="time-series-model-type-json-example"></a>Zaman serisi modeli türü JSON örneği

Örnek:

```JSON
{
    "name": "SampleType",
    "description": "This is sample type",
    "variables": {
        "Avg Temperature": {
            "kind": "numeric",
            "filter": null,
            "value": { "tsx": "$event.temperature.Double" },
            "aggregation": {"tsx": "avg($value)"}
        },
        "Count Temperature": {
            "kind": "aggregate",
            "filter": null,
            "value": null,
            "aggregation": {"tsx": "count()"}
        }
    }
}
```

Zaman serisi modeli türleri hakkında daha fazla bilgi için bkz. [başvuru belgeleri](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#types-api).

## <a name="variables"></a>Değişkenler

Zaman serisi görüşleri türlerinin değişkenleri, olaylarından değerleri üzerinde adlandırılmış hesaplamalar vardır. Zaman serisi görüşleri değişken tanımları formül ve hesaplama kuralları içerir. Değişken tanımları içeren *tür*, *değer*, *filtre*, *azaltma*, ve *sınırları*. Değişkenleri, zaman serisi modelindeki tür tanımında depolanır ve satır içi saklı tanımını geçersiz kılmak için sorgu API'leri aracılığıyla sağlanabilir.

Değişken tanımları için bir gösterge olarak aşağıdaki matris çalışır:

![tablo][2]

### <a name="variable-kind"></a>Değişken türü

Aşağıdaki değişken türleri desteklenir:

* *Sayısal*
* *Toplama*

### <a name="variable-filter"></a>Değişken filtresi

Değişken filtreleri koşullara göre hesaplama için kabul satır sayısını sınırlamak için bir isteğe bağlı bir filtre yan tümcesi belirtin.

### <a name="variable-value"></a>Değişken değeri

Değişken değerleri ve hesaplamayı kullanılmalıdır. Diyoruz olayları sütunundadır.

### <a name="variable-aggregation"></a>Değişken toplama

Toplama işlevi değişkenin hesaplama parçası sağlar. Time Series Insights destekleyen normal toplamlar (yani, *min*, *max*, *ortalama*, *toplam*, ve *sayısı*).

## <a name="time-series-model-hierarchies"></a>Zaman serisi modeli hiyerarşiler

Hiyerarşileri, özellik adları ve aralarındaki ilişkiler belirterek örneklerini düzenleyin. Tek bir hiyerarşi veya birden çok hiyerarşi olabilir. Verilerinizi geçerli bir parçası olması gerekmez, ancak her örneği için bir hiyerarşi eşlenmesi gerekir. Zaman serisi modeli örneği, tek bir hiyerarşi veya birden çok hiyerarşi eşleyebilirsiniz.

Hiyerarşileri tarafından tanımlanan *hiyerarşi kimliği*, *adı*, ve *kaynak*. Hiyerarşiler kullanıcılar oluşturmak istediğiniz hiyerarşinin yukarıdan aşağıya üst-alt sırasıdır bir yolu vardır. Üst-alt özellikleri harita *örnek alanları*.

### <a name="time-series-model-hierarchy-json-example"></a>Zaman serisi modeli hiyerarşi JSON örneği

Örnek:

```JSON
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
```

Zaman serisi modeli Hiyerarşiler hakkında daha fazla bilgi için bkz: [başvuru belgeleri](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#hierarchies-api).

### <a name="hierarchy-definition-behavior"></a>Hiyerarşi tanımı davranışı

Hiyerarşi H1 sahip olduğu aşağıdaki örneği göz önünde bulundurun *oluşturmaya*, *kat*, ve *odası* kendi tanımının bir parçası olarak:

```plaintext
 H1 = [“building”, “floor”, “room”]
```

Yapılandırmanıza bağlı olarak *örnek alanları*, hiyerarşi öznitelikleri ve değerleri aşağıdaki tabloda gösterildiği gibi görünür:

| Zaman Serisi Kimliği | Örnek alanları |
| --- | --- |
| ID1 | "oluşturma" = "1000"kat "", "10", "odası" = "55" =  |
| ID2 | "oluşturma" = "1000", "odası" = "55" |
| ID3 | "kat" = "10" |
| ID4 | "oluşturma" = "1000"kat "", "10" =  |
| ID5 | "Oluşturma", "kat" veya "odası" hiçbiri ayarlayın |

Önceki örnekte ıd1 ve ıd4 gösterir Azure Time Series Insights gezgininin H1 hiyerarşisinde bir parçası olarak ve rest altında sınıflandırılan *ana öğesiz örnekleri* belirtilen veri hiyerarşi için uygun değilsiniz için.

## <a name="time-series-model-instances"></a>Zaman serisi modeli örnekleri

Zaman serisi örneklerdir. Çoğu durumda *DeviceID* veya *AssetID* ortamındaki varlık benzersiz tanımlayıcısıdır. Örneğiniz kendileriyle ilişkili tanımlayıcı bilgiler örnek özelliklerini çağrılır. En az örnek özelliklerini hiyerarşisi bilgileri içerir. Bunlar, üretici, operatör ya da son hizmet tarihi gibi kullanışlı, açıklayıcı verileri de içerebilir.

Örnekleri tarafından tanımlanan *TypeID*, *timeSeriesId*, *adı*, *açıklama*, *hierarchyIds* , ve *instanceFields*. Her örneği için tek bir eşler *türü*ve bir veya daha fazla hiyerarşi. Örnekleri, hiyerarşileri ve ek tüm özellikler devralır *instanceFields* daha fazla örnek özellik tanımı eklenebilir.

*instanceFields* örneği ve bir örneğini tanımlayan statik verilerin özellikleridir. Arama işlemleri gerçekleştirmek için dizin de desteklerken hiyerarşi veya hiyerarşi olmayan özellik değerlerini tanımlarlar.

*Adı* özelliği isteğe bağlıdır ve büyük küçük harfe duyarlı. Varsa *adı* olduğundan kullanılamıyor, bu zaman serisi kimliği için varsayılan olarak kullanılır Varsa bir *adı* zaman serisi kimliği (kılavuz Gezgini'nde grafikleri aşağıda) kutusu içinde kullanılabilir olmaya devam edecektir sağlanır. 

## <a name="time-series-model-instance-json-example"></a>Zaman serisi modeli örnek JSON örneği

Örnek:

```JSON
{
    "typeId": "1be09af9-f089-4d6b-9f0b-48018b5f7393",
    "timeSeriesId": ["sampleTimeSeriesId"],
    "name": "sampleName",
    "description": "Sample Instance",
    "hierarchyIds": [
        "1643004c-0a84-48a5-80e5-7688c5ae9295"
    ],
    "instanceFields": {
        "state": "California",
        "city": "Los Angeles"
    }
}
```

Zaman serisi modeli örnekleri hakkında daha fazla bilgi için bkz. [başvuru belgeleri](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#instances-api).

## <a name="time-series-model-settings-example"></a>Zaman serisi modeli ayarları örneği

Örnek:

```JSON
{
    "modelSettings": {
        "name": "DefaultModel",
        "timeSeriesIdProperties": [
            {
                "name": "id",
                "type": "String"
            }
        ],
        "defaultTypeId": "1be09af9-f089-4d6b-9f0b-48018b5f7393"
    }
}
```

Zaman serisi modeli ayarları hakkında daha fazla bilgi için bkz. [başvuru belgeleri](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#model-settings-api).

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [Azure zaman serisi öngörüleri Önizleme depolama ve giriş](./time-series-insights-update-storage-ingress.md).

- Yeni bkz [zaman serisi modeli](https://docs.microsoft.com/rest/api/time-series-insights/preview-model).

<!-- Images -->
[1]: media/v2-update-tsm/tsm.png
[2]: media/v2-update-tsm/table.png
