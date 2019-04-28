---
title: Azure dijital İkizlerini kullanıcı tanımlı işlevler oluşturma | Microsoft Docs
description: Kullanıcı tanımlı işlevler, matchers ve rol atamaları Azure dijital İkizlerini oluşturma
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: alinast
ms.custom: seodec18
ms.openlocfilehash: 7208f96d99127247b51510e0c43c1733bb327dfb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60921899"
---
# <a name="how-to-create-user-defined-functions-in-azure-digital-twins"></a>Azure dijital İkizlerini kullanıcı tanımlı işlevler oluşturma

[Kullanıcı tanımlı işlevleri](./concepts-user-defined-functions.md) gelen telemetri iletilerini ve uzamsal graf meta verileri yürütülecek özel mantığı yapılandırma kullanıcıların etkinleştirin. Kullanıcılar ayrıca gönderebilir olayları için önceden tanımlanmış [uç noktaları](./how-to-egress-endpoints.md).

Bu kılavuzda algılamak nasıl yazılacağını gösteren bir örnek anlatılmaktadır ve tüm okuma uyar, belirli bir sıcaklık alınan sıcaklık olaylardan aşıyor.

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

## <a name="client-library-reference"></a>İstemci Kitaplığı Başvurusu

Kullanıcı tanımlı işlevler çalışma zamanı yardımcı yöntemler olarak kullanılabilir işlevler listelenir [istemci kitaplığı başvurusu](./reference-user-defined-functions-client-library.md) belge.

## <a name="create-a-matcher"></a>Bir Eşleştiricisi oluşturma

Matchers ne için belirli bir telemetri iletisi kullanıcı tanımlı işlevleri çalıştırma belirleyen graf nesneleridir.

- Geçerli Eşleştiricisi koşul karşılaştırmalar:

  - `Equals`
  - `NotEquals`
  - `Contains`

- Geçerli Eşleştiricisi koşul hedefleri:

  - `Sensor`
  - `SensorDevice`
  - `SensorSpace`

Aşağıdaki örnek Eşleştiricisi herhangi bir algılayıcı telemetri olay üzerinde true değerlendirir `"Temperature"` veri türü değeri olarak. Kimliği doğrulanmış bir HTTP POST isteği yaparak, bir kullanıcı tanımlı işlev üzerinde birden fazla matchers oluşturabilirsiniz:

```plaintext
YOUR_MANAGEMENT_API_URL/matchers
```

JSON gövdesi ile:

```JSON
{
  "Name": "Temperature Matcher",
  "Conditions": [
    {
      "target": "Sensor",
      "path": "$.dataType",
      "value": "\"Temperature\"",
      "comparison": "Equals"
    }
  ],
  "SpaceId": "YOUR_SPACE_IDENTIFIER"
}
```

| Değer | Şununla değiştir |
| --- | --- |
| YOUR_SPACE_IDENTIFIER | Örneğiniz üzerinde barındırılıyorsa hangi sunucu bölge |

## <a name="create-a-user-defined-function"></a>Kullanıcı tanımlı işlev oluşturma

Kullanıcı tanımlı bir işlev oluşturma, Azure dijital İkizlerini yönetim API'leri için çok bölümlü bir HTTP isteğinin yapılmasını gerektirir.

[!INCLUDE [Digital Twins multipart requests](../../includes/digital-twins-multipart.md)]

Matchers oluşturulduktan sonra aşağıdaki kimliği doğrulanmış çok bölümlü HTTP POST isteği işlevi parçacığıyla karşıya yükleyin:

```plaintext
YOUR_MANAGEMENT_API_URL/userdefinedfunctions
```

Aşağıdaki metni kullanın:

```plaintext
--USER_DEFINED_BOUNDARY
Content-Type: application/json; charset=utf-8
Content-Disposition: form-data; name="metadata"

{
  "spaceId": "YOUR_SPACE_IDENTIFIER",
  "name": "User Defined Function",
  "description": "The contents of this udf will be executed when matched against incoming telemetry.",
  "matchers": ["YOUR_MATCHER_IDENTIFIER"]
}
--USER_DEFINED_BOUNDARY
Content-Disposition: form-data; name="contents"; filename="userDefinedFunction.js"
Content-Type: text/javascript

function process(telemetry, executionContext) {
  // Code goes here.
}

--USER_DEFINED_BOUNDARY--
```

