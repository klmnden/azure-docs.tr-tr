---
title: Azure IOT Hub ileti yönlendirme sorgu | Microsoft Docs
description: Geliştirici Kılavuzu - Azure IOT Hub ileti yönlendirme sorgusu söz dizimi.
author: ash2017
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/13/2018
ms.author: asrastog
ms.openlocfilehash: 94d3599fe919cf648be7115be68002d2aa458ee3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60400652"
---
# <a name="iot-hub-message-routing-query-syntax"></a>IOT Hub ileti yönlendirme sorgusu söz dizimi

İleti yönlendirme, farklı veri türleri yani yönlendirmek kullanıcıların sağlar, cihazın telemetri iletilerini, cihaz yaşam döngüsü olaylarını ve cihaz ikizi olayları çeşitli uç noktalar ile değiştirin. Sizin için önemli verileri almak için yönlendirme önce bu verileri zengin sorguların de uygulayabilirsiniz. Bu makalede, IOT Hub ileti yönlendirme sorgu dili açıklar ve bazı ortak sorgu kalıpları sağlar.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

İleti yönlendirme sorgu ileti özellikleri ve ileti gövdesi hem de cihaz ikizi etiketleri ve cihaz ikizi özelliklerini sağlar. İleti gövdesi JSON değil ise, ileti yönlendirme ileti yine de yönlendirebilirsiniz, ancak ileti gövdesi sorguları uygulanamaz.  Sorgular, burada bir Boolean true, sorgu kolaylaştırır Boolean ifadeler, tüm gelen veri yolları başarılı Boole false sorgu başarısız olursa ve veri yönlendirilir açıklanmıştır. İfade null veya tanımsız olur. false kabul edilir ve tanılama günlüklerinde arıza durumunda bir hata oluşturulur. Sorgu söz dizimi değerlendirilir ve kaydedilmesini rota için doğru olması gerekir.  

## <a name="message-routing-query-based-on-message-properties"></a>İleti yönlendirme sorgu ileti özelliklerine bağlı 

IOT hub'ı tanımlayan bir [ortak biçimi](iot-hub-devguide-messages-construct.md) CİHAZDAN buluta tüm protokoller üzerinde birlikte çalışabilirlik için Mesajlaşma için. IOT Hub ileti iletinin aşağıdaki JSON gösterimine varsayar. Sistem özellikleri tüm kullanıcılar için eklenir ve iletisinin içeriği tanımlayın. Kullanıcılar, iletiyi seçerek uygulama özellikleri ekleyebilirsiniz. IOT Hub CİHAZDAN buluta ileti büyük/küçük harfe olmadığından benzersiz özellik adlarını kullanmanızı öneririz. Aynı ada sahip birden çok özellikleri vardır, örneğin, IOT hub'ı yalnızca özelliklerinden gönderir.  

```json
{ 
  "message": { 
    "systemProperties": { 
      "contentType": "application/json", 
      "contentEncoding": "UTF-8", 
      "iothub-message-source": "deviceMessages", 
      "iothub-enqueuedtime": "2017-05-08T18:55:31.8514657Z" 
    }, 
    "appProperties": { 
      "processingPath": "{cold | warm | hot}", 
      "verbose": "{true, false}", 
      "severity": 1-5, 
      "testDevice": "{true | false}" 
    }, 
    "body": "{\"Weather\":{\"Temperature\":50}}" 
  } 
} 
```

### <a name="system-properties"></a>Sistem özellikleri

Sistem özellikleri içeriği ve iletilerin kaynak tanımlamanıza yardımcı olacaktır. 

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| contentType | string | Kullanıcının, iletinin içerik türünü belirtir. Sorgu ileti gövdesinde izin vermek için bu değer, uygulama/JSON ayarlanmalıdır. |
| contentEncoding | string | Kullanıcı iletisi kodlama türünü belirtir. İzin verilen değerler şunlardır: UTF-8, UTF-16, UTF-32 contentType application/JSON değerine ayarlanırsa. |
| ıothub bağlantı cihaz kimliği | string | Bu değer, IOT Hub tarafından ayarlanır ve cihaz Kimliğini tanımlar. Sorgulamak için aşağıdaki komutu kullanın `$connectionDeviceId`. |
| iothub-enqueuedtime | string | Bu değer, IOT Hub tarafından ayarlanır ve gerçek enqueuing iletinin UTC saatini gösterir. Sorgulamak için aşağıdaki komutu kullanın `enqueuedTime`. |

Bölümünde anlatıldığı gibi [IOT Hub iletilerini](iot-hub-devguide-messages-construct.md), bir iletiye ek sistem özellikleri vardır. Ek olarak **contentType**, **contentEncoding**, ve **enqueuedTime**, **connectionDeviceId** ve  **connectionModuleId** da sorgulanabilir.

### <a name="application-properties"></a>Uygulama özellikleri

Uygulama özellikleri iletiye eklenen kullanıcı tanımlı dizelerdir. Bu alanlar isteğe bağlıdır.  

### <a name="query-expressions"></a>Sorgu ifadeleri

İleti sistemi özellikleri üzerinde bir sorgu ile önek gerekiyor `$` simgesi. Uygulama özelliklerini sorgular adlarına ile erişilir ve ön ekine sahip değil `$`simgesi. Bir uygulama özelliği adı ile başlıyorsa `$`, ardından IOT Hub için Sistem Özellikleri'nde arama yapar ve bulunamadığında ve ardından uygulama özellikleri görünür. Örneğin: 

