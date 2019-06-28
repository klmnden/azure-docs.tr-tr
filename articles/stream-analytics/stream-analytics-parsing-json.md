---
title: JSON ayrıştırma ve Azure Stream analytics'te AVRO
description: Bu makalede, diziler, JSON, CSV biçimlendirilmiş veri gibi karmaşık veri türleri üzerinde çalışılacak açıklar.
services: stream-analytics
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: daf5b97e4ac586f89e5964ee16ee73c86f59b01d
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67329353"
---
# <a name="parse-json-and-avro-data-in-azure-stream-analytics"></a>Azure Stream analytics'te JSON ve Avro verileri ayrıştırılamadı

Azure Stream Analytics, Avro CSV ve JSON veri biçimleri olayları işlemeyi destekler. Hem JSON hem de Avro verileri yapılandırılmış olması ve iç içe nesneler (kayıtlar) ve diziler gibi bazı karmaşık türler bulunur. 




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
        "CustomSensor02" : 99,
        "SensorMetadata" : 
        {
        "Manufacturer":"ABC",
        "Version":"1.2.45"
        }
    }
}
```


### <a name="access-nested-fields-in-known-schema"></a>Bilinen şema erişim iç içe geçmiş alanları
İç içe alanlar doğrudan sorgunuzdan kolayca erişmek için nokta gösterimi (.) kullanın. Örneğin, bu sorgu, yukarıdaki JSON verilerinde Location özelliği altında enlem ve boylam koordinatları seçer. Nokta gösterimi, aşağıda gösterildiği gibi birden çok düzeyi gitmek için kullanılabilir.

```SQL
SELECT
    DeviceID,
    Location.Lat,
    Location.Long,
    SensorReadings.SensorMetadata.Version
FROM input
```

### <a name="select-all-properties"></a>Tüm Özellikler'i seçin
Tüm özellikleri kullanarak bir iç içe geçmiş kaydı seçebilirsiniz ' *' joker karakter. Aşağıdaki örnek göz önünde bulundurun:

```SQL
SELECT input.Location.*
FROM input
```

Sonuç olur:

```json
{
    "Lat" : 47,
    "Long" : 122
}
```


### <a name="access-nested-fields-when-property-name-is-a-variable"></a>Özellik adı bir değişken olduğunda erişim iç içe geçmiş alanları
Kullanım [GetRecordPropertyValue](https://docs.microsoft.com/stream-analytics-query/getmetadatapropertyvalue) özellik adı bir değişken ise işlev. 

Örneğin, bir örnek veri akışı her cihaz algılayıcı için başvuru veri içeren eşiklerini katılabileceği gerekiyor düşünün. Tür başvuru verilerin bir parçacığı aşağıda gösterilmiştir.

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
FROM input      -- stream input
JOIN thresholds -- reference data input
ON
    input.DeviceId = thresholds.DeviceId
WHERE
    GetRecordPropertyValue(input.SensorReadings, thresholds.SensorName) > thresholds.Value
    -- the where statement selects the property value coming from the reference data
```

### <a name="convert-record-fields-into-separate-events"></a>Kayıt alanları ayrı olayları Dönüştür
Kayıt alanları ayrı olayları dönüştürmek için [UYGULA](https://docs.microsoft.com/stream-analytics-query/apply-azure-stream-analytics) işleci ile birlikte [GetRecordProperties](https://docs.microsoft.com/stream-analytics-query/getrecordproperties-azure-stream-analytics) işlevi. Örneğin, önceki örnekte SensorReading için birkaç kayıt varsa, bunları farklı olaylarını ayıklamak için aşağıdaki sorguyu kullanılabilir:

```SQL
SELECT
    event.DeviceID,
    sensorReading.PropertyName,
    sensorReading.PropertyValue
FROM input as event
CROSS APPLY GetRecordProperties(event.SensorReadings) AS sensorReading
```



## <a name="array-data-types"></a>Dizi veri türleri

Sıralı bir koleksiyonu değerler dizisi veri türleridir. Dizi değerlerini üzerindeki tipik bazı işlemler aşağıda ayrıntılı şekilde verilmiştir. Bu örneklerde, giriş olayları, "arrayField" adında bir özelliğe sahip varsayılmaktadır. diğer bir deyişle bir dizi veri türü.

Bu örneklerde işlevleri [GetArrayElement](https://docs.microsoft.com/stream-analytics-query/getarrayelement-azure-stream-analytics), [GetArrayElements](https://docs.microsoft.com/stream-analytics-query/getarrayelements-azure-stream-analytics), [GetArrayLength](https://docs.microsoft.com/stream-analytics-query/getarraylength-azure-stream-analytics)ve [UYGULA](https://docs.microsoft.com/stream-analytics-query/apply-azure-stream-analytics) işleci.

### <a name="working-with-a-specific-array-element"></a>Belirli bir dizi öğesi ile çalışma
Dizi öğesinin belirtilen dizinindeki (ilk dizi öğesinin seçerek) seçin:

```SQL
SELECT
    GetArrayElement(arrayField, 0) AS firstElement
FROM input
```

### <a name="select-array-length"></a>Dizi uzunluğu seçin

```SQL
SELECT
    GetArrayLength(arrayField) AS arrayLength
FROM input
```

### <a name="convert-array-elements-into-separate-events"></a>Dizi öğelerini ayrı olayları Dönüştür
Tüm dizi öğesi tek tek olayları seçin. [UYGULA](https://docs.microsoft.com/stream-analytics-query/apply-azure-stream-analytics) işleci ile birlikte [GetArrayElements](https://docs.microsoft.com/stream-analytics-query/getarrayelements-azure-stream-analytics) yerleşik işlev tüm dizi öğeleri tek tek olayları olarak ayıklar:

```SQL
SELECT
    arrayElement.ArrayIndex,
    arrayElement.ArrayValue
FROM input as event
CROSS APPLY GetArrayElements(event.arrayField) AS arrayElement
```


## <a name="see-also"></a>Ayrıca Bkz.
[Azure Stream analytics'te veri türleri](https://docs.microsoft.com/stream-analytics-query/data-types-azure-stream-analytics)
