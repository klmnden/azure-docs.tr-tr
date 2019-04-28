---
title: Azure IOT Hub sorgu dili anlama | Microsoft Docs
description: Geliştirici Kılavuzu - açıklama SQL benzeri IOT Hub'ın sorgu dili, IOT hub'dan cihaz/modül ikizler ve işler hakkında bilgi almak için kullanılır.
author: rezasherafat
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: rezas
ms.openlocfilehash: e5387f1e44a55b0a30f8620b49d237ac1e1ec2b6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61442176"
---
# <a name="iot-hub-query-language-for-device-and-module-twins-jobs-and-message-routing"></a>Cihaz ve modül ikizleri, işler ve ileti yönlendirme için IOT Hub sorgu dili

IOT hub'ı sağlayan bilgi almak için bir güçlü SQL benzeri dil ilgili [cihaz ikizlerini](iot-hub-devguide-device-twins.md) ve [işleri](iot-hub-devguide-jobs.md), ve [ileti yönlendirme](iot-hub-devguide-messages-d2c.md). Bu makalede sunar:

* IOT Hub sorgu dili, önemli özelliklere giriş ve
* Dil ayrıntılı açıklaması. İleti yönlendirme için sorgu dili hakkında daha fazla bilgi için bkz: [sorguları içinde ileti yönlendirme](../iot-hub/iot-hub-devguide-routing-query-syntax.md).

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="device-and-module-twin-queries"></a>Cihaz ve modül ikizi sorguları

[Cihaz ikizlerini](iot-hub-devguide-device-twins.md) ve etiketler ve Özellikler modül ikizlerini rastgele JSON nesneleri içerebilir. IOT hub'ı tüm ikizi bilgilerini içeren tek bir JSON belgesi olarak sorgu cihaz ikizleri ve modül ikizlerini sağlar.

Örneğin, IOT hub cihaz ikizlerini aşağıdaki yapıya sahip olduğunuzu varsaymaktadır (modül ikizi olacak yalnızca bir ek Moduleıd ile benzer):

```json
{
    "deviceId": "myDeviceId",
    "etag": "AAAAAAAAAAc=",
    "status": "enabled",
    "statusUpdateTime": "0001-01-01T00:00:00",
    "connectionState": "Disconnected",
    "lastActivityTime": "0001-01-01T00:00:00",
    "cloudToDeviceMessageCount": 0,
    "authenticationType": "sas",
    "x509Thumbprint": {
        "primaryThumbprint": null,
        "secondaryThumbprint": null
    },
    "version": 2,
    "tags": {
        "location": {
            "region": "US",
            "plant": "Redmond43"
        }
    },
    "properties": {
        "desired": {
            "telemetryConfig": {
                "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",
                "sendFrequencyInSecs": 300
            },
            "$metadata": {
            ...
            },
            "$version": 4
        },
        "reported": {
            "connectivity": {
                "type": "cellular"
            },
            "telemetryConfig": {
                "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",
                "sendFrequencyInSecs": 300,
                "status": "Success"
            },
            "$metadata": {
            ...
            },
            "$version": 7
        }
    }
}
```

### <a name="device-twin-queries"></a>Cihaz çifti sorguları

IOT Hub cihaz ikizlerini adlı bir belge koleksiyonu kullanıma sunar **cihazları**. Örneğin, aşağıdaki sorgu tüm cihaz çiftleri kümesini alır:

```sql
SELECT * FROM devices
```

