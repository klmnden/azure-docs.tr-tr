---
title: Azure dijital İkizlerini kullanıcı tanımlı işlevleri istemci Kitaplığı Başvurusu | Microsoft Docs
description: İstemci Kitaplığı Başvurusu Azure dijital İkizlerini kullanıcı tanımlı işlevleri.
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: article
ms.date: 06/06/2019
ms.author: alinast
ms.custom: seodec18
ms.openlocfilehash: be05cec8e3d755f1b04e5ecc5ec7c740053a74d4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67073047"
---
# <a name="user-defined-functions-client-library-reference"></a>Kullanıcı tanımlı işlevleri istemci Kitaplığı Başvurusu

Bu belge, Azure dijital İkizlerini kullanıcı tanımlı işlevleri istemci kitaplığı için başvuru bilgileri sağlar.

## <a name="helper-methods"></a>Yardımcı yöntemler

İstemci Kitaplığı, yaygın olarak kullanılan işlemleri için yardımcı yöntemleri tanımlar.

### <a name="getspacemetadataid--space"></a>getSpaceMetadata(id) ⇒ `space`

Bir alanı tanımlayıcısı göz önünde bulundurulduğunda, bu işlevi grafikten yer alır.

**Tür**: genel işlevi

| Parametre  | Tür                | Açıklama  |
| ---------- | ------------------- | ------------ |
| *id*  | `guid` | alanı tanımlayıcısı |

### <a name="getsensormetadataid--sensor"></a>getSensorMetadata(id) ⇒ `sensor`

Bir algılayıcı tanımlayıcı göz önünde bulundurulduğunda, bu işlev grafikten algılayıcı alır.

**Tür**: genel işlevi

| Parametre  | Tür                | Açıklama  |
| ---------- | ------------------- | ------------ |
| *id*  | `guid` | Algılayıcı tanımlayıcısı |

### <a name="getdevicemetadataid--device"></a>getDeviceMetadata(id) ⇒ `device`

Cihaz tanımlayıcısı göz önünde bulundurulduğunda, bu işlevi cihaz grafikten alır.

**Tür**: genel işlevi

| Parametre  | Tür                | Açıklama  |
| ------ | ------------------- | ------------ |
| *id* | `guid` | Cihaz tanımlayıcısı |

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

İstemci referans yardımcı yöntemlerinden döndürülen yanıt modelleri aşağıda açıklanmıştır.

### <a name="space"></a>Uzay

```JSON
{
  "id": "00000000-0000-0000-0000-000000000000",
  "name": "Space",
  "friendlyName": "Conference Room",
  "typeId": 0,
  "parentSpaceId": "00000000-0000-0000-0000-000000000001",
  "subtypeId": 0
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
  "id": "00000000-0000-0000-0000-000000000002",
  "name": "Device",
  "friendlyName": "Temperature Sensing Device",
  "description": "This device contains a sensor that captures temperature readings.",
  "type": "None",
  "subtype": "None",
  "typeId": 0,
  "subtypeId": 0,
  "hardwareId": "ABC123",
  "gatewayId": "ABC",
  "spaceId": "00000000-0000-0000-0000-000000000000"
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
  "id": "00000000-0000-0000-0000-000000000003",
  "port": "30",
  "pollRate": 3600,
  "dataType": "Temperature",
  "dataSubtype": "None",
  "type": "Classic",
  "portType": "None",
  "dataUnitType": "FahrenheitTemperature",
  "spaceId": "00000000-0000-0000-0000-000000000000",
  "deviceId": "00000000-0000-0000-0000-000000000001",
  "portTypeId": 0,
  "dataUnitTypeId": 0,
  "dataTypeId": 0,
  "dataSubtypeId": 0,
  "typeId": 0  
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
  "dataType": "Temperature",
  "value": "70",
  "createdTime": ""
}
```

### <a name="extended-property"></a>Genişletilmiş özellik

```JSON
{
  "name": "OccupancyStatus",
  "value": "Occupied"
}
```

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [Azure dijital İkizlerini kullanıcı tanımlı işlevleri](./concepts-user-defined-functions.md).

- Bilgi [kullanıcı tanımlı işlevler oluşturma](./how-to-user-defined-functions.md).

- Bilgi [kullanıcı tanımlı işlevleri hata ayıklama](./how-to-diagnose-user-defined-functions.md).
