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
ms.date: 05/25/17
ms.author: elioda
ms.openlocfilehash: a7650104eda58923558892f6f0f6666d16dbce28
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="reference---iot-hub-query-language-for-device-twins-jobs-and-message-routing"></a>Başvuru - cihaz çiftlerini, işler ve ileti yönlendirme için IOT hub'ı sorgulama dili

IOT hub'ı sağlayan bilgi almak için güçlü bir SQL benzeri dili ile ilgili [cihaz çiftlerini] [ lnk-twins] ve [işleri][lnk-jobs], ve [ileti yönlendirme][lnk-devguide-messaging-routes]. Bu makalede sunar:

* Önemli özellikleri giriş IOT hub'ı sorgu dili ve
* Dil ayrıntılı açıklaması.

## <a name="get-started-with-device-twin-queries"></a>Cihaz çifti sorguları ile çalışmaya başlama
[Cihaz çiftlerini] [ lnk-twins] etiketleri ve özellikleri rastgele JSON nesnelerini içerebilir. IOT Hub, tüm cihaz çifti bilgilerini içeren tek bir JSON belgesi olarak sorgu cihaz çiftlerini sağlar.
Örneğin, IOT hub cihaz çiftlerini aşağıdaki yapısını olduğunu varsayın:

```json
{
    "deviceId": "myDeviceId",
    "etag": "AAAAAAAAAAc=",
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

IOT Hub cihaz çiftlerini rasgele koşullarla filtreleme almanıza olanak tanır. Örneğin,

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
```

ile cihaz çiftlerini alır **location.region** etiketi kümesine **ABD**.
Boole işleçleri ve aritmetik karşılaştırmaları de, örneğin desteklenir

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
    AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60
```

telemetri dakikada daha az sıklıkta göndermek için yapılandırılan ABD bulunan tüm cihaz çiftlerini alır. Bir kolaylık da dizi sabitleri ile kullanmak mümkündür **IN** ve **NBU** (içinde değil) işleçler. Örneğin,

```sql
SELECT * FROM devices
WHERE properties.reported.connectivity IN ['wired', 'wifi']
```

WiFi bildirilen veya bağlantı kablolu tüm cihaz çiftlerini alır. Genellikle, belirli bir özellik içeren tüm cihaz çiftlerini tanımlamak gereklidir. IOT hub'ı destekleyen işlevi `is_defined()` bu amaç için. Örneğin,

```SQL
SELECT * FROM devices
WHERE is_defined(properties.reported.connectivity)
```

tanımladığınız tüm cihaz çiftlerini alınan `connectivity` özelliği bildirdi. Başvurmak [WHERE yan tümcesi] [ lnk-query-where] filtreleme yetenekleri tam başvuru için bölüm.

Gruplandırma ve toplamalar da desteklenir. Örneğin,

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

cihaz sayısı her telemetri yapılandırma durumunu döndürür.

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

Önceki örnekte olduğu üç aygıt başarılı bir yapılandırma bildirilen, iki hala yapılandırmayı uygulama ve bir hata bildirdi bir durum gösterilir.

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

Not nasıl **sorgu** nesne örneği içeren bir sayfa boyutunu (en fazla 1000) ve sonra birden çok sayfa çağırarak alınabilir **GetNextAsTwinAsync** yöntemleri birden çok kez.
Sorgu nesnesi birden çok sunan Not **sonraki\***bağlı olarak cihaz çiftine veya iş nesneleri veya düz JSON gibi sorgu tarafından tahminleri kullanırken kullanılması gereken seri durumdan çıkarma seçeneği.

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

Not nasıl **sorgu** nesne örneği içeren bir sayfa boyutunu (en fazla 1000) ve sonra birden çok sayfa çağırarak alınabilir **nextAsTwin** yöntemleri birden çok kez.
Sorgu nesnesi birden çok sunan Not **sonraki\***bağlı olarak cihaz çiftine veya iş nesneleri veya düz JSON gibi sorgu tarafından tahminleri kullanırken kullanılması gereken seri durumdan çıkarma seçeneği.

### <a name="limitations"></a>Sınırlamalar
> [!IMPORTANT]
> Sorgu sonuçları en son değerleri göre gecikme birkaç dakika içinde cihaz çiftlerini olabilir. Tek tek cihaz çiftlerini kimliğine göre sorgulama, her zaman, her zaman en son değerleri içerir ve daha yüksek azaltma sınırları almak cihaz çifti API kullanılması tercih edilir.

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
> İşlerini özelliği şu anda hiçbir zaman cihaz çiftlerini (diğer bir deyişle, 'aygıtlardan' içeren sorgular) sorgulanırken döndürülür. Yalnızca doğrudan sorgu kullanarak erişilebilir `FROM devices.jobs`.
>
>

Örneğin, tek bir cihazı etkileyen tüm işleri (Geçmiş ve zamanlanmış) almak için aşağıdaki sorguyu kullanabilirsiniz:

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.deviceId = 'myDeviceId'
```

