---
title: Azure dijital İkizlerini kullanıcı tanımlı işlevler kullanma | Microsoft Docs
description: Kullanıcı tanımlı işlevler, matchers ve rol atamaları Azure ile dijital İkizlerini oluşturma kılavuzu.
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: alinast
ms.openlocfilehash: 7fbaff5ed1b60a4434ba2eb0c78c6aa1f3fd6645
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324342"
---
# <a name="how-to-use-user-defined-functions-in-azure-digital-twins"></a>Azure dijital İkizlerini kullanıcı tanımlı işlevler kullanma

[Kullanıcı tanımlı işlevleri](./concepts-user-defined-functions.md) özel mantığı, gelen telemetri iletilerini ve uzamsal graf meta verilerini, olayları göndermek için önceden tanımlı uç noktaları kullanıcının karşı çalıştırmak kullanıcıyı etkinleştirir. Bu kılavuzdaki sıcaklık olayları algılamak için acting örneği alacağız ve tüm okuma uyar, belirli bir sıcaklık aşıyor.

Aşağıdaki örneklerde `https://yourManagementApiUrl` dijital İkizlerini API URI'sini belirtir:

```plaintext
https://yourInstanceName.yourLocation.azuresmartspaces.net/management
```

| Özel öznitelik adı | Değiştirin |
| --- | --- |
| `yourInstanceName` | Azure dijital İkizlerini örneğinizin adı |
| `yourLocation` | Örneğiniz üzerinde barındırılıyorsa hangi sunucu bölge |

## <a name="client-library-reference"></a>İstemci Kitaplığı Başvurusu