| Değer | Şununla değiştir |
| --- | --- |
| USER_DEFINED_BOUNDARY | Çok bölümlü içerik sınır adı |
| YOUR_SPACE_IDENTIFIER | Alanı tanımlayıcısı  |
| YOUR_MATCHER_IDENTIFIER | Kullanmak istediğiniz Eşleştiricisi kimliği |

1. Üst bilgileri içerdiğini doğrulayın: `Content-Type: multipart/form-data; boundary="USER_DEFINED_BOUNDARY"`.
1. Çok bölümlü gövde olduğundan emin olun:

   - İlk bölümü, kullanıcı tanımlı işlev gerekli meta veriler içerir.
   - İkinci bölümü, JavaScript işlem mantığını içerir.

1. İçinde **USER_DEFINED_BOUNDARY** bölümünde, değiştirin **spaceId** (`YOUR_SPACE_IDENTIFIER`) ve **matchers** (`YOUR_MATCHER_IDENTIFIER`) değerleri.
1. JavaScript kullanıcı tanımlı işlevi olarak sağlanır doğrulayın `Content-Type: text/javascript`.

### <a name="example-functions"></a>Örnek işlevleri

Algılayıcı telemetri algılayıcı için doğrudan veri türüyle okuma ayarlamak **sıcaklık**, olduğu `sensor.DataType`:

```JavaScript
function process(telemetry, executionContext) {

  // Get sensor metadata
  var sensor = getSensorMetadata(telemetry.SensorId);

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Set the sensor reading as the current value for the sensor.
  setSensorValue(telemetry.SensorId, sensor.DataType, parseReading.SensorValue);
}
```

**Telemetri** parametresi sunan **SensorId** ve **ileti** öznitelikleri, karşılık gelen bir algılayıcı tarafından gönderilen ileti. **ExecutionContext** parametre aşağıdaki öznitelikleri gösterir:

```csharp
var executionContext = new UdfExecutionContext
{
    EnqueuedTime = request.HubEnqueuedTime,
    ProcessorReceivedTime = request.ProcessorReceivedTime,
    UserDefinedFunctionId = request.UserDefinedFunctionId,
    CorrelationId = correlationId.ToString(),
};
```

Telemetri algılayıcı okuma önceden tanımlanmış bir eşik değerini geçiyor, sonraki örnekte, biz bir ileti Kaydet. Azure dijital İkizlerini örneğinde tanılama ayarlarınızı etkinse, kullanıcı tanımlı işlevleri günlüklerinden de iletilir:

```JavaScript
function process(telemetry, executionContext) {

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Example sensor telemetry reading range is greater than 0.5
  if(parseFloat(parseReading.SensorValue) > 0.5) {
    log(`Alert: Sensor with ID: ${telemetry.SensorId} detected an anomaly!`);
  }
}
```

Önceden tanımlanmış sabitinin sıcaklık düzeye yükseldiğinde, aşağıdaki kod bir bildirim tetikleyen:

```JavaScript
function process(telemetry, executionContext) {

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Define threshold
  var threshold = 70;

  // Trigger notification 
  if(parseInt(parseReading) > threshold) {
    var alert = {
      message: 'Temperature reading has surpassed threshold',
      sensorId: telemetry.SensorId,
      reading: parseReading
    };

    sendNotification(telemetry.SensorId, "Sensor", JSON.stringify(alert));
  }
}
```

Bir daha karmaşık kullanıcı tanımlı işlev kod örneği için bkz. [doluluk hızlı](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availability.js).

## <a name="create-a-role-assignment"></a>Rol ataması oluşturma

Altında çalıştırılacak kullanıcı tanımlı işlevi için bir rol ataması oluşturun. Kullanıcı tanımlı işlev için hiçbir rol ataması zaten varsa, grafik nesnelerde eylemler gerçekleştirme erişimine sahiptir ya da Yönetim API ile etkileşim için doğru izinleri olmaz. Bir kullanıcı tanımlı işlev gerçekleştirebilir eylemleri belirtilir ve rol tabanlı erişim denetimi içinde Azure dijital İkizlerini yönetim API'leri aracılığıyla tanımlanır. Örneğin, kullanıcı tanımlı işlevleri kapsamda belirli roller ya da belirli erişim denetimi yolları belirterek sınırlı olabilir. Daha fazla bilgi için [rol tabanlı erişim denetimi](./security-role-based-access-control.md) belgeleri.