Bu sorgu, cihaza özel durumu (ve büyük olasılıkla doğrudan yöntemi yanıtı) döndürülen her bir iş nasıl sağladığını unutmayın.
Tüm nesne özelliklerinde rastgele Boolean koşullara ile filtrelemek mümkündür **devices.jobs** koleksiyonu.
Örneğin, aşağıdaki sorguyu:

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.deviceId = 'myDeviceId'
    AND devices.jobs.jobType = 'scheduleTwinUpdate'
    AND devices.jobs.status = 'completed'
    AND devices.jobs.createdTimeUtc > '2016-09-01'
```

Tüm tamamlanan alır cihaz çifti güncelleştirme işleri aygıtın **myDeviceId** Eylül 2016 sonra oluşturulmuş.

Tek bir işin cihaz başına sonuçlar almak mümkündür.

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.jobId = 'myJobId'
```

### <a name="limitations"></a>Sınırlamalar
Üzerinde şu anda sorgular **devices.jobs** desteklemez:

* Projeksiyonlar, bu nedenle yalnızca `SELECT *` mümkündür.
* Proje Özellikleri (önceki bölüme bakın) ek olarak cihaz çiftine başvurmak koşulları.
* Count, avg, grupla gibi toplamalar gerçekleştiriliyor.

## <a name="get-started-with-device-to-cloud-message-routes-query-expressions"></a>Cihaz bulut ileti yollar sorgu ifadeleri ile çalışmaya başlama

Kullanarak [cihaz bulut yolları][lnk-devguide-messaging-routes], IOT Hub'ın tek bir ileti karşı değerlendirilen ifadeleri göre farklı uç noktalar için cihaz bulut iletilerini gönderilmesi için yapılandırabilirsiniz.

Rota [koşulu] [ lnk-query-expressions] twin ve iş sorguları koşullarında olarak aynı IOT hub'ı sorgu dilini kullanır. Rota koşullar ileti üstbilgilerini ve gövde üzerinde değerlendirilir. Yönlendirme sorgu ifadesi yalnızca ileti gövdesi, yalnızca ileti üstbilgilerini gerektirebilir veya her ikisi de üstbilgileri iletisi ve ileti gövdesi. IOT hub'ı iletileri yönlendirmek için üstbilgiler ve ileti gövdesi için belirli bir şema varsayar ve aşağıdaki bölümlerde IOT Hub'ın düzgün bir şekilde yönlendirmek gereklidir:

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
Kullanıcı özellikleri her zaman kendi adıyla erişilir. Bir kullanıcı özellik adı bir sistem özelliği ile çakıştığı için ortaya çıkarsa (gibi `$to`), kullanıcı özelliği ile alınan `$to` ifade.
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

IOT Hub, ileti gövdesinde dayalı yalnızca yönlendirebilirsiniz ileti gövdesi doğru ise, içeriği biçimlendirilmiş JSON UTF-8, UTF-16 veya UTF-32 kodlanmış. İletiye içerik türü ayarlamalısınız `application/json` ve IOT Hub'ı gövde içeriğine göre ileti yönlendirmek izin vermek için ileti üstbilgilerinde desteklenen UTF Kodlamalar birine içerik kodlaması. Üstbilgileri birini belirtilmezse, IOT hub'ı karşı ileti gövdesi içeren herhangi bir sorgu ifade değerlendirmek çalışmaz. İletinizi bir JSON ileti değilse veya ileti içerik türü ve içerik kodlamasını belirtmiyorsa hala ileti yönlendirme iletisi başlıklarını temel ileti yönlendirmek için kullanabilirsiniz.

Kullanabileceğiniz `$body` ileti yönlendirmek için sorgu ifadesinde. Sorgu ifadesinde basit gövde başvurusu, gövde dizi başvuru ya da birden fazla gövde başvuru kullanabilirsiniz. Sorgu ifadesi ayrıca gövde başvuru iletisi başlığı başvurusu ile birleştirebilirsiniz. Örneğin, tüm geçerli sorgu ifadeleri şunlardır:

```sql
$body.message.Weather.Location.State = 'WA'
$body.Weather.HistoricalData[0].Month = 'Feb'
$body.Weather.Temperature = 50 AND $body.message.Weather.IsEnabled
length($body.Weather.Location.State) = 2
$body.Weather.Temperature = 50 AND Status = 'Active'
```

