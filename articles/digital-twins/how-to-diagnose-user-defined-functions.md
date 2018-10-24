---
title: Azure dijital İkizlerini, UDF'ler hata ayıklama | Microsoft Docs
description: Azure dijital İkizlerini, UDF'ler hata ayıklama hakkında kılavuz
author: stefanmsft
manager: deshner
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/22/2018
ms.author: stefanmsft
ms.openlocfilehash: 852b2d35ae605f5529d162d52655fd258ca07c5a
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49946105"
---
# <a name="how-to-debug-issues-with-user-defined-functions-in-azure-digital-twins"></a>Azure dijital İkizlerini içinde kullanıcı tanımlı işlevlerle sorunlarında hata ayıklama

Bu makalede, kullanıcı tanımlı işlevleri tanılamak nasıl özetlenir. Ardından, bu bunlarla çalışırken karşılaşılan en yaygın senaryolardan bazıları tanımlar.

## <a name="debug-issues"></a>Sorunlarında hata ayıklama

Azure dijital İkizlerini örneğinizin içinde gerçekleşen tüm sorunlarının nasıl tanılandığını bilerek sorunu, sorunu ve çözüm nedenini etkili bir şekilde tanımlamak için yardımcı olur.

### <a name="enable-log-analytics-for-your-instance"></a>Örneğiniz için log analytics etkinleştir

Günlükleri ve ölçümleri Azure dijital İkizlerini örneğinizin Azure İzleyici sunulur. Aşağıdaki belgeler, oluşturduğunuz varsayılır bir [Azure Log Analytics](../log-analytics/log-analytics-queries.md) çalışma alanını kullanarak [Azure portalı](../log-analytics/log-analytics-quick-create-workspace.md)temellidir [Azure CLI](../log-analytics/log-analytics-quick-create-workspace-cli.md), aracılığıyla veya [ PowerShell](../log-analytics/log-analytics-quick-create-workspace-posh.md).

> [!NOTE]
> 5 dakikalık bir gecikmeyle olayları gönderirken karşılaşabilirsiniz **Log Analytics** ilk kez.

Makaleyi okuyun ["Toplamak ve Azure kaynaklarınızdan gelen günlük verilerini kullanabilirsiniz"](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) Portal, Azure CLI veya PowerShell aracılığıyla Azure dijital İkizlerini Örneğiniz için tanılama ayarlarını etkinleştirme. Tüm günlük kategorileri, Ölçümler ve Azure Log Analytics çalışma alanınızı seçtiğinizden emin olun.

### <a name="trace-sensor-telemetry"></a>Telemetri algılayıcı izleme

Tanılama ayarları Azure dijital İkizlerini Örneğinizde etkin, tüm günlük kategorileri seçilir ve günlükleri Azure Log Analytics'e gönderildiğini olduğundan emin olun.

Algılayıcı telemetri iletileriyle ilgili günlüklerinin için eşleştirilecek gönderilen olay verileri üzerinde bir bağıntı kimliği belirtebilirsiniz. Bunu yapmak için ayarlanmış `x-ms-client-request-id` özelliğini bir GUID.

Telemetri gönderdikten sonra sorgulamak için günlükleri kullanarak Azure Log Analytics'i açın bağıntı kimliği:

```Kusto
AzureDiagnostics
| where CorrelationId = 'yourCorrelationIdentifier'
```

| Özel öznitelik adı | Değiştirin |
| --- | --- |
| *yourCorrelationIdentifier* | Olay verileri üzerinde belirtilen bağıntı kimliği |

Kullanıcı tanımlı işlevinizi oturum açarsanız, bu günlükleri kategori Azure Log Analytics örneğinizle görünür `UserDefinedFunction`. Bunları almak için Azure Log Analytics'te şu sorgu koşulunu girin:

```Kusto
AzureDiagnostics
| where Category = 'UserDefinedFunction'
```

Güçlü sorgu işlemleri hakkında daha fazla bilgi için bkz: [sorguları ile çalışmaya başlama](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries).

## <a name="identify-common-issues"></a>Sık karşılaşılan sorunları tanımlayın

Tanılama hem ortak sorunları belirlemenize, çözüme giderirken önemlidir. Bazı yaygın sorunları karşılaşılan geliştirme kullanıcı tanımlı işlevleri aşağıda özetlenmiştir.

### <a name="ensure-a-role-assignment-was-created"></a>Rol atamasının oluşturulduğu emin olun.

Yönetim API'si içinde oluşturulan bir rol ataması olmadan kullanıcı tanımlı işlev bildirimleri, meta verileri alınırken gönderme gibi eylemleri gerçekleştirmek için erişim sahip değil ve ayarı topolojisi içindeki değerleri hesaplanan.

Bir rol ataması, yönetim API'si aracılığıyla kullanıcı tanımlı işleviniz için mevcut olup olmadığını denetleyin:

```plaintext
GET https://yourManagementApiUrl/api/v1.0/roleassignments?path=/&traverse=Down&objectId=yourUserDefinedFunctionId
```

| Özel öznitelik adı | Değiştirin |
| --- | --- |
| *yourManagementApiUrl* | Yönetim API'niz için tam URL yolu  |
| *yourUserDefinedFunctionId* | Rol atamalarını almak için kullanıcı tanımlı işlev kimliği|

