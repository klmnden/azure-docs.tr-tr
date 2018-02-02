---
title: Azure IOT Hub sorgu dili anlama | Microsoft Docs
description: "Geliştirici Kılavuzu - IOT hub'dan cihaz çiftlerini ve işleri hakkında bilgi almak için kullanılan SQL benzeri IOT hub'ı sorgu dili açıklaması."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 851a9ed3-b69e-422e-8a5d-1d79f91ddf15
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/29/2018
ms.author: elioda
ms.openlocfilehash: 01951afa983e7a578281fda38bb4714df6b41891
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="iot-hub-query-language-for-device-twins-jobs-and-message-routing"></a>Cihaz çiftlerini, işler ve ileti yönlendirme için IOT hub'ı sorgulama dili

IOT hub'ı sağlayan bilgi almak için güçlü bir SQL benzeri dili ile ilgili [cihaz çiftlerini] [ lnk-twins] ve [işleri][lnk-jobs], ve [ileti yönlendirme][lnk-devguide-messaging-routes]. Bu makalede sunar:

* Önemli özellikleri giriş IOT hub'ı sorgu dili ve
* Dil ayrıntılı açıklaması.

## <a name="device-twin-queries"></a>Cihaz çifti sorguları
[Cihaz çiftlerini] [ lnk-twins] etiketleri ve özellikleri rastgele JSON nesnelerini içerebilir. IOT Hub, tüm cihaz çifti bilgilerini içeren tek bir JSON belgesi olarak sorgu cihaz çiftlerini sağlar.
Örneğin, IOT hub cihaz çiftlerini aşağıdaki yapısını olduğunu varsayın:

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

IOT Hub cihaz çiftlerini adlı bir belge koleksiyonu olarak kullanıma sunar **aygıtları**.
Bu nedenle aşağıdaki sorgu tüm cihaz çiftlerini kümesini alır:

```sql
SELECT * FROM devices
```