Kullanıcı tanımlı işlevler çalışma zamanı yardımcı yöntemler olarak kullanılabilen işlevleri aşağıdaki numaralandırılmış [istemci başvurusu](#Client-Reference).

## <a name="create-a-matcher"></a>Bir Eşleştiricisi oluşturma

Matchers hangi kullanıcı tanımlı işlevler için belirli bir telemetri iletisi yürütülecek belirlemek graf nesneleridir.

Geçerli Eşleştiricisi koşul karşılaştırmalar:

- `Equals`
- `NotEquals`
- `Contains`

Geçerli Eşleştiricisi koşul hedefleri:

- `Sensor`
- `SensorDevice`
- `SensorSpace`

Aşağıdaki örnek Eşleştiricisi true ile herhangi bir algılayıcı telemetri olayı olarak değerlendirilecektir `Temperature` veri türü değeri olarak. Bir kullanıcı tanımlı işlev üzerinde birden fazla matchers oluşturabilirsiniz.

```text
POST https://yourManagementApiUrl/api/v1.0/matchers
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
  "SpaceId": "yourSpaceIdentifier"
}
```

| Özel öznitelik adı | Değiştirin |
| --- | --- |
| `yourManagementApiUrl` | Yönetim API'niz için tam URL yolu  |
| `yourSpaceIdentifier` | Örneğiniz üzerinde barındırılıyorsa hangi sunucu bölge |

## <a name="create-a-user-defined-function-udf"></a>Bir kullanıcı tanımlı işlev (UDF) oluşturma

Matchers sonra oluşturulan, aşağıdaki POST çağrısına işlevi parçacığıyla karşıya yükleyin:

> [!IMPORTANT]
> - Aşağıdaki üst bilgilerinde ayarlayın `Content-Type: multipart/form-data; boundary="userDefinedBoundary"`.
> - Çok bölümlü gövde:
>   - İlk UDF için gerekli meta veriler hakkında bir parçasıdır.
>   - Javascript işlem mantığı ikinci bölümüdür.
> - Değiştir `userDefinedBoundary` bölümü `SpaceId` ve `Machers` GUID.

```plaintext
POST https://yourManagementApiUrl/api/v1.0/userdefinedfunctions with Content-Type: multipart/form-data; boundary="userDefinedBoundary"
```

| Özel öznitelik adı | Değiştirin |
| --- | --- |
| `yourManagementApiUrl` | Yönetim API'niz için tam URL yolu  |

Gövde:

```plaintext
--userDefinedBoundary
Content-Type: application/json; charset=utf-8
Content-Disposition: form-data; name="metadata"

{
  "SpaceId": "yourSpaceIdentifier",
  "Name": "User Defined Function",
  "Description": "The contents of this udf will be executed when matched against incoming telemetry.",
  "Matchers": ["yourMatcherIdentifier"]
}
--userDefinedBoundary
Content-Disposition: form-data; name="contents"; filename="userDefinedFunction.js"
Content-Type: text/javascript

function process(telemetry, executionContext) {
  // Code goes here.
}

--userDefinedBoundary--
```

| Özel öznitelik adı | Değiştirin |
| --- | --- |
| `yourSpaceIdentifier` | Alanı tanımlayıcısı  |
| `yourMatcherIdentifier` | Kullanmak istediğiniz Eşleştiricisi kimliği |

### <a name="example-functions"></a>Örnek işlevleri

Algılayıcı telemetri algılayıcı için doğrudan veri türüyle okuma ayarlamak `Temperature`, algılayıcı olduğu. Veri türü:

```javascript
function process(telemetry, executionContext) {

  // Get sensor metadata
  var sensor = getSensorMetadata(telemetry.SensorId);

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Set the sensor reading as the current value for the sensor.
  setSensorValue(telemetry.SensorId, sensor.DataType, parseReading.SensorValue);
}
```

Bir ileti oturum telemetri algılayıcı okuma önceden tanımlanmış bir eşik değerini geçiyor. Tanılama ayarlarınızı dijital İkizlerini örneğinde etkinse, kullanıcı tanımlı işlevleri günlüklerinden iletilir:

```javascript
function process(telemetry, executionContext) {

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Example sensor telemetry reading range is greater than 0.5
  if(parseFloat(parseReading.SensorValue) > 0.5) {
    log(`Alert: Sensor with ID: ${telemetry.SensorId} detected an anomaly!`);
  }
}
```

Önceden tanımlanmış sabit sıcaklık düzeye yükseldiğinde, aşağıdaki kod bir uyarı tetikler.

```javascript
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

Başvurmak için daha karmaşık bir UDF kod örneği, [kullanılabilir alanları yeni hava UDF ile işaretleyin](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availability.js)

## <a name="create-a-role-assignment"></a>Rol ataması oluşturma

Biz altında çalıştırılacak kullanıcı tanımlı işlevi için bir rol ataması oluşturmanız gerekir. Biz yapmazsanız, grafik nesnelerde eylemler gerçekleştirmek için yönetim API'si ile etkileşim için uygun izinlere sahip değil. Kullanıcı tanımlı işlev gerçekleştirdiği eylemleri, dijital İkizlerini yönetim API'leri rol tabanlı erişim denetimine muaf değildir. Bunlar belirli roller ya da belirli erişim denetimi yolları belirterek kapsamda sınırlanabilir. Daha fazla bilgi için [rol tabanlı erişim denetimi](./security-role-based-access-control.md) belgeleri.

- Rolleri için sorgu ve UDF için atamak istediğiniz rolü Kimliğini alın. Aşağıdaki Roleıd geçirin.

```plaintext
GET https://yourManagementApiUrl/api/v1.0/system/roles
```

| Özel öznitelik adı | Değiştirin |
| --- | --- |
| `yourManagementApiUrl` | Yönetim API'niz için tam URL yolu  |

- Daha önce oluşturulan UDF kimliği objectID olacaktır
- Bulma `Path` tam yolu ve kopyalama alanları sorgulamak `spacePaths` değeri. UDF rol ataması oluştururken yolda yapıştırın

```plaintext
GET https://yourManagementApiUrl/api/v1.0/spaces?name=yourSpaceName&includes=fullpath
```

| Özel öznitelik adı | Değiştirin |
| --- | --- |
| `yourManagementApiUrl` | Yönetim API'niz için tam URL yolu  |
| `yourSpaceName` | Kullanmak istediğiniz alanı adı |

```plaintext
POST https://yourManagementApiUrl/api/v1.0/roleassignments
{
  "RoleId": "yourDesiredRoleIdentifier",
  "ObjectId": "yourUserDefinedFunctionId",
  "ObjectIdType": "UserDefinedFunctionId",
  "Path": "yourAccessControlPath"
}
```

| Özel öznitelik adı | Değiştirin |
| --- | --- |
| `yourManagementApiUrl` | Yönetim API'niz için tam URL yolu  |
| `yourDesiredRoleIdentifier` | İstenen rol tanımlayıcısı |
| `yourUserDefinedFunctionId` | Kullanmak istediğiniz UDF kimliği |
| `yourAccessControlPath` | Erişim denetimi yolu |

## <a name="send-telemetry-to-be-processed"></a>İşlenecek telemetri gönderme

Telemetri algılayıcı grafikte açıklanan tarafından oluşturulan, karşıya yüklenen kullanıcı tanımlı bir işlevin yürütülmesini tetiklemek. Telemetri veri işlemcisi tarafından devralındığında, bir çalıştırma planı kullanıcı tanımlı işlevi çağırma için oluşturulur.

1. Okuma kapatıp oluşturulan algılayıcı için matchers alın.
1. Hangi matchers başarıyla Değerlendirilmiş bağlı olarak, ilişkili kullanıcı tanımlı işlevleri alın.
1. Her kullanıcı tanımlı işlevi yürütür.

## <a name="client-reference"></a>İstemci başvurusu

### <a name="getspacemetadataid--space"></a>getSpaceMetadata(id) ⇒ `space`

Belirtilen bir alan tanımlayıcısı grafikten yer alır.

**Tür**: genel işlevi

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| id  | `guid` | alanı tanımlayıcısı |

### <a name="getsensormetadataid--sensor"></a>getSensorMetadata(id) ⇒ `sensor`

Belirtilen bir algılayıcı tanımlayıcısı, algılayıcı grafikten alır.

**Tür**: genel işlevi

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| id  | `guid` | Algılayıcı tanımlayıcısı |

### <a name="getdevicemetadataid--device"></a>getDeviceMetadata(id) ⇒ `device`

Belirtilen bir cihaz tanımlayıcısı, cihaz grafikten alır.

**Tür**: genel işlevi

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| id  | `guid` | Cihaz tanımlayıcısı |

### <a name="getsensorvaluesensorid-datatype--value"></a>⇒ getSensorValue (sensorId, veri türü) `value`

Bir algılayıcı tanımlayıcısı ve kendi veri türüne, algılayıcı için geçerli değer alır.

**Tür**: genel işlevi

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| sensorId  | `guid` | Algılayıcı tanımlayıcısı |
| Veri türü  | `string` | Algılayıcı veri türü |

### <a name="getspacevaluespaceid-valuename--value"></a>(spaceId, valueName) getSpaceValue ⇒ `value`

Bir alanı tanımlayıcısı ve değer adı verildiğinde, bu alanı özelliğinin geçerli değeri alır.

**Tür**: genel işlevi

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| spaceId  | `guid` | alanı tanımlayıcısı |
| değer adı  | `string` | özellik adı alanı |

### <a name="getsensorhistoryvaluessensorid-datatype--value"></a>⇒ getSensorHistoryValues (sensorId, veri türü) `value[]`

Bir algılayıcı tanımlayıcısı ve kendi veri türüne, algılayıcı için geçmiş değerlerle alın.

**Tür**: genel işlevi

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| sensorId  | `guid` | Algılayıcı tanımlayıcısı |
| Veri türü  | `string` | Algılayıcı veri türü |

### <a name="getspacehistoryvaluesspaceid-datatype--value"></a>⇒ getSpaceHistoryValues (spaceId, veri türü) `value[]`

Bir alanı tanımlayıcısı ve değer adı verildiğinde, bu özellik alanı için geçmiş değerlerle alın.

**Tür**: genel işlevi

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| spaceId  | `guid` | alanı tanımlayıcısı |
| değer adı  | `string` | özellik adı alanı |

### <a name="getspacechildspacesspaceid--space"></a>getSpaceChildSpaces(spaceId) ⇒ `space[]`

Bir alanı tanımlayıcısı göz önünde bulundurulduğunda, bu üst alanı için alt alanları alın.

**Tür**: genel işlevi

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| spaceId  | `guid` | alanı tanımlayıcısı |

### <a name="getspacechildsensorsspaceid--sensor"></a>getSpaceChildSensors(spaceId) ⇒ `sensor[]`

Bir alanı tanımlayıcısı göz önünde bulundurulduğunda, bu üst alanı alt sensörlerden alın.

**Tür**: genel işlevi

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| spaceId  | `guid` | alanı tanımlayıcısı |

### <a name="getspacechilddevicesspaceid--device"></a>getSpaceChildDevices(spaceId) ⇒ `device[]`

Bir alanı tanımlayıcısı göz önünde bulundurulduğunda, alt cihazlar için bu üst alanı alın.

**Tür**: genel işlevi

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| spaceId  | `guid` | alanı tanımlayıcısı |

### <a name="getdevicechildsensorsdeviceid--sensor"></a>getDeviceChildSensors(deviceId) ⇒ `sensor[]`

Cihaz tanımlayıcısı göz önünde bulundurulduğunda, bu üst cihaz için alt sensörlerden alın.

**Tür**: genel işlevi

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| deviceId  | `guid` | Cihaz tanımlayıcısı |

### <a name="getspaceparentspacechildspaceid--space"></a>getSpaceParentSpace(childSpaceId) ⇒ `space`

Bir alanı tanımlayıcısı göz önünde bulundurulduğunda, kendi üst alanını alır.

**Tür**: genel işlevi

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| childSpaceId  | `guid` | alanı tanımlayıcısı |

### <a name="getsensorparentspacechildsensorid--space"></a>getSensorParentSpace(childSensorId) ⇒ `space`

Bir algılayıcı tanımlayıcı göz önünde bulundurulduğunda, kendi üst alanını alır.

**Tür**: genel işlevi

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| childSensorId  | `guid` | Algılayıcı tanımlayıcısı |

### <a name="getdeviceparentspacechilddeviceid--space"></a>getDeviceParentSpace(childDeviceId) ⇒ `space`

Cihaz tanımlayıcısı göz önünde bulundurulduğunda, kendi üst yer alır.

**Tür**: genel işlevi

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| childDeviceId  | `guid` | Cihaz tanımlayıcısı |

### <a name="getsensorparentdevicechildsensorid--space"></a>getSensorParentDevice(childSensorId) ⇒ `space`

Bir algılayıcı tanımlayıcı göz önünde bulundurulduğunda, kendi üst cihaz alın.

**Tür**: genel işlevi

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| childSensorId  | `guid` | Algılayıcı tanımlayıcısı |

### <a name="getspaceextendedpropertyspaceid-propertyname--extendedproperty"></a>(spaceId, propertyName) getSpaceExtendedProperty ⇒ `extendedProperty`

Bir alanı tanımlayıcısı göz önünde bulundurulduğunda, alanından özelliği ve değerini alın.

**Tür**: genel işlevi

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| spaceId  | `guid` | alanı tanımlayıcısı |
| PropertyName  | `string` | özellik adı alanı |

### <a name="getsensorextendedpropertysensorid-propertyname--extendedproperty"></a>(sensorId, propertyName) getSensorExtendedProperty ⇒ `extendedProperty`

Bir algılayıcı tanımlayıcı göz önünde bulundurulduğunda, algılayıcıdan özelliği ve değerini alın.

**Tür**: genel işlevi

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| sensorId  | `guid` | Algılayıcı tanımlayıcısı |
| PropertyName  | `string` | Algılayıcı özellik adı |

### <a name="getdeviceextendedpropertydeviceid-propertyname--extendedproperty"></a>(cihaz kimliği, propertyName) getDeviceExtendedProperty ⇒ `extendedProperty`

Cihaz tanımlayıcısı göz önünde bulundurulduğunda, CİHAZDAN özelliği ve değerini alın.

**Tür**: genel işlevi

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| deviceId  | `guid` | Cihaz tanımlayıcısı |
| PropertyName  | `string` | cihaz özellik adı |

### <a name="setsensorvaluesensorid-datatype-value"></a>setSensorValue (sensorId, veri türü, değer)

Belirtilen veri türü ile algılayıcı nesnede bir değer ayarlar.

**Tür**: genel işlevi

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| sensorId  | `guid` | Algılayıcı tanımlayıcısı |
| Veri türü  | `string` | Algılayıcı veri türü |
| değer  | `string` | değer |

### <a name="setspacevaluespaceid-datatype-value"></a>setSpaceValue (spaceId, veri türü, değer)

Belirtilen veri türüne sahip alan nesne üzerinde bir değer ayarlar.

**Tür**: genel işlevi

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| spaceId  | `guid` | alanı tanımlayıcısı |
| Veri türü  | `string` | veri türü |
| değer  | `string` | değer |

### <a name="logmessage"></a>log(Message)

Kullanıcı tanımlı işlev içinde aşağıdaki iletiyi günlüğe kaydeder.

**Tür**: genel işlevi

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| message  | `string` | günlüğe kaydedilecek ileti |

### <a name="sendnotificationtopologyobjectid-topologyobjecttype-payload"></a>sendNotification (topologyObjectId, topologyObjectType, yükü)

Gönderilecek özel bir bildirim gönderir.

**Tür**: genel işlevi

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| topologyObjectId  | `Guid` | Grafik nesne tanımlayıcısı (ör. alan / algılayıcı /device kimliği)|
| topologyObjectType  | `string` | (ör. alan / algılayıcı / cihaz)|
| yük  | `string` | bildirimi gönderilecek json yükü |

## <a name="return-types"></a>Dönüş türleri

Yukarıdaki istemci başvurusu dönüş nesneleri açıklayan modelleri şunlardır:

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

Geçerli alan üst alanı döndürür.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Geçerli alan sensörlerden alt öğesini döndürür.

#### <a name="childdevices--device"></a>ChildDevices() ⇒ `device[]`

Geçerli alan cihazları alt öğesini döndürür.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Genişletilmiş özellik ve geçerli bir alan değerini döndürür.

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| PropertyName | `string` | Genişletilmiş özellik adı |

#### <a name="valuevaluename--value"></a>Value(ValueName) ⇒ `value`

Geçerli alan değerini döndürür.

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| değer adı | `string` | değer adı |

#### <a name="historyvaluename--value"></a>History(ValueName) ⇒ `value[]`

Geçmiş geçerli alan değerlerini döndürür.

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| değer adı | `string` | değer adı |

#### <a name="notifypayload"></a>Notify(Payload)

Belirtilen yük ile bir bildirim gönderir.

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| yük | `string` | bildirime eklenecek json yükü |

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

Geçerli cihaz üst alanını döndürür.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Geçerli cihaz sensörlerden alt öğesini döndürür.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Genişletilmiş özellik ve geçerli cihaz için değerini döndürür.

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| PropertyName | `string` | Genişletilmiş özellik adı |

#### <a name="notifypayload"></a>Notify(Payload)

Belirtilen yük ile bir bildirim gönderir.

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| yük | `string` | bildirime eklenecek json yükü |

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

Geçerli algılayıcı üst alanını döndürür.

#### <a name="device--device"></a>Device() ⇒ `device`

Üst cihaz geçerli algılayıcı döndürür.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Genişletilmiş özellik ve geçerli algılayıcı için değerini döndürür.

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| PropertyName | `string` | Genişletilmiş özellik adı |

#### <a name="value--value"></a>Value()) ⇒ `value`

Geçerli algılayıcı değerini döndürür.

#### <a name="history--value"></a>History() ⇒ `value[]`

Geçmiş geçerli algılayıcı değerlerini döndürür.

#### <a name="notifypayload"></a>Notify(Payload)

Belirtilen yük ile bir bildirim gönderir.

| param  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| yük | `string` | bildirime eklenecek json yükü |

### <a name="value"></a>Değer

```JSON
{
  "DataType": "Temperature",
  "Value": "70",
  "CreatedTime": ""
}
```

### <a name="extended-property"></a>Genişletilmiş Özellik

```JSON
{
  "Name": "OccupancyStatus",
  "Value": "Occupied"
}
```

## <a name="next-steps"></a>Sonraki adımlar

Olayları göndermek için dijital İkizlerini uç noktaları oluşturulacağını öğrenmek için [oluşturma dijital İkizlerini uç noktaları](how-to-egress-endpoints.md).

Dijital İkizlerini uç noktaları hakkında daha fazla ayrıntı için okuma [uç noktaları hakkında daha fazla bilgi](concepts-events-routing.md).