## <a name="basics-of-an-iot-hub-query"></a>IOT Hub sorgusuyla temelleri
Her IOT hub'ı sorgu seçme ve FROM yan tümcesi ve isteğe bağlı WHERE ve GROUP BY yan tümceleri oluşur. Her sorgu JSON belgeleri, örneğin cihaz çiftlerini topluluğu üzerinde çalıştırılır. FROM yan tümcesi üzerinde yinelendiğinde için belge koleksiyonunu gösterir (**aygıtları** veya **devices.jobs**). Ardından, WHERE yan tümcesinde filtre uygulanır. Toplamalar ile bu adım olarak gruplandırılır GROUP BY yan tümcesinde ve her grup için belirtilen bir satır oluşturulan SELECT yan tümcesi belirtilmiş.

```sql
SELECT <select_list>
FROM <from_specification>
[WHERE <filter_condition>]
[GROUP BY <group_specification>]
```

## <a name="from-clause"></a>FROM yan tümcesi
**< From_specification > gelen** yan tümcesi, yalnızca iki değer varsayabilirsiniz: **AYGITLARDAN**, cihaz çiftlerini sorgulamak veya **devices.jobs gelen**, işin cihaz başına sorgulamak için ayrıntıları.

## <a name="where-clause"></a>WHERE yan tümcesi
**Burada < filter_condition >** yan tümcesi isteğe bağlıdır. JSON FROM koleksiyonunda belgeleri bir veya daha fazla sonucunu bir parçası olarak dahil edilecek koşullarını gerekir belirtir. Herhangi bir JSON belge sonucu dahil edilecek "true" belirtilen koşulları değerlendirmeniz gerekir.

İzin verilen koşullar bölümünde açıklanan [ifadeleri ve koşullar][lnk-query-expressions].

## <a name="select-clause"></a>SELECT yan tümcesi
SELECT yan tümcesi (**seçin < select_list >**) zorunludur ve değerleri sorgudan alınan belirtir. Yeni JSON nesnelerini oluşturmak için kullanılacak JSON değerleri belirtir.
Her kaynak koleksiyonu filtrelenmiş (ve isteğe bağlı olarak gruplandırılmış) alt öğe için yansıtma aşaması SELECT yan tümcesinde belirtilen değerlerle oluşturulan yeni bir JSON nesnesi oluşturur.

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

Burada **attribute_name** FROM koleksiyonundaki JSON belgesinin herhangi bir özelliğe başvuruyor. SELECT yan tümceleri bazı örnekler bulunabilir [cihaz çifti sorguları ile çalışmaya başlama] [ lnk-query-getstarted] bölümü.

Şu anda, seçim yan tümceleri farklı **seçin \***  cihaz çiftlerini toplama sorgularında yalnızca desteklenir.

## <a name="group-by-clause"></a>GROUP BY yan tümcesi
**GROUP BY < group_specification >** yan tümcesi WHERE yan tümcesinde ve SELECT belirtilen projeksiyon önce belirtilen filtre sonra yürütülen isteğe bağlı bir adım olan. Bir özniteliğin değerine bağlı olarak belgelerin gruplandırır. Bu gruplar, SELECT yan tümcesinde belirtilen toplanmış değerlerini oluşturmak için kullanılır.

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

Burada **attribute_name** FROM koleksiyonundaki JSON belgesinin herhangi bir özelliğe başvuruyor.

Şu anda, GROUP BY yan tümcesi, yalnızca cihaz çiftlerini sorgulanırken desteklenir.

## <a name="expressions-and-conditions"></a>İfadeler ve koşullar
Yüksek bir düzeyde bir *ifade*:

* (Örneğin, Boolean, sayı, dize, dizi veya nesne), JSON türünün bir örneği için değerlendirir ve
* Cihaz JSON belgesi ve yerleşik işleçler ve işlevleri kullanarak sabitleri gelen veri düzenleme tarafından tanımlanır.

*Koşullar* bir Boole değeri değerlendirmek ifadeler. Boole değeri'den farklı herhangi sabiti **true** olarak kabul **false** (dahil olmak üzere **null**, **tanımsız**, herhangi bir nesne veya dizi örneği, herhangi bir dize ve açıkça Boolean **false**).

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

Burada:

