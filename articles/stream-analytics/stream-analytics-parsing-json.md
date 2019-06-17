---
title: JSON ayrıştırma ve Azure Stream analytics'te AVRO
description: Bu makalede, diziler, JSON, CSV biçimlendirilmiş veri gibi karmaşık veri türleri üzerinde çalışılacak açıklar.
services: stream-analytics
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: ad30d363c8e3ea0264ba79db5417e572614a6739
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66496827"
---
# <a name="parse-json-and-avro-data-in-azure-stream-analytics"></a>Azure Stream analytics'te JSON ve Avro verileri ayrıştırılamadı

Azure Stream Analytics, Avro CSV ve JSON veri biçimleri olayları işlemeyi destekler. Hem JSON hem de Avro verileri (kayıtlar) iç içe nesneler ve diziler gibi karmaşık türler içerebilir.

## <a name="array-data-types"></a>Dizi veri türleri

Sıralı bir koleksiyonu değerler dizisi veri türleridir. Dizi değerlerini üzerindeki tipik bazı işlemler aşağıda ayrıntılı şekilde verilmiştir. Bu örneklerde, giriş olayları, "arrayField" adında bir özelliğe sahip varsayılmaktadır. diğer bir deyişle bir dizi veri türü.

Bu örneklerde işlevleri [GetArrayElement](https://msdn.microsoft.com/azure/stream-analytics/reference/getarrayelement-azure-stream-analytics), [GetArrayElements](https://msdn.microsoft.com/azure/stream-analytics/reference/getarrayelements-azure-stream-analytics), [GetArrayLength](https://msdn.microsoft.com/azure/stream-analytics/reference/getarraylength-azure-stream-analytics)ve [UYGULA](https://msdn.microsoft.com/azure/stream-analytics/reference/apply-azure-stream-analytics) işleci.

## <a name="examples"></a>Örnekler
Dizi öğesinin belirtilen dizinindeki (ilk dizi öğesinin seçerek) seçin:

```SQL
SELECT
    GetArrayElement(arrayField, 0) AS firstElement
FROM input
```

Dizi uzunluğu seçin:

```SQL
SELECT
    GetArrayLength(arrayField) AS arrayLength
FROM input
```

Tüm dizi öğesi tek tek olayları seçin. [UYGULA](https://msdn.microsoft.com/azure/stream-analytics/reference/apply-azure-stream-analytics) işleci ile birlikte [GetArrayElements](https://msdn.microsoft.com/azure/stream-analytics/reference/getarrayelements-azure-stream-analytics) yerleşik işlev tüm dizi öğeleri tek tek olayları olarak ayıklar:

```SQL
SELECT
    arrayElement.ArrayIndex,
    arrayElement.ArrayValue
FROM input as event
CROSS APPLY GetArrayElements(event.arrayField) AS arrayElement
```

## <a name="record-data-types"></a>Kayıt veri türleri
Kayıt veri türleri karşılık gelen biçimleri giriş veri akışlarını kullanıldığında, JSON ve Avro diziler temsil etmek için kullanılır. Bu örnekler, giriş olaylarında JSON biçimine okunurken bir örnek sensör gösterir. Tek bir olay'nın örneği aşağıda verilmiştir:

```json
{
    "DeviceId" : "12345",
    "Location" :
    {
        "Lat": 47,
        "Long": 122
    },
    "SensorReadings" :
    {
        "Temperature" : 80,
        "Humidity" : 70,
        "CustomSensor01" : 5,
        "CustomSensor02" : 99
    }
}
```

## <a name="examples"></a>Örnekler
İç içe geçmiş alanlarına erişmek için nokta gösterimi (.) kullanın. Örneğin, bu sorgu, yukarıdaki JSON verilerinde Location özelliği altında enlem ve boylam koordinatları seçer:

```SQL
SELECT
    DeviceID,
    Location.Lat,
    Location.Long
FROM input
```

Kullanım [GetRecordPropertyValue](https://msdn.microsoft.com/azure/stream-analytics/reference/getrecordpropertyvalue-azure-stream-analytics) işlevinin özellik adı bilinmiyor. Örneğin, bir örnek veri akışı her cihaz algılayıcı için başvuru veri içeren eşiklerini katılabileceği gerekiyor düşünün:

```json
{
    "DeviceId" : "12345",
    "SensorName" : "Temperature",
    "Value" : 75
}
```

```SQL
SELECT
    input.DeviceID,
    thresholds.SensorName
FROM input
JOIN thresholds
ON
    input.DeviceId = thresholds.DeviceId
WHERE
    GetRecordPropertyValue(input.SensorReadings, thresholds.SensorName) > thresholds.Value
```

Kayıt alanları ayrı olayları dönüştürmek için [UYGULA](https://msdn.microsoft.com/azure/stream-analytics/reference/apply-azure-stream-analytics) işleci ile birlikte [GetRecordProperties](https://msdn.microsoft.com/azure/stream-analytics/reference/getrecordproperties-azure-stream-analytics) işlevi. Örneğin, bir örnek akış olayları tek tek sensör okumaları akışını dönüştürmek için bu sorgu kullanılabilir:

```SQL
SELECT
    event.DeviceID,
    sensorReading.PropertyName,
    sensorReading.PropertyValue
FROM input as event
CROSS APPLY GetRecordProperties(event.SensorReadings) AS sensorReading
```

Tüm özellikleri kullanarak bir iç içe geçmiş kaydı seçebilirsiniz ' *' joker karakter. Aşağıdaki örnek göz önünde bulundurun:

```SQL
SELECT input.SensorReadings.*
FROM input
```

Sonuç olur:

```json
{
    "Temperature" : 80,
    "Humidity" : 70,
    "CustomSensor01" : 5,
    "CustomSensor022" : 99
}
```

## <a name="see-also"></a>Ayrıca Bkz.
[Azure Stream analytics'te veri türleri](https://msdn.microsoft.com/azure/stream-analytics/reference/data-types-azure-stream-analytics)