Hiçbir rol ataması alınır, bu makaleyi takip [kullanıcı tanımlı işleviniz için bir rol ataması oluşturma](./how-to-user-defined-functions.md).

### <a name="check-if-the-matcher-will-work-for-a-sensors-telemetry"></a>Eşleştiricisi için telemetri algılayıcı'nın çalışıp çalışmayacağını denetleyin

Azure dijital İkizlerini örneklerinizin yönetim API'sine aşağıdaki çağrısı ile verilen Eşleştiricisi için verilen algılayıcı geçerliyse belirlemek mümkün olacaktır.

```plaintext
GET https://yourManagementApiUrl/api/v1.0/matchers/yourMatcherIdentifier/evaluate/yourSensorIdentifier?enableLogging=true
```

| Özel öznitelik adı | Değiştirin |
| --- | --- |
| *yourManagementApiUrl* | Yönetim API'niz için tam URL yolu  |
| *yourMatcherIdentifier* | Değerlendirmek istediğiniz Eşleştiricisi kimliği |
| *yourSensorIdentifier* | Değerlendirmek istediğiniz algılayıcı kimliği |

Yanıt:

```JavaScript
{
    "success": true,
    "logs": [
        "$.dataType: \"Motion\" Equals \"Motion\" => True"
    ]
}
```

### <a name="check-what-a-sensor-will-trigger"></a>Ne algılayıcı tetikleyecek denetleyin

Azure dijital İkizlerini örneklerinizin yönetim API'sine aşağıdaki çağrısı ile verilen algılayıcının gelen telemetriyi tarafından tetiklenen kullanıcı tanımlı işlevleri tanımlayıcıların belirlemek mümkün olacaktır:

```plaintext
GET https://yourManagementApiUrl/api/v1.0/sensors/yourSensorIdentifier/matchers?includes=UserDefinedFunctions
```

| Özel öznitelik adı | Değiştirin |
| --- | --- |
| *yourManagementApiUrl* | Yönetim API'niz için tam URL yolu  |
| *yourSensorIdentifier* | Telemetri gönderdiği algılayıcı kimliği |

Yanıt:

```JavaScript
[
    {
        "id": "48a64768-797e-4832-86dd-de625f5f3fd9",
        "name": "MotionMatcher",
        "spaceId": "2117b3e1-b6ce-42c1-9b97-0158bef59eb7",
        "conditions": [
            {
                "id": "024a067a-414f-415b-8424-7df61392541e",
                "target": "Sensor",
                "path": "$.dataType",
                "value": "\"Motion\"",
                "comparison": "Equals"
            }
        ],
        "userDefinedFunctions": [
            {
                "id": "373d03c5-d567-4e24-a7dc-f993460120fc",
                "spaceId": "2117b3e1-b6ce-42c1-9b97-0158bef59eb7",
                "name": "Motion User-Defined Function",
                "disabled": false
            }
        ]
    }
]
```

### <a name="issue-with-receiving-notifications"></a>Bildirim alma ile sorun

Harekete geçirilen kullanıcı tanımlı işlevin içindeki değil alıcı bildirimleri yaptığınızda emin, kullanılan tanımlayıcı türü topolojisi nesne türü parametreniz eşleşir.

**Yanlış** örneği:

```JavaScript
var customNotification = {
    Message: 'Custom notification that will not work'
};

sendNotification(telemetry.SensorId, "Space", JSON.stringify(customNotification));
```

Belirtilen topoloji nesne türü 'Alanı' iken bir algılayıcı için kullanılan tanımlayıcı başvurduğundan bu senaryo ortaya çıkar.

**Doğru** örneği:

```JavaScript
var customNotification = {
    Message: 'Custom notification that will work'
};

sendNotification(telemetry.SensorId, "Sensor", JSON.stringify(customNotification));
```

Bu sorunu konusunda en kolay yolu kullanmaktır `Notify` meta veri nesnesi üzerinde yöntemi.

Örnek:

```JavaScript
function process(telemetry, executionContext) {
    var sensorMetadata = getSensorMetadata(telemetry.SensorId);

    var customNotification = {
        Message: 'Custom notification'
    };

    // Short-hand for above methods where object type is known from metadata.
    sensorMetadata.Notify(JSON.stringify(customNotification));
}
```

## <a name="common-diagnostic-exceptions"></a>Ortak tanılama özel durumları

Tanılama ayarlarını etkinleştirme, bu sık karşılaşılan özel durumlar karşılaşabilirsiniz:

1. **Azaltma**: kullanıcı tanımlı işlevinizi sınırları ana hatlarıyla belirtilen yürütme hızını aşıyorsa [hizmet sınırları](./concepts-service-limits.md) makalesi, bu kısıtlanır. Azaltma sınırları süresi dolana kadar başarılı bir şekilde yürütülen başka işlem kapsar.

1. **Veri bulunamadı**: var olmayan meta verilerine erişmek, kullanıcı tanımlı işlev çalışırsa, işlem başarısız olur.

1. **Yetkili**:, kullanıcı tanımlı işlev yoksa, bir rol ataması ayarlayın veya topolojisinden belirli meta verilerine erişmek için yeterli izinlere sahip değil, işlem başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar

Nasıl etkinleştireceğinizi öğrenin [izleme ve günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) Azure dijital İkizlerini içinde.