Sistem özelliği contentEncoding üzerinde sorgulama 

```sql
$contentEncoding = 'UTF-8'
```

Uygulama özelliği processingPath üzerinde sorgulamak için:

```sql
processingPath = 'hot'
```

Bu sorguları birleştirmek için Boolean ifadeler ve İşlevler kullanabilirsiniz:

```sql
$contentEncoding = 'UTF-8' AND processingPath = 'hot'
```

Desteklenen işleçler tam bir listesi ve İşlevler listelenir [ifade ve koşullar](iot-hub-devguide-query-language.md#expressions-and-conditions)

## <a name="message-routing-query-based-on-message-body"></a>İleti yönlendirme sorgu ileti gövdesinde tabanlı 

İleti gövdesi üzerinde sorgulama yapmayı etkinleştirmek için ileti UTF-8, UTF-16 veya UTF-32 kodlanmış bir JSON biçiminde olmalıdır. `contentType` Ayarlanmalıdır `application/JSON` ve `contentEncoding` biri desteklenen UTF kodlamalarda sistem özelliği olarak. Bu özellik belirtilmezse, IOT Hub ileti üzerinde sorgu ifadesi değerlendirmez. 

Aşağıdaki örnek, doğru biçimlendirilmiş ve kodlanmış JSON gövdesi ile bir ileti oluşturma işlemi gösterilmektedir: 

```javascript
var messageBody = JSON.stringify(Object.assign({}, {
    "Weather": {
        "Temperature": 50,
        "Time": "2017-03-09T00:00:00.000Z",
        "PrevTemperatures": [
            20,
            30,
            40
        ],
        "IsEnabled": true,
        "Location": {
            "Street": "One Microsoft Way",
            "City": "Redmond",
            "State": "WA"
        },
        "HistoricalData": [
            {
                "Month": "Feb",
                "Temperature": 40
            },
            {
                "Month": "Jan",
                "Temperature": 30
            }
        ]
    }
}));

// Encode message body using UTF-8  
var messageBytes = Buffer.from(messageBody, "utf8");

var message = new Message(messageBytes);

// Set message body type and content encoding 
message.contentEncoding = "utf-8";
message.contentType = "application/json";

// Add other custom application properties   
message.properties.add("Status", "Active");

deviceClient.sendEvent(message, (err, res) => {
    if (err) console.log('error: ' + err.toString());
    if (res) console.log('status: ' + res.constructor.name);
});
```

### <a name="query-expressions"></a>Sorgu ifadeleri

İleti gövdesi bir sorgu ile önek gerekiyor `$body`. Sorgu ifadesi içinde bir gövde başvuru, gövde dizi başvuru ya da birden fazla gövdesi başvuru kullanabilirsiniz. İleti sistemi özellikleri ve ileti uygulama özellikleri başvurusu, sorgu ifadesi gövdesi başvuru ayrıca birleştirebilirsiniz. Örneğin, tüm geçerli sorgu ifadeleri şunlardır: 

```sql
$body.Weather.HistoricalData[0].Month = 'Feb' 
```

```sql
$body.Weather.Temperature = 50 AND $body.Weather.IsEnabled 
```

```sql
length($body.Weather.Location.State) = 2 
```

```sql
$body.Weather.Temperature = 50 AND processingPath = 'hot'
```

## <a name="message-routing-query-based-on-device-twin"></a>İleti yönlendirme sorgu üzerinde cihaz ikizini tabanlı 

İleti yönlendirme sayesinde sorgulamak [cihaz İkizi](iot-hub-devguide-device-twins.md) etiketleri ve JSON nesneleri olan özellikleri. Modül ikizi üzerinde sorgulama desteklenmediğini unutmayın. Cihaz İkizi-etiketler ve özellikler bir örnek aşağıda gösterilmiştir.

```JSON
{
    "tags": { 
        "deploymentLocation": { 
            "building": "43", 
            "floor": "1" 
        } 
    }, 
    "properties": { 
        "desired": { 
            "telemetryConfig": { 
                "sendFrequency": "5m" 
            }, 
            "$metadata" : {...}, 
            "$version": 1 
        }, 
        "reported": { 
            "telemetryConfig": { 
                "sendFrequency": "5m", 
                "status": "success" 
            },
            "batteryLevel": 55, 
            "$metadata" : {...}, 
            "$version": 4 
        } 
    } 
} 
```

### <a name="query-expressions"></a>Sorgu ifadeleri

İleti gövdesi bir sorgu ile önek gerekiyor `$twin`. Sorgu ifadesi, ayrıca bir gövde başvuru, ileti sistemi özellikleri ve ileti uygulama özellikleri başvurusu ikizi etiketi veya Özellik Başvurusu birleştirebilirsiniz. Sorgu büyük küçük harfe duyarlı değil olarak etiketleri ve özellikleri benzersiz adlar kullanmanızı öneririz. Aynı zamanda kullanmadığınızdan `twin`, `$twin`, `body`, veya `$body`, özellik adları. Örneğin, tüm geçerli sorgu ifadeleri şunlardır: 

```sql
$twin.properties.desired.telemetryConfig.sendFrequency = '5m'
```

```sql
$body.Weather.Temperature = 50 AND $twin.properties.desired.telemetryConfig.sendFrequency = '5m'
```

```sql
$twin.tags.deploymentLocation.floor = 1 
```

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [ileti yönlendirme](iot-hub-devguide-messages-d2c.md).
* Deneyin [ileti yönlendirme öğretici](tutorial-routing.md).