> [!NOTE]
> [Azure IOT SDK'ları](iot-hub-devguide-sdks.md) büyük sonuçlarının disk belleği destekler.

IOT Hub cihaz ikizlerini rastgele koşullarla filtreleme almanıza olanak tanır. Örneğin, nerede ikizlerini aygıt alma için **location.region** etiket ayarlandığında **ABD** aşağıdaki sorguyu kullanın:

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
```

Boole işleçleri ve aritmetik karşılaştırmalar de desteklenir. Örneğin, ABD'de bulunan ve yapılandırılmış cihaz ikizlerini küçüktür her dakika telemetri gönderecek şekilde almak için aşağıdaki sorguyu kullanın:

```sql
SELECT * FROM devices
  WHERE tags.location.region = 'US'
    AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60
```

Bir kolaylık olarak da dizi sabitleriyle kullanmak mümkündür **IN** ve **NBU** işleçleri (içinde değil). Örneğin, alınacak raporu WiFi ya da kablolu bağlantı cihaz ikizlerini aşağıdaki sorguyu kullanın:

```sql
SELECT * FROM devices
  WHERE properties.reported.connectivity IN ['wired', 'wifi']
```

Genellikle, belirli bir özellik içeren tüm cihaz çiftlerini tanımlamak gereklidir. IOT hub'ın desteklediği işlevi `is_defined()` bu amaç için. Örneğin, tanımlama alma cihaz ikizlerini için `connectivity` özelliğini aşağıdaki sorguyu kullanın:

```SQL
SELECT * FROM devices
  WHERE is_defined(properties.reported.connectivity)
```

Başvurmak [WHERE yan tümcesi](iot-hub-devguide-query-language.md#where-clause) filtreleme yetenekleri tam başvuru için bölümü.

Gruplandırma ve toplamalar de desteklenir. Örneğin, her telemetri yapılandırma durumu cihaz sayısını bulmak için aşağıdaki sorguyu kullanın:

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
  FROM devices
  GROUP BY properties.reported.telemetryConfig.status
```

Bu gruplandırma sorgu bir sonuç aşağıdaki örneğe benzer döndürür:

```json
[
    {
        "numberOfDevices": 3,
        "status": "Success"
    },
    {
        "numberOfDevices": 2,
        "status": "Pending"
    },
    {
        "numberOfDevices": 1,
        "status": "Error"
    }
]
```

Bu örnekte, üç CİHAZDAN başarılı yapılandırma bildirilen iki yine de yapılandırmayı uygulama ve bir hata bildirdi.

Yansıtma sorguları, geliştiricilerin dönüş yalnızca ilgilendikleri özellikleri sağlar. Örneğin, tüm son etkinlik zamanı almak için cihazları kullanın aşağıdaki sorguyu kesildi:

```sql
SELECT LastActivityTime FROM devices WHERE status = 'enabled'
```

### <a name="module-twin-queries"></a>Modül ikizi sorguları

Modül ikizlerini üzerinde sorgulama için cihaz ikizlerini üzerinde sorgulama benzer, ancak farklı koleksiyon/ad alanı, yani "cihazların" yerine kullanarak device.modules sorgulayabilirsiniz:

```sql
SELECT * FROM devices.modules
```

Biz devices.modules koleksiyonları ve cihazlar arasında birleşim izin vermez. Cihazlar arasında sorgu modül ikizlerini için isterseniz etiketlere göre yaparsınız. Bu sorgu, tarama durumundaki tüm cihazlardaki tüm modül ikizlerini döndürecektir:

```sql
SELECT * FROM devices.modules WHERE properties.reported.status = 'scanning'
```

Bu sorgu tüm modül ikizlerini tarama durumundaki, ancak yalnızca cihazları belirtilen alt kümesi üzerinde döndürür:

```sql
SELECT * FROM devices.modules
  WHERE properties.reported.status = 'scanning'
  AND deviceId IN ['device1', 'device2']
```

### <a name="c-example"></a>C# örneği

Tarafından sunulan sorgu işlevini [C# hizmeti SDK'sını](iot-hub-devguide-sdks.md) içinde **RegistryManager** sınıfı.

Basit bir sorgu örneği aşağıda verilmiştir:

```csharp
var query = registryManager.CreateQuery("SELECT * FROM devices", 100);
while (query.HasMoreResults)
{
    var page = await query.GetNextAsTwinAsync();
    foreach (var twin in page)
    {
        // do work on twin object
    }
}
```

**Sorgu** nesnesi örneği içeren bir sayfa boyutunu (en fazla 100). Birden çok sayfa çağırarak alındıktan sonra **GetNextAsTwinAsync** yöntemleri birden çok kez.

Birden çok sorgu nesnesi sunan **sonraki** değerleri, sorgu için gerekli seri durumundan çıkarma seçeneğine bağlı olarak. Örneğin, cihaz ikizi veya iş nesneleri veya izdüşümler kullanılırken düz JSON.

### <a name="nodejs-example"></a>Node.js örnek

Tarafından sunulan sorgu işlevini [Node.js için Azure IOT hizmeti SDK'sını](iot-hub-devguide-sdks.md) içinde **kayıt defteri** nesne.

Basit bir sorgu örneği aşağıda verilmiştir:

```javascript
var query = registry.createQuery('SELECT * FROM devices', 100);
var onResults = function(err, results) {
    if (err) {
        console.error('Failed to fetch the results: ' + err.message);
    } else {
        // Do something with the results
        results.forEach(function(twin) {
            console.log(twin.deviceId);
        });

        if (query.hasMoreResults) {
            query.nextAsTwin(onResults);
        }
    }
};
query.nextAsTwin(onResults);
```

**Sorgu** nesnesi örneği içeren bir sayfa boyutunu (en fazla 100). Birden çok sayfa çağırarak alındıktan sonra **nextAsTwin** birden çok kez yöntemi.

Birden çok sorgu nesnesi sunan **sonraki** değerleri, sorgu için gerekli seri durumundan çıkarma seçeneğine bağlı olarak. Örneğin, cihaz ikizi veya iş nesneleri veya izdüşümler kullanılırken düz JSON.

### <a name="limitations"></a>Sınırlamalar

> [!IMPORTANT]
> Sorgu sonuçları birkaç dakika gecikme en son değerleri göre'ındaki cihaz ikizlerini olabilir. Bireysel cihaz ikizlerini Kimliğine göre sorgulama, alma cihaz ikizi API'yi kullanın. Bu API, her zaman en son değerleri içeren ve daha yüksek sınırlar azaltma sahiptir.

Şu anda karşılaştırmalar yalnızca ilkel türler arasında (hiçbir nesne), örneğin desteklenmektedir `... WHERE properties.desired.config = properties.reported.config` yalnızca bu özellikleri basit değerler varsa desteklenir.

## <a name="get-started-with-jobs-queries"></a>İşleri sorguları kullanmaya başlama

[İşleri](iot-hub-devguide-jobs.md) cihazları kümelerinin işlemleri yürütmek için bir yol sağlar. Her cihaz ikizi bunu olduğu adlı bir koleksiyon bölümünde işlerinin bilgileri içeren **işleri**.

```json
{
    "deviceId": "myDeviceId",
    "etag": "AAAAAAAAAAc=",
    "tags": {
        ...
    },
    "properties": {
        ...
    },
    "jobs": [
        {
            "deviceId": "myDeviceId",
            "jobId": "myJobId",
            "jobType": "scheduleTwinUpdate",
            "status": "completed",
            "startTimeUtc": "2016-09-29T18:18:52.7418462",
            "endTimeUtc": "2016-09-29T18:20:52.7418462",
            "createdDateTimeUtc": "2016-09-29T18:18:56.7787107Z",
            "lastUpdatedDateTimeUtc": "2016-09-29T18:18:56.8894408Z",
            "outcome": {
                "deviceMethodResponse": null
            }
        },
        ...
    ]
}
```

Şu anda, bu koleksiyon olarak sorgulanabilen **devices.jobs** IOT Hub sorgu dili.

> [!IMPORTANT]
> İşler özelliği şu anda hiçbir cihaz ikizlerini sorgulanırken döndürülür. Diğer bir deyişle, 'cihazlardan' içeren sorgular. İşleri özelliği yalnızca doğrudan sorgu kullanarak erişilebilir `FROM devices.jobs`.
>
>

Örneğin, tek bir cihazı etkileyen tüm işleri (Geçmiş ve zamanlanmış) almak için aşağıdaki sorguyu kullanabilirsiniz:

```sql
SELECT * FROM devices.jobs
  WHERE devices.jobs.deviceId = 'myDeviceId'
```

Bu sorgu, cihaza özel durumu (ve muhtemelen doğrudan yöntem yanıt) döndürülen her işin nasıl sağladığını unutmayın.

Tüm nesne özellikleri üzerinde rastgele Boole koşullarıyla filtrelemek mümkündür **devices.jobs** koleksiyonu.

Örneğin, belirli bir cihaz için Eylül 2016'dan sonra oluşturulan tüm tamamlanan cihaz ikizi güncelleştirme işlerini almak için aşağıdaki sorguyu kullanın:

```sql
SELECT * FROM devices.jobs
  WHERE devices.jobs.deviceId = 'myDeviceId'
    AND devices.jobs.jobType = 'scheduleTwinUpdate'
    AND devices.jobs.status = 'completed'
    AND devices.jobs.createdTimeUtc > '2016-09-01'
```

Cihaz başına tek bir iş sonuçlarını da alabilir.

```sql
SELECT * FROM devices.jobs
  WHERE devices.jobs.jobId = 'myJobId'
```

### <a name="limitations"></a>Sınırlamalar

Şu anda üzerinde sorgular **devices.jobs** desteklemez:

* Tahminler, bu nedenle yalnızca `SELECT *` mümkündür.
* Proje Özellikleri (önceki bölüme bakın) yanı sıra cihaz ikizi başvuran koşulları.
* Toplamalar, sayısı, ortalama, grup tarafından gerçekleştiriliyor.

## <a name="basics-of-an-iot-hub-query"></a>Bir IOT Hub sorgu temelleri

Her IOT Hub sorgu seçin ve ile isteğe bağlı bir WHERE yan tümceleri ve GROUP BY yan tümcesi oluşur. Her sorgu, JSON belgelerini, örneğin cihaz çiftleri koleksiyonu üzerinde çalıştırılır. FROM yan tümcesi belge koleksiyonunun üzerinde çalışmalar gösterir (**cihazları** veya **devices.jobs**). Ardından, WHERE yan tümcesinde filtre uygulanır. Bu adımın sonuçları toplama ile gruplandırılır GROUP BY yan tümcesinde belirtildiği gibi. Her bir grup için bir satır oluşturulur SELECT yan tümcesinde belirtildiği gibi.

```sql
SELECT <select_list>
  FROM <from_specification>
  [WHERE <filter_condition>]
  [GROUP BY <group_specification>]
```

## <a name="from-clause"></a>FROM yan tümcesi

**< From_specification >'nden** yan tümcesi, yalnızca iki değer varsayabilirsiniz: **CİHAZLARDAN** sorgu cihaz ikizlerini için veya **devices.jobs gelen** sorgu iş cihaz başına ayrıntıları.


## <a name="where-clause"></a>WHERE yan tümcesi
**Burada < filter_condition >** yan tümcesi, isteğe bağlıdır. Bu JSON FROM koleksiyonda belge bir veya daha fazla koşul sonucu bir parçası olarak dahil edilecek karşılamalıdır belirtir. Herhangi bir JSON belgesi, "sonucu dahil edilmesi için true olarak" belirli koşullar değerlendirmelidir.

İzin verilen koşullar bölümünde açıklanan [ifadeleri ve koşulları](iot-hub-devguide-query-language.md#expressions-and-conditions).

## <a name="select-clause"></a>SELECT yan tümcesi

**Seçin < select_list >** zorunludur ve hangi değerleri sorgudan alınan belirtir. Bu JSON yeni JSON nesneleri oluşturmak için kullanılacak değerleri belirtir.
Filtrelenmiş (ve isteğe bağlı olarak gruplandırılmış) alt FROM koleksiyonunun her öğesi için yansıtma aşaması yeni bir JSON nesnesi oluşturur. Bu nesne, SELECT yan tümcesinde belirtilen değerlerle oluşturulur.

SELECT yan tümcesi dilbilgisi aşağıda verilmiştir:

```
SELECT [TOP <max number>] <projection list>

<projection_list> ::=
    '*'
    | <projection_element> AS alias [, <projection_element> AS alias]+

<projection_element> :==
    attribute_name
    | <projection_element> '.' attribute_name
    | <aggregate>

<aggregate> :==
    count()
    | avg(<projection_element>)
    | sum(<projection_element>)
    | min(<projection_element>)
    | max(<projection_element>)
```

**Attrıbute_name** JSON belgesini FROM koleksiyondaki herhangi bir özelliğini ifade eder. Cihaz ikizi sorgularını bölümü ile Başlarken SELECT yan tümceleri bazı örnekler bulunabilir.

Şu anda farklı seçim tümceleri **seçin*** yalnızca cihaz ikizlerini üzerinde toplama sorguları desteklenir.

## <a name="group-by-clause"></a>GROUP BY yan tümcesi
**< Group_specification > GROUP BY** yan tümcesi SELECT belirtilen projeksiyon önce ve WHERE yan tümcesinde belirtilen filtre sonra yürüten isteğe bağlı bir adımdır. Bu belgeler bir özniteliğin değerine göre gruplandırır. Bu gruplar, SELECT yan tümcesinde belirtilen toplanan değerler oluşturmak için kullanılır.

GROUP BY kullanarak bir sorgu örneğidir:

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

GROUP BY için biçimsel sözdizimi aşağıdaki gibidir:

```
GROUP BY <group_by_element>
<group_by_element> :==
    attribute_name
    | < group_by_element > '.' attribute_name
```

**Attrıbute_name** JSON belgesini FROM koleksiyondaki herhangi bir özelliğini ifade eder.

GROUP BY yan tümcesi şu anda yalnızca cihaz ikizlerini sorgulanırken desteklenir.

> [!IMPORTANT]
> Terim `group` sorgularda özel bir anahtar sözcük olarak kabul edilir. Kullandığınız durumda `group` örn, hataları önlemek için çift köşeli parantez ile çevreleyen, özellik adı olarak düşünün `SELECT * FROM devices WHERE tags.[[group]].name = 'some_value'`.
>
>

## <a name="expressions-and-conditions"></a>İfadeleri ve koşulları
Yüksek bir düzeyde bir *ifade*:

* (Örneğin, Boolean, sayı, dize, dizi veya nesne) bir JSON türde bir örnek olarak değerlendirilir.
* Yerleşik bir işleç ve işlevlerini kullanarak sabitleri ve cihaz JSON belgesini gelen verileri işleyerek tanımlanır.

*Koşullar* değerlendirmek için bir Boolean ifadeler. Mantıksal farklı bir sabit **true** olarak kabul edilir **false**. Bu kural içerir **null**, **tanımlanmamış**, herhangi bir nesne veya dizi örneği, herhangi bir dize ve Boole **false**.

İfadeler için sözdizimi aşağıdaki gibidir:

```
<expression> ::=
    <constant> |
    attribute_name |
    <function_call> |
    <expression> binary_operator <expression> |
    <create_array_expression> |
    '(' <expression> ')'

<function_call> ::=
    <function_name> '(' expression ')'

<constant> ::=
    <undefined_constant>
    | <null_constant>
    | <number_constant>
    | <string_constant>
    | <array_constant>

<undefined_constant> ::= undefined
<null_constant> ::= null
<number_constant> ::= decimal_literal | hexadecimal_literal
<string_constant> ::= string_literal
<array_constant> ::= '[' <constant> [, <constant>]+ ']'
```

Her simge ifadeleri söz diziminde temsil anlamak için aşağıdaki tabloya bakın:

| Sembol | Tanım |
| --- | --- |
| attrıbute_name | JSON belgesinde herhangi bir özelliği **FROM** koleksiyonu. |
| binary_operator | Listelenen herhangi bir ikili işleç [işleçleri](#operators) bölümü. |
| işlev_adı| Listelenen herhangi bir işlev [işlevleri](#functions) bölümü. |
| decimal_literal |Kayan noktalı ondalık gösterimde ifade. |
| hexadecimal_literal |' % S'dize '0 x bir onaltılık basamak dize tarafından izlenen' olarak ifade edilen bir sayı. |
| string_literal |Dize değişmez değerleri, sıfır veya daha fazla Unicode karakter dizisi veya kaçış dizileri tarafından temsil edilen Unicode dizelerdir. Dize sabit değerlerinin tek tırnak işareti ya da çift tırnak işaretleri içine alınır. İzin verilen çıkar: `\'`, `\"`, `\\`, `\uXXXX` 4 onaltılık basamak tarafından tanımlanan Unicode karakter. |

### <a name="operators"></a>İşleçler
Aşağıdaki işleçleri destekler:

| Aile | İşleçler |
| --- | --- |
| Aritmetik |+, -, *, /, % |
| Mantıksal |VE, VEYA DEĞİL |
| Karşılaştırma |=, !=, <, >, <=, >=, <> |

### <a name="functions"></a>İşlevler
İkizler ve işler desteklenen tek sorgulanırken işlevi şu şekildedir:

| İşlev | Açıklama |
| -------- | ----------- |
| IS_DEFINED(Property) | Özellik değeri atanıp atanmadığını gösteren bir Boole değeri döndürür (dahil olmak üzere `null`). |

Yollar koşullarında aşağıdaki matematik işlevleri desteklenir:

| İşlev | Açıklama |
| -------- | ----------- |
| Abs(x) | Belirtilen sayısal ifade (pozitif) mutlak değerini döndürür. |
| EXP(x) | Üstel belirtilen sayısal ifadenin değerini döndürür (e ^ x). |
| POWER(x,y) | Belirtilen ifadenin değerini belirtilen bir kuvvete döndürür (x ^ y).|
| SQUARE(x) | Belirtilen bir sayısal değer karesini döndürür. |
| CEILING(x) | Büyüktür veya eşittir, belirtilen sayısal ifadenin en küçük tamsayı değerini döndürür. |
| FLOOR(x) | Belirtilen sayısal ifade küçük veya eşit en büyük tamsayı döndürür. |
| SIGN(x) | (+ 1) pozitif, sıfır (0) veya eksi (-1) belirtilen sayısal ifade döndürür.|
| Sqrt(x) | Belirtilen sayısal değerinin kare kökünü döndürür. |

Yollar koşullarında aşağıdaki tür denetlemesi ve atama işlevleri desteklenir:

| İşlev | Açıklama |
| -------- | ----------- |
| AS_NUMBER | Giriş dizesinin bir sayıya dönüştürür. `noop` Giriş bir sayı ise; `Undefined` , dize bir sayıyı temsil etmiyor.|
| IS_ARRAY | Belirtilen ifade türü bir dizi olup olmadığını gösteren bir Boole değeri döndürür. |
| IS_BOOL | Belirtilen ifade türünü bir Boole değeri olup olmadığını gösteren bir Boole değeri döndürür. |
| IS_DEFINED | Özellik değeri atanıp atanmadığını gösteren bir Boole değeri döndürür. |
| IS_NULL | Belirtilen ifadenin türü null olup olmadığını gösteren bir Boole değeri döndürür. |
| IS_NUMBER | Belirtilen ifade türünü bir sayı olup olmadığını gösteren bir Boole değeri döndürür. |
| IS_OBJECT | Belirtilen ifade türünü bir JSON nesnesi olup olmadığını gösteren bir Boole değeri döndürür. |
| IS_PRIMITIVE | Belirtilen ifadenin türü basit bir tür olup olmadığını gösteren bir Boole değeri döndürür (dize, sayısal, Boole veya `null`). |
| IS_STRING | Belirtilen ifadenin türü dize olup olmadığını gösteren bir Boole değeri döndürür. |

Yollar koşullarda, aşağıdaki dize işlevleri desteklenir:

| İşlev | Açıklama |
| -------- | ----------- |
| CONCAT (x, y,...) | İki veya daha fazla dize değerlerini birleştirirken sonucu olan bir dize döndürür. |
| LENGTH(x) | Belirtilen dize ifadesinin karakter sayısını döndürür.|
| Lower(x) | Büyük harf karakter verileri küçük harfe dönüştürmenin sonra bir dize ifadesi döndürür. |
| Upper(x) | Küçük harf karakter verileri büyük harfe dönüştürmenin sonra bir dize ifadesi döndürür. |
| Alt dize (string, başlangıç [, uzunluğu]) | Belirtilen karakterin sıfır tabanlı konumunda başlayan bir dize ifadesi bölümünü döndürür ve belirtilen uzunlukta veya dizenin sonuna kadar devam eder. |
| INDEX_OF (string, parça) | İkinci dizenin başlangıç konumunu döndürür dize bulunamazsa, ilk belirtilen dize ifadesi veya -1 içindeki ifadenin dize.|
| STARTS_WITH (x, y) | Boole döndürüp döndüremeyeceğini belirten döndürür ilk dize ifade olup olmadığını ve ikinci başlatır. |
| ENDS_WITH (x, y) | Boole döndürüp döndüremeyeceğini belirten döndürür ilk dize ifade olup olmadığını ve ikinci sona erer. |
| CONTAINS(x,y) | Döndürür bir Boolean gösteren ikinci ilk dize ifade olup olmadığını içerir. |

## <a name="next-steps"></a>Sonraki adımlar

Sorguları kullanarak uygulamalarınızda çalıştırma hakkında bilgi edinmek [Azure IOT SDK'ları](iot-hub-devguide-sdks.md).