| Simgesi | Tanım |
| --- | --- |
| attribute_name | JSON belgesinde herhangi bir özelliği **FROM** koleksiyonu. |
| binary_operator | Listelenen herhangi bir ikili işleç [işleçleri](#operators) bölümü. |
| işlev_adı| Listelenen herhangi bir işlev [işlevleri](#functions) bölümü. |
| decimal_literal |Bir kayan noktalı ondalık gösterimde. |
| hexadecimal_literal |'0 x onaltılık basamak dizesiyle ve ardından' dize olarak ifade edilen bir sayı. |
| string_literal |Dize değişmez değerleri, sıfır veya daha fazla Unicode karakter dizisi veya kaçış sıraları tarafından temsil edilen Unicode dizelerdir. Dize değişmez değerleri tek tırnak içine alınmış (kesme işareti: ') veya çift tırnak (tırnak işareti: "). Çıkışları izin: `\'`, `\"`, `\\`, `\uXXXX` 4 onaltılık basamak tarafından tanımlanan Unicode karakterler. |

### <a name="operators"></a>İşleçler
Aşağıdaki işleçleri desteklenir:

| Ailesi | İşleçler |
| --- | --- |
| Aritmetik |+,-,*,/,% |
| Mantıksal |VE VEYA DEĞİL |
| Karşılaştırma |=, !=, <, >, <=, >=, <> |

### <a name="functions"></a>İşlevler
Çiftlerini ve desteklenen tek işleri sorgulanırken işlevi şu şekildedir:

| İşlevi | Açıklama |
| -------- | ----------- |
| IS_DEFINED(Property) | Özellik değeri atanmış olan gösteren bir Boole değeri döndürür (de dahil olmak üzere `null`). |

Yollar koşullarında aşağıdaki matematik işlevleri desteklenir:

| İşlevi | Açıklama |
| -------- | ----------- |
| Abs(x) | Belirtilen sayısal ifade (pozitif) mutlak değerini döndürür. |
| EXP(x) | Belirtilen sayısal ifade üstel değeri döndürür (e ^ x). |
| Power(x,y) | Belirtilen güç belirtilen ifadenin değerini döndürür (x ^ y).|
| SQUARE(x) | Kare belirtilen sayısal değeri döndürür. |
| CEILING(x) | Büyüktür veya eşittir, belirtilen sayısal ifadenin en küçük tamsayı değeri döndürür. |
| FLOOR(x) | Belirtilen sayısal ifade küçük veya eşit en büyük tamsayıyı döndürür. |
| SIGN(x) | Artı (+ 1), sıfır (0) veya belirtilen sayısal ifadenin eksi (-1) işareti döndürür.|
| Sqrt(x) | Kare belirtilen sayısal değeri döndürür. |

Yollar koşullarda, aşağıdaki tür denetleme ve atama işlevleri desteklenir:

| İşlevi | Açıklama |
| -------- | ----------- |
| AS_NUMBER | Giriş dizesini sayıya dönüştürür. `noop`Giriş bir sayı ise; `Undefined` dize bir sayıyı temsil etmiyor durumunda.|
| IS_ARRAY | Belirtilen ifade türü bir dizi olup olmadığını gösteren bir Boole değeri döndürür. |
| IS_BOOL | Belirtilen ifade türü bir Boole değeri olup olmadığını gösteren bir Boole değeri döndürür. |
| IS_DEFINED | Özellik değeri atanmış olan gösteren bir Boole değeri döndürür. |
| IS_NULL | Belirtilen ifade türü null olup olmadığını gösteren bir Boole değeri döndürür. |
| IS_NUMBER | Belirtilen ifade türü bir sayı olup olmadığını gösteren bir Boole değeri döndürür. |
| IS_OBJECT | Belirtilen ifade türü bir JSON nesnesi olup olmadığını gösteren bir Boole değeri döndürür. |
| IS_PRIMITIVE | Belirtilen ifade türü bir basit tür olup olmadığını gösteren bir Boole değeri döndürür (dize, Boolean, sayısal veya `null`). |
| IS_STRING | Belirtilen ifade türü bir dize olup olmadığını gösteren bir Boole değeri döndürür. |

Yollar koşullarında aşağıdaki dize işlevleri desteklenir:

| İşlevi | Açıklama |
| -------- | ----------- |
| CONCAT(x,...) | İki veya daha fazla dize değerlerini birleştirme sonucu olan bir dize döndürür. |
| LENGTH(x) | Belirtilen dize ifadesinin karakterlerin sayısını döndürür.|
| Lower(x) | Büyük harf karakter verileri küçük harfe dönüştürmek sonra bir dize ifadesi döndürür. |
| Upper(x) | Küçük harf karakter verileri büyük harfe dönüştürme sonra bir dize ifadesi döndürür. |
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