1. [Sistem API sorgu](./security-create-manage-role-assignments.md#all) kullanıcı tanımlı işlevinize atamak istediğiniz rolü Kimliği almak tüm roller için. Kimliği doğrulanmış bir HTTP GET isteği yaparak bunu:

    ```plaintext
    YOUR_MANAGEMENT_API_URL/system/roles
    ```
   Korumak istediğiniz rol kimliği. JSON gövdesi özniteliği olarak geçirilecek **Roleıd** (`YOUR_DESIRED_ROLE_IDENTIFIER`) altında.

1. **objectID** (`YOUR_USER_DEFINED_FUNCTION_ID`) daha önce oluşturulan kullanıcı tanımlı işlev kimliği olacaktır.
1. Değerini bulun **yolu** (`YOUR_ACCESS_CONTROL_PATH`), boşluk ile sorgulamak `fullpath`.
1. Döndürülen kopyalama `spacePaths` değeri. Kullanacağınız aşağıda. Kimliği doğrulanmış bir HTTP GET isteği olun:

    ```plaintext
    YOUR_MANAGEMENT_API_URL/spaces?name=YOUR_SPACE_NAME&includes=fullpath
    ```

    | Değer | Şununla değiştir |
    | --- | --- |
    | YOUR_SPACE_NAME | Kullanmak istediğiniz alanı adı |

1. Döndürülen yapıştırın `spacePaths` içine değer **yolu** kimliği doğrulanmış bir HTTP POST isteği yaparak kullanıcı tanımlı işlev rol ataması oluşturmak için:

    ```plaintext
    YOUR_MANAGEMENT_API_URL/roleassignments
    ```
    JSON gövdesi ile:

    ```JSON
    {
      "roleId": "YOUR_DESIRED_ROLE_IDENTIFIER",
      "objectId": "YOUR_USER_DEFINED_FUNCTION_ID",
      "objectIdType": "YOUR_USER_DEFINED_FUNCTION_TYPE_ID",
      "path": "YOUR_ACCESS_CONTROL_PATH"
    }
    ```

    | Değer | Şununla değiştir |
    | --- | --- |
    | YOUR_DESIRED_ROLE_IDENTIFIER | İstenen rol tanımlayıcısı |
    | YOUR_USER_DEFINED_FUNCTION_ID | Kullanmak istediğiniz kullanıcı tanımlı işlev kimliği |
    | YOUR_USER_DEFINED_FUNCTION_TYPE_ID | Kullanıcı tanımlı işlev türü belirtme kimliği |
    | YOUR_ACCESS_CONTROL_PATH | Erişim denetimi yolu |

>[!TIP]
> Makaleyi okuyun [oluşturmak ve rol atamalarını yönetmek nasıl](./security-create-manage-role-assignments.md) kullanıcı tanımlı işlev yönetim API işlemleri ve uç noktaları hakkında daha fazla bilgi.

## <a name="send-telemetry-to-be-processed"></a>İşlenecek telemetri gönderme

Uzamsal zeka grafikte tanımlanan algılayıcı telemetri gönderir. Buna karşılık, telemetriyi karşıya yüklenen kullanıcı tanımlı bir işlevin yürütülmesini tetikler. Veri işlemcisi telemetriyi seçer. Ardından bir çalıştırma planı, kullanıcı tanımlı işlevi çağırma için oluşturulur.

1. Okuma kaynaklandığı algılayıcı için matchers alın.
1. Hangi matchers başarıyla bağlı olarak değerlendirildi, ilişkili kullanıcı tanımlı işlevleri alın.
1. Her kullanıcı tanımlı işlevi yürütür.

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi nasıl [dijital İkizlerini Azure uç noktaları oluşturma](./how-to-egress-endpoints.md) olayları göndermek için.

- Azure dijital İkizlerini yönlendirme hakkında daha fazla ayrıntı için okuma [yönlendirme olayları ve iletileri](./concepts-events-routing.md).

- Gözden geçirme [istemci Kitaplığı Başvurusu belgeleri](./reference-user-defined-functions-client-library.md).
