---
title: Azure dijital İkizlerini kullanıcı tanımlı işlevler kullanma | Microsoft Docs
description: Kullanıcı tanımlı işlevler, matchers ve rol atamaları Azure ile dijital İkizlerini oluşturma kılavuzu.
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: alinast
ms.openlocfilehash: 33190472215e7a02b94951a73054ebe3e1994e54
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51623919"
---
# <a name="how-to-use-user-defined-functions-in-azure-digital-twins"></a>Azure dijital İkizlerini kullanıcı tanımlı işlevler kullanma

[Kullanıcı tanımlı işlevleri](./concepts-user-defined-functions.md) özel mantığı gelen telemetri iletilerini ve uzamsal graf meta verileri karşı çalıştırmak için kullanıcı (UDF) etkinleştirin. Ardından kullanıcı önceden tanımlı uç nokta için olay gönderebilirsiniz. Bu kılavuzda, sıcaklık olayları algılamak için acting örneği anlatılmaktadır ve tüm okuma uyar, belirli bir sıcaklık aşıyor.

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

## <a name="client-library-reference"></a>İstemci Kitaplığı Başvurusu

Kullanıcı tanımlı işlevler çalışma zamanı yardımcı yöntemler olarak kullanılabilir olan işlevlerin listelenir [istemci başvurusu](#Client-Reference) bölümü.

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

Aşağıdaki örnek Eşleştiricisi herhangi bir algılayıcı telemetri olay üzerinde true değerlendirir `"Temperature"` veri türü değeri olarak. Bir kullanıcı tanımlı işlev üzerinde birden fazla matchers oluşturabilirsiniz:

```plaintext
POST yourManagementApiUrl/matchers
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

| Değeriniz | Şununla değiştir |
| --- | --- |
| YOUR_SPACE_IDENTIFIER | Örneğiniz üzerinde barındırılıyorsa hangi sunucu bölge |

## <a name="create-a-user-defined-function-udf"></a>Bir kullanıcı tanımlı işlev (UDF) oluşturma

İşlev kod parçacığını aşağıdaki matchers oluşturulduktan sonra karşıya **POST** çağırın:

> [!IMPORTANT]
> - Aşağıdaki üst bilgilerinde ayarlayın `Content-Type: multipart/form-data; boundary="userDefinedBoundary"`.
> - Çok bölümlü gövde:
>   - İlk UDF için gerekli meta veriler hakkında bir parçasıdır.
>   - JavaScript işlem mantığı ikinci bölümüdür.
> - İçinde **userDefinedBoundary** bölümünde, değiştirin **SpaceId** ve **Machers** değerleri.

```plaintext
POST yourManagementApiUrl/userdefinedfunctions with Content-Type: multipart/form-data; boundary="userDefinedBoundary"
```

| Parametre değeri | Şununla değiştir |
| --- | --- |
| *userDefinedBoundary* | Çok parçalı içerik sınır adı |

### <a name="body"></a>Gövde

```plaintext
--userDefinedBoundary
Content-Type: application/json; charset=utf-8
Content-Disposition: form-data; name="metadata"

{
  "SpaceId": "YOUR_SPACE_IDENTIFIER",
  "Name": "User Defined Function",
  "Description": "The contents of this udf will be executed when matched against incoming telemetry.",
  "Matchers": ["YOUR_MATCHER_IDENTIFIER"]
}
--userDefinedBoundary
Content-Disposition: form-data; name="contents"; filename="userDefinedFunction.js"
Content-Type: text/javascript

function process(telemetry, executionContext) {
  // Code goes here.
}

--userDefinedBoundary--
```

| Değeriniz | Şununla değiştir |
| --- | --- |
| YOUR_SPACE_IDENTIFIER | Alanı tanımlayıcısı  |
| YOUR_MATCHER_IDENTIFIER | Kullanmak istediğiniz Eşleştiricisi kimliği |

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

Daha karmaşık bir UDF kod örneği için [kullanılabilir alanları yeni bir uzaktan UDF ile kontrol](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availability.js).

## <a name="create-a-role-assignment"></a>Rol ataması oluşturma

Biz altında çalıştırılacak kullanıcı tanımlı işlevi için bir rol ataması oluşturmanız gerekir. Biz Aksi takdirde, grafik nesnelerde eylemler gerçekleştirmek için yönetim API'si ile etkileşim için uygun izinlere olmaz. Kullanıcı tanımlı işlev gerçekleştirdiği eylemleri, rol tabanlı erişim denetimi Azure dijital İkizlerini yönetim API'leri içinde muaf değildir. Bunlar belirli roller ya da belirli erişim denetimi yolları belirterek kapsamda sınırlanabilir. Daha fazla bilgi için [rol tabanlı erişim denetimi](./security-role-based-access-control.md) belgeleri.

1. Roller için sorgulama ve UDF için atamak istediğiniz rolü Kimliğini alın. Geçirin **Roleıd**:

    ```plaintext
    GET yourManagementApiUrl/system/roles
    ```

1. **ObjectID** daha önce oluşturulan UDF kimliği olacaktır.
1. Değerini bulun **yolu** alanlarınıza ile sorgulamak `fullpath`.
1. Döndürülen kopyalama `spacePaths` değeri. Aşağıdaki kodda, kullanacaksınız:

    ```plaintext
    GET yourManagementApiUrl/spaces?name=yourSpaceName&includes=fullpath
    ```

    | Parametre değeri | Şununla değiştir |
    | --- | --- |
    | *yourSpaceName* | Kullanmak istediğiniz alanı adı |

1. Döndürülen yapıştırın `spacePaths` içine değer **yolu** UDF rol ataması oluşturmak için:

    ```plaintext
    POST yourManagementApiUrl/roleassignments
    {
      "RoleId": "YOUR_DESIRED_ROLE_IDENTIFIER",
      "ObjectId": "YOUR_USER_DEFINED_FUNCTION_ID",
      "ObjectIdType": "YOUR_USER_DEFINED_FUNCTION_TYPE_ID",
      "Path": "YOUR_ACCESS_CONTROL_PATH"
    }
    ```

    | Değeriniz | Şununla değiştir |
    | --- | --- |
    | YOUR_DESIRED_ROLE_IDENTIFIER | İstenen rol tanımlayıcısı |
    | YOUR_USER_DEFINED_FUNCTION_ID | Kullanmak istediğiniz UDF kimliği |
    | YOUR_USER_DEFINED_FUNCTION_TYPE_ID | UDF türünü belirterek kimliği |
    | YOUR_ACCESS_CONTROL_PATH | Erişim denetimi yolu |

## <a name="send-telemetry-to-be-processed"></a>İşlenecek telemetri gönderme

Karşıya yüklenen kullanıcı tanımlı işlevin çalışması telemetri algılayıcı grafikte açıklanan tarafından oluşturulan tetikler. Veri işlemcisi telemetriyi seçer. Ardından bir çalıştırma planı, kullanıcı tanımlı işlevi çağırma için oluşturulur.

1. Okuma kapatıp oluşturulan algılayıcı için matchers alın.
1. Hangi matchers başarıyla Değerlendirilmiş bağlı olarak, ilişkili kullanıcı tanımlı işlevleri alın.
1. Her kullanıcı tanımlı işlevi çalıştırın.

## <a name="client-reference"></a>İstemci başvurusu

### <a name="getspacemetadataid--space"></a>getSpaceMetadata(id) ⇒ `space`

Bir alanı tanımlayıcısı göz önünde bulundurulduğunda, bu işlevi grafikten yer alır.

**Tür**: genel işlevi

| Parametre  | Tür                | Açıklama  |
| ---------- | ------------------- | ------------ |
| *Kimliği*  | `guid` | alanı tanımlayıcısı |

### <a name="getsensormetadataid--sensor"></a>getSensorMetadata(id) ⇒ `sensor`

Bir algılayıcı tanımlayıcı göz önünde bulundurulduğunda, bu işlev grafikten algılayıcı alır.

**Tür**: genel işlevi

| Parametre  | Tür                | Açıklama  |
| ---------- | ------------------- | ------------ |
| *Kimliği*  | `guid` | Algılayıcı tanımlayıcısı |

### <a name="getdevicemetadataid--device"></a>getDeviceMetadata(id) ⇒ `device`

Cihaz tanımlayıcısı göz önünde bulundurulduğunda, bu işlevi cihaz grafikten alır.

**Tür**: genel işlevi

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *Kimliği* | `guid` | Cihaz tanımlayıcısı |

### <a name="getsensorvaluesensorid-datatype--value"></a>⇒ getSensorValue (sensorId, veri türü) `value`

Verilen bir algılayıcı tanımlayıcısı ve kendi veri türüne, bu işlev, algılayıcı için geçerli değeri alır.

**Tür**: genel işlevi

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *sensorId*  | `guid` | Algılayıcı tanımlayıcısı |
| *Veri türü*  | `string` | Algılayıcı veri türü |

### <a name="getspacevaluespaceid-valuename--value"></a>(spaceId, valueName) getSpaceValue ⇒ `value`

Bir alanı tanımlayıcısı ve değer adı verildiğinde, bu işlev, alan özelliğinin geçerli değeri alır.

**Tür**: genel işlevi

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *spaceId*  | `guid` | alanı tanımlayıcısı |
| *değer adı* | `string` | özellik adı alanı |

### <a name="getsensorhistoryvaluessensorid-datatype--value"></a>⇒ getSensorHistoryValues (sensorId, veri türü) `value[]`

Verilen bir algılayıcı tanımlayıcısı ve veri türü, bu işlev, algılayıcı için geçmiş değerlerle alır.

**Tür**: genel işlevi

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | Algılayıcı tanımlayıcısı |
| *Veri türü* | `string` | Algılayıcı veri türü |

### <a name="getspacehistoryvaluesspaceid-datatype--value"></a>⇒ getSpaceHistoryValues (spaceId, veri türü) `value[]`

Bir alanı tanımlayıcısı ve değer adı verildiğinde, bu işlev Geçmiş alanı bu özellik değerlerini alır.

**Tür**: genel işlevi

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | alanı tanımlayıcısı |
| *değer adı* | `string` | özellik adı alanı |

### <a name="getspacechildspacesspaceid--space"></a>getSpaceChildSpaces(spaceId) ⇒ `space[]`

Bir alanı tanımlayıcısı göz önünde bulundurulduğunda, bu işlev, üst alanı için alt alanları alır.

**Tür**: genel işlevi

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | alanı tanımlayıcısı |

### <a name="getspacechildsensorsspaceid--sensor"></a>getSpaceChildSensors(spaceId) ⇒ `sensor[]`

Bir alanı tanımlayıcısı göz önünde bulundurulduğunda, bu işlev, üst alanı alt sensörlerden alır.

**Tür**: genel işlevi

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | alanı tanımlayıcısı |

### <a name="getspacechilddevicesspaceid--device"></a>getSpaceChildDevices(spaceId) ⇒ `device[]`

Bir alanı tanımlayıcısı göz önünde bulundurulduğunda, bu işlevi alt cihazlar için bu üst yer alır.

**Tür**: genel işlevi

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | alanı tanımlayıcısı |

### <a name="getdevicechildsensorsdeviceid--sensor"></a>getDeviceChildSensors(deviceId) ⇒ `sensor[]`

Cihaz tanımlayıcısı göz önünde bulundurulduğunda, bu işlev bu üst cihaz için alt sensörlerden alır.

**Tür**: genel işlevi

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *cihaz kimliği* | `guid` | Cihaz tanımlayıcısı |

### <a name="getspaceparentspacechildspaceid--space"></a>getSpaceParentSpace(childSpaceId) ⇒ `space`

Bu işlev bir alanı tanımlayıcısı göz önünde bulundurulduğunda, kendi üst alanını alır.

**Tür**: genel işlevi

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *childSpaceId* | `guid` | alanı tanımlayıcısı |

### <a name="getsensorparentspacechildsensorid--space"></a>getSensorParentSpace(childSensorId) ⇒ `space`

Bir algılayıcı tanımlayıcı göz önünde bulundurulduğunda, bu işlev kendi üst alanını alır.

**Tür**: genel işlevi

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *childSensorId* | `guid` | Algılayıcı tanımlayıcısı |

### <a name="getdeviceparentspacechilddeviceid--space"></a>getDeviceParentSpace(childDeviceId) ⇒ `space`

Bu işlev, cihaz tanımlayıcısı göz önünde bulundurulduğunda, kendi üst alanını alır.

**Tür**: genel işlevi

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *childDeviceId* | `guid` | Cihaz tanımlayıcısı |

### <a name="getsensorparentdevicechildsensorid--space"></a>getSensorParentDevice(childSensorId) ⇒ `space`

Bir algılayıcı tanımlayıcı göz önünde bulundurulduğunda, bu işlev, üst cihaz alır.

**Tür**: genel işlevi

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *childSensorId* | `guid` | Algılayıcı tanımlayıcısı |

### <a name="getspaceextendedpropertyspaceid-propertyname--extendedproperty"></a>(spaceId, propertyName) getSpaceExtendedProperty ⇒ `extendedProperty`

Bir alanı tanımlayıcısı göz önünde bulundurulduğunda, bu işlevin özelliği ve değerini alanından alır.

**Tür**: genel işlevi

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | alanı tanımlayıcısı |
| *PropertyName* | `string` | özellik adı alanı |

### <a name="getsensorextendedpropertysensorid-propertyname--extendedproperty"></a>(sensorId, propertyName) getSensorExtendedProperty ⇒ `extendedProperty`

Bir algılayıcı tanımlayıcı göz önünde bulundurulduğunda, bu işlevin özelliği ve değerini algılayıcıdan alır.

**Tür**: genel işlevi

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | Algılayıcı tanımlayıcısı |
| *PropertyName* | `string` | Algılayıcı özellik adı |

### <a name="getdeviceextendedpropertydeviceid-propertyname--extendedproperty"></a>(cihaz kimliği, propertyName) getDeviceExtendedProperty ⇒ `extendedProperty`

Cihaz tanımlayıcısı göz önünde bulundurulduğunda, bu işlevin özelliği ve değerini CİHAZDAN alır.

**Tür**: genel işlevi

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *cihaz kimliği* | `guid` | Cihaz tanımlayıcısı |
| *PropertyName* | `string` | cihaz özellik adı |

### <a name="setsensorvaluesensorid-datatype-value"></a>setSensorValue (sensorId, veri türü, değer)

Bu işlev, belirtilen veri türü ile algılayıcı nesnede bir değer ayarlar.

**Tür**: genel işlevi

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | Algılayıcı tanımlayıcısı |
| *Veri türü*  | `string` | Algılayıcı veri türü |
| *value*  | `string` | Değer |

### <a name="setspacevaluespaceid-datatype-value"></a>setSpaceValue (spaceId, veri türü, değer)

Bu işlev, belirtilen veri türüne sahip alan nesne üzerinde bir değer ayarlar.

**Tür**: genel işlevi

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | alanı tanımlayıcısı |
| *Veri türü* | `string` | Veri türü |
| *value* | `string` | Değer |

### <a name="logmessage"></a>log(Message)

Bu işlev kullanıcı tanımlı işlev içinde aşağıdaki iletiyi günlüğe kaydeder.

**Tür**: genel işlevi

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *İleti* | `string` | günlüğe kaydedilecek ileti |

### <a name="sendnotificationtopologyobjectid-topologyobjecttype-payload"></a>sendNotification (topologyObjectId, topologyObjectType, yükü)

Bu işlev özel bir bildirim gönderilecek gönderdiği.

**Tür**: genel işlevi

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *topologyObjectId*  | `guid` | Nesne tanımlayıcısı grafiğini oluşturun. Örnekler alanı, sensör ve cihaz kimliği.|
| *topologyObjectType*  | `string` | Sensör ve cihaz verilebilir.|
| *Yükü*  | `string` | Bildirimi gönderilecek JSON yükü. |

## <a name="return-types"></a>Dönüş türleri

Aşağıdaki modelleri önceki istemci başvurusu dönüş nesneleri açıklanır.

### <a name="space"></a>Uzay

```JSON
{
  "Id": "00000000-0000-0000-0000-000000000000",
  "Name": "Space",
  "FriendlyName": "Conference Room",
  "TypeId": 0,
  "ParentSpaceId": "00000000-0000-0000-0000-000000000001",
  "SubtypeId": 0
}
```

### <a name="space-methods"></a>Alanı yöntemleri

#### <a name="parent--space"></a>Parent() ⇒ `space`

Bu işlev geçerli alanının üst alanı döndürür.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Bu işlev geçerli alanı sensörlerden alt öğesini döndürür.

#### <a name="childdevices--device"></a>ChildDevices() ⇒ `device[]`

Bu işlev, cihazların geçerli alanı alt döndürür.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Bu işlev, genişletilmiş özellik ve geçerli bir alan değerini döndürür.

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *PropertyName* | `string` | Genişletilmiş özellik adı |

#### <a name="valuevaluename--value"></a>Value(ValueName) ⇒ `value`

Bu işlev, geçerli alanı değerini döndürür.

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *değer adı* | `string` | değer adı |

#### <a name="historyvaluename--value"></a>History(ValueName) ⇒ `value[]`

Bu işlev, geçerli alanı geçmiş değerlerini döndürür.

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *değer adı* | `string` | değer adı |

#### <a name="notifypayload"></a>Notify(Payload)

Bu işlev, belirtilen yüküyle bir bildirim gönderir.

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *Yükü* | `string` | Bildirime eklenecek JSON yükü |

### <a name="device"></a>Cihaz

```JSON
{
  "Id": "00000000-0000-0000-0000-000000000002",
  "Name": "Device",
  "FriendlyName": "Temperature Sensing Device",
  "Description": "This device contains a sensor that captures temperature readings.",
  "Type": "None",
  "Subtype": "None",
  "TypeId": 0,
  "SubtypeId": 0,
  "HardwareId": "ABC123",
  "GatewayId": "ABC",
  "SpaceId": "00000000-0000-0000-0000-000000000000"
}
```

### <a name="device-methods"></a>Cihaz yöntemleri

#### <a name="parent--space"></a>Parent() ⇒ `space`

Bu işlev, geçerli cihaz üst alanını döndürür.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Bu işlev geçerli cihazın sensör alt öğesini döndürür.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Bu işlev, genişletilmiş özellik ve geçerli cihaz için değerini döndürür.

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *PropertyName* | `string` | Genişletilmiş özellik adı |

#### <a name="notifypayload"></a>Notify(Payload)

Bu işlev, belirtilen yüküyle bir bildirim gönderir.

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *Yükü* | `string` | Bildirime eklenecek JSON yükü |

### <a name="sensor"></a>Algılayıcı

```JSON
{
  "Id": "00000000-0000-0000-0000-000000000003",
  "Port": "30",
  "PollRate": 3600,
  "DataType": "Temperature",
  "DataSubtype": "None",
  "Type": "Classic",
  "PortType": "None",
  "DataUnitType": "FahrenheitTemperature",
  "SpaceId": "00000000-0000-0000-0000-000000000000",
  "DeviceId": "00000000-0000-0000-0000-000000000001",
  "PortTypeId": 0,
  "DataUnitTypeId": 0,
  "DataTypeId": 0,
  "DataSubtypeId": 0,
  "TypeId": 0  
}
```

### <a name="sensor-methods"></a>Algılayıcı yöntemleri

#### <a name="space--space"></a>Space() ⇒ `space`

Bu işlev, geçerli algılayıcı üst alanını döndürür.

#### <a name="device--device"></a>Device() ⇒ `device`

Bu işlev, geçerli algılayıcı üst cihaz döndürür.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Bu işlev, genişletilmiş özellik ve geçerli algılayıcı için değerini döndürür.

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *PropertyName* | `string` | Genişletilmiş özellik adı |

#### <a name="value--value"></a>Value()) ⇒ `value`

Bu işlev, geçerli algılayıcı değerini döndürür.

#### <a name="history--value"></a>History() ⇒ `value[]`

Bu işlev, geçerli algılayıcı geçmiş değerlerini döndürür.

#### <a name="notifypayload"></a>Notify(Payload)

Bu işlev, belirtilen yüküyle bir bildirim gönderir.

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *Yükü* | `string` | Bildirime eklenecek JSON yükü |

### <a name="value"></a>Değer

```JSON
{
  "DataType": "Temperature",
  "Value": "70",
  "CreatedTime": ""
}
```

### <a name="extended-property"></a>Genişletilmiş özellik

```JSON
{
  "Name": "OccupancyStatus",
  "Value": "Occupied"
}
```

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi nasıl [dijital İkizlerini Azure uç noktaları oluşturma](how-to-egress-endpoints.md) olayları göndermek için.

- Azure dijital İkizlerini uç noktaları hakkında daha fazla ayrıntı için bilgi [uç noktaları hakkında daha fazla](concepts-events-routing.md).