> [!NOTE]
> [Azure IOT SDK'ları] [ lnk-hub-sdks] büyük sonuçlarının disk belleği destekler.

IOT Hub cihaz çiftlerini rasgele koşullarla filtreleme almanıza olanak tanır. Örneğin, where çiftlerini cihaz almaya **location.region** etiketi ayarlanmış **ABD** aşağıdaki sorguyu kullanın:

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
```

Boole işleçleri ve aritmetik karşılaştırmaları de desteklenir. Örneğin, aygıt almak için aşağıdaki sorguyu değerinden dakikada telemetri göndermeyi ABD'de bulunan ve yapılandırılmış çiftlerini kullanın:

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
    AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60
```

Bir kolaylık da dizi sabitleri ile kullanmak mümkündür **IN** ve **NBU** (içinde değil) işleçler. Örneğin, almak için aşağıdaki sorguyu WiFi veya kablolu bağlantısı rapor cihaz çiftlerini kullanın:

```sql
SELECT * FROM devices
WHERE properties.reported.connectivity IN ['wired', 'wifi']
```

Genellikle, belirli bir özellik içeren tüm cihaz çiftlerini tanımlamak gereklidir. IOT hub'ı destekleyen işlevi `is_defined()` bu amaç için. Örneğin, tanımlamak alma cihaz çiftlerini için `connectivity` özelliğini aşağıdaki sorguyu kullanın:

```SQL
SELECT * FROM devices
WHERE is_defined(properties.reported.connectivity)
```

Başvurmak [WHERE yan tümcesi] [ lnk-query-where] filtreleme yetenekleri tam başvuru için bölüm.

Gruplandırma ve toplamalar da desteklenir. Örneğin, cihaz sayısı her telemetri bulmak için yapılandırma durumu kullanın aşağıdaki sorguyu:

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

Bu gruplandırma sorgu bir sonuç aşağıdaki örneğe benzer şekilde döndürür:

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

Bu örnekte, üç aygıt başarılı bir yapılandırma bildirilen, iki hala yapılandırmayı uygulama ve bir hata bildirdi.

Yansıtma sorguları yalnızca önem verdiğiniz özellikleri döndürülecek geliştiricilerin olanak sağlar. Örneğin, tüm son etkinlik zamanı almak için cihazları kullanın aşağıdaki sorguyu kesildi:

```sql
SELECT LastActivityTime FROM devices WHERE status = 'enabled'
```

### <a name="c-example"></a>C# örnek
Sorgu işlevi tarafından sunulan [C# hizmeti SDK'sını] [ lnk-hub-sdks] içinde **RegistryManager** sınıfı.
Burada, basit bir sorgu örneği verilmiştir:

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

**Sorgu** nesne örneği içeren bir sayfa boyutunu (en fazla 100). Birden çok sayfa çağırarak alınır sonra **GetNextAsTwinAsync** yöntemleri birden çok kez.

Birden çok sorgu nesneyi kullanıma sunan **sonraki** değerleri, sorgu için gerekli seri durumdan çıkarma seçeneği bağlı olarak. Örneğin, aygıt çifti ya da iş nesneleri veya tahminleri kullanırken düz JSON.

### <a name="nodejs-example"></a>Node.js örneği
Sorgu işlevi tarafından sunulan [Node.js için Azure IOT hizmeti SDK'sını] [ lnk-hub-sdks] içinde **kayıt defteri** nesnesi.
Burada, basit bir sorgu örneği verilmiştir:

```nodejs
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

**Sorgu** nesne örneği içeren bir sayfa boyutunu (en fazla 100). Birden çok sayfa çağırarak alınır sonra **nextAsTwin** yöntemi birden çok kez.

Birden çok sorgu nesneyi kullanıma sunan **sonraki** değerleri, sorgu için gerekli seri durumdan çıkarma seçeneği bağlı olarak. Örneğin, aygıt çifti ya da iş nesneleri veya tahminleri kullanırken düz JSON.

### <a name="limitations"></a>Sınırlamalar

> [!IMPORTANT]
> Sorgu sonuçları en son değerleri göre gecikme birkaç dakika içinde cihaz çiftlerini olabilir. Tek tek cihaz çiftlerini Kimliğine göre sorgulama alma cihaz çifti API kullanın. Bu API, her zaman en son değerleri içerir ve daha yüksek azaltma sınırları vardır.

Şu anda karşılaştırmaları yalnızca ilkel türler arasında (hiçbir nesne), örneğin desteklenir `... WHERE properties.desired.config = properties.reported.config` yalnızca bu özellikleri ilkel değerler varsa desteklenir.

## <a name="get-started-with-jobs-queries"></a>İşlerini sorguları ile çalışmaya başlama

[İşlerini] [ lnk-jobs] aygıtların kümeleri üzerinde işlemlerini yürütmek için bir yol sağlar. Her cihaz çifti bilgilerin onu olduğu adlı bir koleksiyon bölümünde işlerin içeren **işleri**.
Mantıksal olarak

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

Bu koleksiyon şu anda olarak sorgulanabilir **devices.jobs** IOT hub'ı sorgu dili.

> [!IMPORTANT]
> İşlerini özelliği şu anda hiçbir zaman cihaz çiftlerini sorgulanırken döndürülür. Diğer bir deyişle, 'aygıtlardan' içeren sorgular. İşlerini özelliği yalnızca doğrudan sorgu kullanarak erişilebilir `FROM devices.jobs`.
>
>

Örneğin, tek bir cihazı etkileyen tüm işleri (Geçmiş ve zamanlanmış) almak için aşağıdaki sorguyu kullanabilirsiniz:

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.deviceId = 'myDeviceId'
```

Bu sorgu, cihaza özel durumu (ve büyük olasılıkla doğrudan yöntemi yanıtı) döndürülen her bir iş nasıl sağladığını unutmayın.
Tüm nesne özelliklerinde rastgele Boolean koşullara ile filtrelemek mümkündür **devices.jobs** koleksiyonu.
Örneğin, Eylül 2016 sonra belirli bir aygıt için oluşturulmuş tüm tamamlanan cihaz çifti güncelleştirme işleri almak için aşağıdaki sorguyu kullanın:

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.deviceId = 'myDeviceId'
    AND devices.jobs.jobType = 'scheduleTwinUpdate'
    AND devices.jobs.status = 'completed'
    AND devices.jobs.createdTimeUtc > '2016-09-01'
```

Tek bir iş aygıt başına sonuçlarını da alabilir.

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.jobId = 'myJobId'
```

### <a name="limitations"></a>Sınırlamalar
Üzerinde şu anda sorgular **devices.jobs** desteklemez:

* Projeksiyonlar, bu nedenle yalnızca `SELECT *` mümkündür.
* Proje Özellikleri (önceki bölüme bakın) ek olarak cihaz çiftine başvurmak koşulları.
* Count, avg, grupla gibi toplamalar gerçekleştiriliyor.

## <a name="device-to-cloud-message-routes-query-expressions"></a>Cihaz bulut ileti yollarını sorgu ifadeleri

Kullanarak [cihaz bulut yolları][lnk-devguide-messaging-routes], IOT Hub'ın farklı uç noktalar için cihaz bulut iletilerini gönderilmesi için yapılandırabilirsiniz. Gönderme karşı tek bir ileti hesaplanan ifadeleri temel alır.

Rota [koşulu] [ lnk-query-expressions] twin ve iş sorguları koşullarında olarak aynı IOT hub'ı sorgu dilini kullanır. Rota koşullar ileti üstbilgilerini ve gövde üzerinde değerlendirilir. Yönlendirme sorgu ifadesi yalnızca ileti üstbilgilerini gerektirebilir yalnızca ileti gövdesinin veya her ikisi de. IOT Hub, iletileri yönlendirmek için üstbilgiler ve ileti gövdesi için belirli bir şema varsayar. Aşağıdaki bölümlerde, IOT Hub'ın düzgün bir şekilde yönlendirmek gerekli olan açıklanmaktadır.

### <a name="routing-on-message-headers"></a>İleti üstbilgilerinde yönlendirme

IOT Hub aşağıdaki JSON gösterimi ileti yönlendirme iletisi üstbilgilerinin varsayılır:

```json
{
    "$messageId": "",
    "$enqueuedTime": "",
    "$to": "",
    "$expiryTimeUtc": "",
    "$correlationId": "",
    "$userId": "",
    "$ack": "",
    "$connectionDeviceId": "",
    "$connectionDeviceGenerationId": "",
    "$connectionAuthMethod": "",
    "$content-type": "",
    "$content-encoding": "",

    "userProperty1": "",
    "userProperty2": ""
}
```

İleti sistemi özelliklerini öneki ile `'$'` simgesi.
Kullanıcı özellikleri her zaman kendi adıyla erişilir. Bir kullanıcı özellik adı bir sistem özelliği ile çakışan varsa (gibi `$to`), kullanıcı özelliği ile alınır `$to` ifade.
Köşeli ayraçlar kullanarak sistem özelliği her zaman erişebilirsiniz `{}`: ifade örneği için kullanabileceğiniz `{$to}` sistem özelliğine erişmek için `to`. Köşeli parantez içindeki özellik adları, her zaman karşılık gelen sistem özelliği alır.

Özellik adları büyük küçük harfe duyarlı olduğunu unutmayın.

> [!NOTE]
> Tüm ileti özellikleri dizelerdir. Bölümünde açıklandığı gibi sistem özelliklerini [Geliştirici Kılavuzu][lnk-devguide-messaging-format], şu anda sorgularında kullanılabilir değildir.
>

Örneğin, kullanırsanız, bir `messageType` özelliği, tüm telemetri bir uç nokta ve başka bir uç nokta için tüm uyarılar yönlendirmek isteyebilirsiniz. Telemetri yönlendirmek için aşağıdaki ifade yazabilirsiniz:

```sql
messageType = 'telemetry'
```

Ve uyarı iletileri yönlendirmek için aşağıdaki ifade:

```sql
messageType = 'alert'
```

Boole ifadeleri ve işlevleri de desteklenir. Bu özellik, örneğin önem düzeyi arasında ayrım sağlar:

```sql
messageType = 'alerts' AND as_number(severity) <= 2
```

Başvurmak [ifade ve koşullar] [ lnk-query-expressions] desteklenen işleçler ve işlevlerin tam listesi için bölümü.

### <a name="routing-on-message-bodies"></a>İleti gövdeleri yönlendirme

IOT Hub, ileti gövdesinde dayalı yalnızca yönlendirebilirsiniz ileti gövdesi doğru ise, içeriği biçimlendirilmiş JSON UTF-8, UTF-16 veya UTF-32 kodlanmış. İletiye içerik türü ayarlayın `application/json`. İçerik bir ileti üstbilgilerinde desteklenen UTF Kodlamalar kodlama ayarlayın. Üstbilgileri birini belirtilmezse, IOT hub'ı karşı ileti gövdesi içeren herhangi bir sorgu ifade değerlendirilecek denemez. İleti bir JSON ileti değilse veya ileti içerik türü ve içerik kodlamasını belirtmiyorsa, ileti yönlendirme iletisi başlıklarını temel ileti yönlendirmek için kullanmaya devam edebilirsiniz.

Kullanabileceğiniz `$body` ileti yönlendirmek için sorgu ifadesinde. Sorgu ifadesinde basit gövde başvurusu, gövde dizi başvuru ya da birden fazla gövde başvuru kullanabilirsiniz. Sorgu ifadesi ayrıca gövde başvuru iletisi başlığı başvurusu ile birleştirebilirsiniz. Örneğin, tüm geçerli sorgu ifadeleri şunlardır:

```sql
$body.message.Weather.Location.State = 'WA'
$body.Weather.HistoricalData[0].Month = 'Feb'
$body.Weather.Temperature = 50 AND $body.message.Weather.IsEnabled
length($body.Weather.Location.State) = 2
$body.Weather.Temperature = 50 AND Status = 'Active'
```

## <a name="basics-of-an-iot-hub-query"></a>IOT Hub sorgusuyla temelleri
Her IOT hub'ı sorgu seçin ve ile isteğe bağlı WHERE yan tümceleri ve GROUP BY yan tümcesi oluşur. Her sorgu JSON belgeleri, örneğin cihaz çiftlerini topluluğu üzerinde çalıştırılır. FROM yan tümcesi üzerinde yinelendiğinde için belge koleksiyonunu gösterir (**aygıtları** veya **devices.jobs**). Ardından, WHERE yan tümcesinde filtre uygulanır. Bu adım toplamalar gruplandırılır GROUP BY yan tümcesinde belirtilen. Her grup için bir satır oluşturulan SELECT yan tümcesi belirtilmiş.

```sql
SELECT <select_list>
FROM <from_specification>
[WHERE <filter_condition>]
[GROUP BY <group_specification>]
```

## <a name="from-clause"></a>FROM yan tümcesi
**< From_specification > gelen** yan tümcesi, yalnızca iki değer varsayabilirsiniz: **AYGITLARDAN** sorgu cihaz çiftlerini için veya **devices.jobs gelen** sorgu iş aygıt başına ayrıntıları.

## <a name="where-clause"></a>WHERE yan tümcesi
**Burada < filter_condition >** yan tümcesi isteğe bağlıdır. JSON FROM koleksiyonunda belgeleri bir veya daha fazla sonucunu bir parçası olarak dahil edilecek koşullarını gerekir belirtir. Herhangi bir JSON belge sonucu dahil edilecek "true" belirtilen koşulları değerlendirmeniz gerekir.

İzin verilen koşullar bölümünde açıklanan [ifadeleri ve koşullar][lnk-query-expressions].

## <a name="select-clause"></a>SELECT yan tümcesi
**Seçin < select_list >** zorunludur ve değerleri sorgudan alınan belirtir. Yeni JSON nesnelerini oluşturmak için kullanılacak JSON değerleri belirtir.
Her kaynak koleksiyonu filtrelenmiş (ve isteğe bağlı olarak gruplandırılmış) alt öğe için yansıtma aşaması yeni bir JSON nesnesi oluşturur. Bu nesne, SELECT yan tümcesinde belirtilen değerlerle oluşturulur.

SELECT yan tümcesi dilbilgisi aşağıdadır:

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

**Attribute_name** FROM koleksiyonundaki JSON belgesinin herhangi bir özelliğe başvuruyor. SELECT yan tümceleri bazı örnekler bulunabilir [cihaz çifti sorguları ile çalışmaya başlama] [ lnk-query-getstarted] bölümü.

Şu anda, seçim yan tümceleri farklı **seçin*** cihaz çiftlerini toplama sorgularında yalnızca desteklenir.

## <a name="group-by-clause"></a>GROUP BY yan tümcesi
**GROUP BY < group_specification >** yan tümcesi WHERE yan tümcesinde ve SELECT belirtilen projeksiyon önce belirtilen filtre sonra yürüten bir adımdır isteğe bağlıdır. Bir özniteliğin değerine bağlı olarak belgelerin gruplandırır. Bu gruplar, SELECT yan tümcesinde belirtilen toplanmış değerlerini oluşturmak için kullanılır.

GROUP BY kullanarak bir sorgu örneğidir:

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

GROUP BY resmi sözdizimi şöyledir:

```
GROUP BY <group_by_element>
<group_by_element> :==
    attribute_name
    | < group_by_element > '.' attribute_name
```

**Attribute_name** FROM koleksiyonundaki JSON belgesinin herhangi bir özelliğe başvuruyor.

Şu anda, GROUP BY yan tümcesi, yalnızca cihaz çiftlerini sorgulanırken desteklenir.

## <a name="expressions-and-conditions"></a>İfadeler ve koşullar
Yüksek bir düzeyde bir *ifade*:

* (Örneğin, mantıksal değer, sayı, dize, dizi veya nesne) JSON türünün bir örneği için değerlendirir.
* Cihaz JSON belgesi ve yerleşik işleçler ve işlevleri kullanarak sabitleri gelen veri düzenleme tarafından tanımlanır.

*Koşullar* bir Boole değeri değerlendirmek ifadeler. Boole değeri'den farklı herhangi sabiti **true** olarak kabul **false**. Bu kural içerir **null**, **tanımsız**, herhangi bir nesne veya dizi örneği, herhangi bir dize ve Boolean **false**.

İfadeler sözdizimi aşağıdaki gibidir:

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

Her simge ifadeleri sözdiziminde ifade anlamak için aşağıdaki tabloya bakın:

| Simgesi | Tanım |
| --- | --- |
| attribute_name | JSON belgesinde herhangi bir özelliği **FROM** koleksiyonu. |
| binary_operator | Listelenen herhangi bir ikili işleç [işleçleri](#operators) bölümü. |
| function_name| Listelenen herhangi bir işlev [işlevleri](#functions) bölümü. |
| decimal_literal |Bir kayan noktalı ondalık gösterimde. |
| hexadecimal_literal |'0 x onaltılık basamak dizesiyle ve ardından' dize olarak ifade edilen bir sayı. |
| string_literal |Dize değişmez değerleri, sıfır veya daha fazla Unicode karakter dizisi veya kaçış sıraları tarafından temsil edilen Unicode dizelerdir. Dize değişmez değerleri, tek tırnak veya çift tırnak içine alınır. Çıkışları izin: `\'`, `\"`, `\\`, `\uXXXX` 4 onaltılık basamak tarafından tanımlanan Unicode karakterler. |

### <a name="operators"></a>İşleçler
Aşağıdaki işleçleri desteklenir:

| Aile | İşleçler |
| --- | --- |
| Aritmetik |+, -, *, /, % |
| Mantıksal |AND, OR, NOT |
| Karşılaştırma |=, !=, <, >, <=, >=, <> |

### <a name="functions"></a>İşlevler
Çiftlerini ve desteklenen tek işleri sorgulanırken işlevi şu şekildedir:

| İşlev | Açıklama |
| -------- | ----------- |
| IS_DEFINED(Property) | Özellik değeri atanmış olan gösteren bir Boole değeri döndürür (de dahil olmak üzere `null`). |

Yollar koşullarında aşağıdaki matematik işlevleri desteklenir:

| İşlev | Açıklama |
| -------- | ----------- |
| ABS(x) | Belirtilen sayısal ifade (pozitif) mutlak değerini döndürür. |
| EXP(x) | Belirtilen sayısal ifade üstel değeri döndürür (e ^ x). |
| POWER(x,y) | Belirtilen güç belirtilen ifadenin değerini döndürür (x ^ y).|
| SQUARE(x) | Kare belirtilen sayısal değeri döndürür. |
| CEILING(x) | Büyüktür veya eşittir, belirtilen sayısal ifadenin en küçük tamsayı değeri döndürür. |
| FLOOR(x) | Belirtilen sayısal ifade küçük veya eşit en büyük tamsayıyı döndürür. |
| SIGN(x) | Artı (+ 1), sıfır (0) veya belirtilen sayısal ifadenin eksi (-1) işareti döndürür.|
| SQRT(x) | Belirtilen sayısal değer kare kökünü döndürür. |

Yollar koşullarda, aşağıdaki tür denetleme ve atama işlevleri desteklenir:

| İşlev | Açıklama |
| -------- | ----------- |
| AS_NUMBER | Giriş dizesini sayıya dönüştürür. `noop`Giriş bir sayı ise; `Undefined` dize bir sayıyı temsil etmiyor durumunda.|
| IS_ARRAY | Belirtilen ifade türü bir dizi olup olmadığını gösteren bir Boole değeri döndürür. |
| IS_BOOL | Belirtilen ifade türü bir Boole değeri olup olmadığını gösteren bir Boole değeri döndürür. |
| IS_DEFINED | Özellik değeri atanmış olan gösteren bir Boole değeri döndürür. |
| IS_NULL | Belirtilen ifade türü null olup olmadığını gösteren bir Boole değeri döndürür. |
| IS_NUMBER | Belirtilen ifade türü bir sayı olup olmadığını gösteren bir Boole değeri döndürür. |
| IS_OBJECT | Belirtilen ifade türü bir JSON nesnesi olup olmadığını gösteren bir Boole değeri döndürür. |
| IS_PRIMITIVE | Belirtilen ifade türü bir basit tür olup olmadığını gösteren bir Boole değeri döndürür (dize, Boolean, sayısal ve veya `null`). |
| IS_STRING | Belirtilen ifade türü bir dize olup olmadığını gösteren bir Boole değeri döndürür. |

Yollar koşullarında aşağıdaki dize işlevleri desteklenir:

| İşlev | Açıklama |
| -------- | ----------- |
| CONCAT (x, y,...) | İki veya daha fazla dize değerlerini birleştirme sonucu olan bir dize döndürür. |
| LENGTH(x) | Belirtilen dize ifadesinin karakterlerin sayısını döndürür.|
| LOWER(x) | Büyük harf karakter verileri küçük harfe dönüştürmek sonra bir dize ifadesi döndürür. |
| UPPER(x) | Küçük harf karakter verileri büyük harfe dönüştürme sonra bir dize ifadesi döndürür. |
| SUBSTRING (dize, başlangıç [, uzunluk]) | Belirtilen karakter sıfır tabanlı konumdan başlayarak bir dize ifadesi bölümünü döndürür ve belirtilen uzunlukta veya dize sonu devam eder. |
| INDEX_OF (dize, parça) | İkinci ilk örneğinin başlangıç konumunu döndürür dizesi ifade ilk belirtilen dize ifadesi veya -1 içinde dizesi bulunamadı.|
| STARTS_WITH (x, y) | Döndüren bir Boolean belirten ilk ifade dize olup olmadığını ve ikinci başlatır. |
| ENDS_WITH (x, y) | Döndürür Boolean belirten bir ilk ifade dize olup olmadığını ve ikinci sona erer. |
| CONTAINS(x,y) | Döndüren bir Boolean belirten ikinci ilk ifade dize olup olmadığını içerir. |

## <a name="next-steps"></a>Sonraki adımlar
Sorguları kullanarak uygulamalarınızı yürütün öğrenin [Azure IOT SDK'ları][lnk-hub-sdks].

[lnk-query-where]: iot-hub-devguide-query-language.md#where-clause
[lnk-query-expressions]: iot-hub-devguide-query-language.md#expressions-and-conditions
[lnk-query-getstarted]: iot-hub-devguide-query-language.md#get-started-with-device-twin-queries

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-devguide-messaging-routes]: iot-hub-devguide-messages-read-custom.md
[lnk-devguide-messaging-format]: iot-hub-devguide-messages-construct.md
[lnk-devguide-messaging-routes]: ./iot-hub-devguide-messages-read-custom.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
