---
title: IOT Hub için Azure olay kılavuz şema | Microsoft Docs
description: IOT Hub'ının özelliklerini ve olay şema biçimi için başvuru sayfası
services: iot-hub
documentationcenter: ''
author: kgremban
manager: timlt
editor: ''
ms.service: event-grid
ms.topic: reference
ms.date: 01/30/2018
ms.author: kgremban
ms.openlocfilehash: 812ca3ba546112f54a76319fda853d441ce34f1b
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="azure-event-grid-event-schema-for-iot-hub"></a>IOT Hub için Azure olay kılavuz şeması

Bu makale, Azure IOT Hub olaylar için şema ve özellikleri sağlar. Olay şemaları giriş için bkz: [Azure olay kılavuz olay şema](event-schema.md). 

## <a name="available-event-types"></a>Kullanılabilir olay türleri

Azure IOT Hub aşağıdaki olay türlerini gösterir:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.Devices.DeviceCreated | IOT hub'a bir cihaz kaydedildiğinde yayımladı. |
| Microsoft.Devices.DeviceDeleted | Bir cihaz IOT hub'ından silindiğinde yayımladı. | 

## <a name="example-event"></a>Örnek olayı

Şemanın DeviceCreated ve DeviceDeleted olayları için aynı yapıya sahip. Bu örnek olay IOT hub'a bir cihaz kaydedildiğinde gerçekleşen bir olay şeması gösterir:

```json
[{
  "id": "56afc886-767b-d359-d59e-0da7877166b2",
  "topic": "/SUBSCRIPTIONS/<subscription ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<hub name>",
  "subject": "devices/LogicAppTestDevice",
  "eventType": "Microsoft.Devices.DeviceCreated",
  "eventTime": "2018-01-02T19:17:44.4383997Z",
  "data": {
    "twin": {
      "deviceId": "LogicAppTestDevice",
      "etag": "AAAAAAAAAAE=",
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
      "properties": {
        "desired": {
          "$metadata": {
            "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
          },
          "$version": 1
        },
        "reported": {
          "$metadata": {
            "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
          },
          "$version": 1
        }
      }
    },
    "hubName": "egtesthub1",
    "deviceId": "LogicAppTestDevice",
    "operationTimestamp": "2018-01-02T19:17:44.4383997Z",
    "opType": "DeviceCreated"
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="event-properties"></a>Olay Özellikleri

Tüm olaylar aynı üst düzey veri içerir: 

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| id | dize | Olay için benzersiz tanımlayıcı. |
| Konu | dize | Olay kaynağı tam kaynak yolu. Bu alan yazılabilir değil. Bu değer olay kılavuz sağlar. |
| Konu | dize | Olay konu yayımcı tarafından tanımlanan yolu. |
| Olay türü | dize | Bu olay kaynağı için kayıtlı olay türünden biri. |
| EventTime | dize | Olayı oluşturan zaman sağlayıcının UTC zamanı temel alınarak. |
| veriler | object | IOT hub'ı olay verileri.  |
| dataVersion | dize | Veri nesnesinin şema sürümü. Yayımcı şema sürümü tanımlar. |
| metadataVersion | dize | Olay meta verilerinin şema sürümü. Olay kılavuz, şemanın en üst düzey özellikleri tanımlar. Bu değer olay kılavuz sağlar. |

Veri nesnesi içeriğini, her olay yayımcısı için farklıdır. IOT hub'ı olaylar için veri nesnesi aşağıdaki özellikleri içerir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| hubName | dize | IOT cihaz burada oluşturulduğu veya silindiği Hub adı. |
| deviceId | dize | Cihazın benzersiz tanımlayıcısı. Bu büyük küçük harf duyarlı dize en fazla 128 karakter uzunluğunda olabilir ve ASCII 7 bit alfasayısal karakterlerden ve şu özel karakterleri destekler: `- : . + % _ # * ? ! ( ) , = @ ; $ '`. |
| operationTimestamp | dize | İşlemi ISO8601 zaman damgası. |
| opType | dize | Bu işlem için IOT Hub tarafından belirtilen olay türü: ya da `DeviceCreated` veya `DeviceDeleted`.
| Twin | object | Uygulama cihaz meta verilerini, bulut represenation olan cihaz çifti hakkında bilgi sağlar. | 
| cihaz kimliği | dize | Cihaz çifti benzersiz tanımlayıcısı. | 
| ETag | dize | Cihaz çifti içeriğini açıklayan bilgileri parçası. Her etag cihaz çifti benzersiz olması garanti edilmez. | 
| durum | dize | Olup cihaz çifti etkin veya devre dışı. | 
| statusUpdateTime | dize | Son cihaz çifti durumu ISO8601 damgası güncelleştirin. |
| connectionState | dize | Olup aygıt bağlı veya kesilen. | 
| lastActivityTime | dize | Son Etkinlik ISO8601 zaman damgası. | 
| cloudToDeviceMessageCount | integer | Bu cihaz için gönderilen cihaz iletileri buluta sayısı. | 
| authenticationType | dize | Bu aygıt için kullanılan kimlik doğrulama türü: ya da `SAS`, `SelfSigned`, veya `CertificateAuthority`. |
| X509Thumbprint | dize | Parmak izi x509 için benzersiz bir değerdir belirli bir sertifikanın sertifika deposunda bulmak için yaygın olarak kullanılan sertifika. Parmak izi SHA1 algoritması kullanılarak dinamik olarak oluşturulur ve fiziksel olarak sertifika yok. | 
| primaryThumbprint | dize | X509 birincil parmak izi sertifika. |
| secondaryThumbprint | dize | X509 ikincil parmak izi sertifika. | 
| sürüm | integer | Bir her artırılır tamsayı zaman cihaz çifti güncelleştirilir. |
| İstenen | object | Yalnızca uygulama arka uç tarafından yazılır ve cihaz tarafından okunur özelliklerin bir kısmı. | 
| bildirilen | object | Yalnızca aygıt tarafından yazılır ve uygulama arka ucu tarafından okunur özellikler bölümü. |
| lastUpdated | dize | ISO8601 zaman damgası son cihaz çifti özelliğinin güncelleştirin. | 

## <a name="next-steps"></a>Sonraki adımlar

* Azure olay kılavuz giriş için bkz: [olay kılavuz nedir?](overview.md)
* IOT Hub ve olay kılavuz birlikte nasıl çalıştığını hakkında bilgi edinmek için [tetikleyici eylemleri olay kılavuz kullanarak IOT hub'ı olaylarına tepki](../iot-hub/iot-hub-event-grid.md).