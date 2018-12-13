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
ms.date: 12/04/2018
ms.openlocfilehash: 5d5f94aebcd55474385e903246ce7945586456dd
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52890424"
---
# <a name="time-series-model"></a>Zaman Serisi Modeli

Bu belge ayrıntıları **zaman serisi modeli** Azure zaman serisi öngörüleri (TSI) güncelleştirmenin parçası (TSM). Bu model, özelliklerini ve oluşturmaya ve kendi modelinizi güncelleştirme kullanmaya nasıl başlayacağınızı açıklar.

Geleneksel olarak, IOT cihazlarından toplanan verileri bulmak ve sensörlerden hızlı çözümlemeniz zorlaştıran bağlamsal bilgiler eksik. Bulma ve seçki, Bakım ve kullanıma hazır veri kümeleri hazırlanmalarına yardımcı olması için zaman serisi verilerini iyileştirmesini sağlayarak IOT verilerini analiz etme TSM ana hacktivism kolaylaştırmaktır. Bunlar cihaz ve cihaz olmayan varlıklar bağlama göre ele alınmasına beri TSMs sorgu ve gezinti önemli bir rol oynar. Verileri, bunlarda depolanan formülleri yararlanarak TSM powers zaman serisi sorgularını hesaplamalar kalıcı hale.

![TSM][1]

## <a name="key-capabilities"></a>Temel işlevler

Basit ve zaman serisi contextualization yönetmek için kolay bir hale getirmek için hedefle TSM Azure TSI (Önizleme) içinde aşağıdaki özellikleri sağlar:

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
``````

Zaman serisi modeli türleri hakkında daha fazla bilgiyi [başvuru belgeleri](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#types-api).

## <a name="variables"></a>Değişkenler

Azure TSI türlerinin değişkenleri, olaylarından değerleri üzerinde adlandırılmış hesaplamalar vardır. TSI değişken tanımlar, formül ve hesaplama kuralları içerir. Tür, değer, filtre, azaltma ve sınırları değişken tanımları içerir. Değişkenleri TSM tür tanımında depolanır ve satır içi saklı tanımını geçersiz kılmak için sorgu API'leri aracılığıyla sağlanabilir.

Değişken tanımları için bir gösterge olarak aşağıdaki matris çalışır:

![tablo][2]

### <a name="variable-kind"></a>Değişken türü

Aşağıdaki değişken türleri desteklenir:

* Sayısal
* Toplama

### <a name="variable-filter"></a>Değişken filtresi

Değişken filtreleri koşullara göre hesaplama için kabul satır sayısını sınırlamak için bir isteğe bağlı bir filtre yan tümcesi belirtin.

### <a name="variable-value"></a>Değişken değeri

Değişken değerleri ve hesaplamayı kullanılmalıdır. Diyoruz olayları sütunundadır.

### <a name="variable-aggregation"></a>Değişken toplama

Değişkeni toplu işlevi olan hesaplamayı parçası sağlar. TSI normal toplamalar desteği (yani, **min**, **max**, **ortalama**, **toplam**, ve **sayısı**).

## <a name="time-series-model-hierarchies"></a>Zaman serisi modeli hiyerarşiler

Hiyerarşileri, özellik adları ve aralarındaki ilişkiler belirterek örneklerini düzenleyin. Tek bir hiyerarşi veya birden çok hiyerarşi olabilir. Ayrıca bunlar, verilerinizi geçerli bir parçası olması gerekmez, ancak her örneği için bir hiyerarşi eşlenmesi gerekir. TSM örneği, tek bir hiyerarşi veya birden çok hiyerarşi eşleyebilirsiniz.

Hiyerarşileri tarafından tanımlanan **hiyerarşi kimliği**, **adı**, ve **kaynak**. Yolları Hiyerarşiler varsa, yukarıdan aşağıya üst-alt öğe sırasını oluşturmak için kullanıcının istediği hiyerarşiyi bir yoludur. Üst/alt özellikleri örnek alanları eşleyin.

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

Zaman serisi modeli hiyerarşiden hakkında daha fazla bilgiyi [başvuru belgeleri](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#hierarchies-api).

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

Zaman serisi örneklerdir. Çoğu durumda *DeviceID* veya *AssetID* ortamındaki varlık benzersiz tanımlayıcısı olacaktır. Örneğiniz kendileriyle ilişkili tanımlayıcı bilgiler örnek özelliklerini çağrılır. En az örnek özelliklerini hiyerarşisi bilgileri içerir. Bunlar, üretici, operatör ya da son hizmet tarihi gibi kullanışlı, açıklayıcı verileri de içerebilir.

Örnekleri tarafından tanımlanan *timeSeriesId*, *TypeID*, *HierarchyId*, ve *instanceFields*. Her örneği için tek bir eşler *türü*ve bir veya daha fazla hiyerarşi. Örnekleri hiyerarşiden, ek sırasında tüm özellikler devralır *instanceFields* daha fazla örnek özellik tanımı eklenebilir.

*instanceFields* örneği ve bir örneğini tanımlayan statik verilerin özellikleridir. Arama işlemleri gerçekleştirmek için dizin de desteklerken hiyerarşi veya hiyerarşi olmayan özellik değerlerini tanımlarlar.

## <a name="time-series-model-instance-json-example"></a>Zaman serisi modeli örnek JSON örneği

Örnek:

```JSON
{
    "typeId": "1be09af9-f089-4d6b-9f0b-48018b5f7393",
    "timeSeriesId": ["sampleTimeSeriesId"],
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

Zaman serisi modeli örneklerden hakkında daha fazla bilgiyi [başvuru belgeleri](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#instances-api).

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

Zaman serisi modeli ayarları hakkında daha fazla bilgiyi [başvuru belgeleri](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#model-settings-api).

## <a name="next-steps"></a>Sonraki adımlar

Okuma [Azure TSI (Önizleme) depolama ve giriş](./time-series-insights-update-storage-ingress.md).

Okuma hakkında yeni [zaman serisi modeli](https://docs.microsoft.com/rest/api/time-series-insights/preview-model).

<!-- Images -->
[1]: media/v2-update-tsm/tsm.png
[2]: media/v2-update-tsm/table.png
