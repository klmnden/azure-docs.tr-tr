---
title: Uzaktan izleme çözümü cihaz bilgileri meta verilerde | Microsoft Docs
description: Azure IOT önceden yapılandırılmış çözümü uzaktan izlemenin ve mimarisinin açıklaması.
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 1b334769-103b-4eb0-a293-184f3d1ba9a3
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 80f03a4cef1d79e819c59ca68a786776a5c4edb7
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34636105"
---
# <a name="device-information-metadata-in-the-remote-monitoring-preconfigured-solution"></a>Önceden yapılandırılmış Uzaktan izleme çözümü cihaz bilgileri meta veriler

Azure IOT paketi Uzaktan izleme çözümü cihaz meta verilerini yönetmek için bir yaklaşım gösterir. Bu makalede, bu çözüm anlamak etkinleştirmeniz için gereken bir yaklaşım özetlenmektedir:

* Çözüm hangi cihaz meta verilerini depolar.
* Nasıl çözüm cihaz meta verilerini yönetir.

## <a name="context"></a>Bağlam

Çözüm kullanan önceden yapılandırılmış Uzaktan izleme [Azure IOT Hub] [ lnk-iot-hub] aygıtlarınızı buluta veri göndermesini sağlamak için. Çözümü üç farklı konumlarda cihazlarla ilgili bilgileri depolar:

| Konum | Depolanan bilgileri | Uygulama |
| -------- | ------------------ | -------------- |
| Kimlik kayıt defteri | Cihaz kimliği, kimlik doğrulaması anahtarları, durumu etkin | IOT Hub'ına yerleşik |
| Cihaz çiftlerini | Meta veriler: bildirilen özellikleri, istenen özellikleri, etiketler | IOT Hub'ına yerleşik |
| Cosmos DB | Komut ve yöntemi geçmişi | Özel çözüm için |

IOT hub'ında bir [cihaz kimlik kayıt defteri] [ lnk-identity-registry] IOT hub'ı ve kullanım erişimini yönetmek için [cihaz çiftlerini] [ lnk-device-twin] cihaz meta verilerini yönetmek için. Ayrıca bir uzaktan izleme çözümü özgü olan *cihaz kayıt defteri* komutu ve yöntemi geçmişini saklar. Uzaktan izleme çözümü kullanan bir [Cosmos DB] [ lnk-docdb] komut ve yöntemi geçmişi için özel bir depo uygulamak için veritabanı.

> [!NOTE]
> Önceden yapılandırılmış Uzaktan izleme çözümü cihaz kimlik kayıt defteri Cosmos DB veritabanında bilgilerle eşitlenmiş tutar. Her ikisi de aynı cihaz kimliği IOT hub'ına bağlı her cihazın benzersiz şekilde tanımlamak için kullanın.

## <a name="device-metadata"></a>Cihaz meta veriler

IOT hub'ı tutan bir [cihaz çifti] [ lnk-device-twin] her sanal ve fiziksel cihaz için bir uzaktan izleme çözümüne bağlı. Çözüm, aygıtlar ile ilişkili meta verileri yönetmek için cihaz çiftlerini kullanır. Cihaz çifti IOT Hub tarafından korunan bir JSON belgesinin ve çözüm cihaz çiftlerini ile etkileşim kurmak için IOT Hub API kullanır.

Cihaz çifti üç tür meta verileri depolar:

- *Özellikler bildirilen* IOT hub'a bir cihaz tarafından gönderilir. Uzaktan izleme çözümünde başlatma ve yanıt olarak bildirilen özellikleri sanal cihazlar Gönder **cihaz durumunu değiştir** komutlar ve yöntemleri. Bildirilen özelliklerinde görüntüleyebilirsiniz **cihaz listesi** ve **cihaz ayrıntıları** çözüm Portalı'nda. Bildirilen özellikleri salt okunurdur.
- *Özellikler istenen* IOT hub'ından cihazlar tarafından alınır. Tüm gerekli yapılandırma cihazda değişikliği yapmak için cihaz sorumluluğundadır. Bu ayrıca değişikliği geri hub'ı bildirilen bir özellik olarak rapor aygıta sorumluluğundadır. İstenen özellik değeri çözüm Portalı aracılığıyla ayarlayabilirsiniz.
- *Etiketler* yalnızca cihaz çiftine mevcut ve hiçbir zaman bir aygıt ile eşitlenir. Çözüm portalında etiket değerleri ayarlamak ve cihaz listesini filtre bunları kullanın. Çözüm bir etiket çözüm portalında bir aygıt için görüntülemek için bu simgeyi tanımlamak için de kullanır.

Sanal cihazlar özelliklerinden üreticisini, model numarası, enlem ve boylam dahil örnek bildirdi. Sanal cihazlar, ayrıca bildirilen bir özellik olarak desteklenen yöntemlerin listesi döndürür.

> [!NOTE]
> Sanal cihaz kodu, IoT Hub’ına geri gönderilen bildirilen özellikleri güncelleştirmek üzere yalnızca istenen **Desired.Config.TemperatureMeanValue** ve **Desired.Config.TelemetryInterval** özelliklerini kullanır. Diğer tüm istenen özelliği değişiklik isteklerini göz ardı edilir.

Cihaz kayıt defteri Cosmos DB veritabanında depolanan bir aygıt bilgileri meta verileri JSON belgesi aşağıdaki yapıya sahiptir:

```json
{
  "DeviceProperties": {
    "DeviceID": "deviceid1",
    "HubEnabledState": null,
    "CreatedTime": "2016-04-25T23:54:01.313802Z",
    "DeviceState": "normal",
    "UpdatedTime": null
    },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [],
  "CommandHistory": [],
  "IsSimulatedDevice": false,
  "id": "fe81a81c-bcbc-4970-81f4-7f12f2d8bda8"
}
```

> [!NOTE]
> Aygıt bilgileri aygıt IOT Hub'ına gönderir telemetri açıklamak için meta verileri de içerir. Uzaktan izleme çözümü, Pano biçimini özelleştirmek için bu telemetri meta veri kullanan [dinamik telemetri][lnk-dynamic-telemetry].

## <a name="lifecycle"></a>Yaşam döngüsü

Çözüm Portalı'nda bir cihaz ilk oluşturduğunuzda, çözüm komut ve yöntemi geçmişini depolamak için Cosmos DB veritabanında bir giriş oluşturur. Bu noktada, çözüm Ayrıca aygıtı için bir giriş aygıtın IOT Hub ile kimlik doğrulaması için kullandığı anahtarları oluşturur cihaz kimliği kayıt oluşturur. Ayrıca, bir cihaz çifti oluşturur.

Bir cihaz ilk çözüme bağlandığında, bildirilen özellikleri ve bir cihaz bilgi iletisi gönderir. Bildirilen özellik değerlerini otomatik olarak cihaz çiftine kaydedilir. Bildirilen özellikleri aygıt üreticisi, model numarası, seri numarası ve desteklenen yöntemlerin listesi içerir. Cihaz bilgi iletisi komut parametreleri hakkında bilgiler dahil olmak üzere cihaz destekler komutların listesini içerir. Bu iletiyi çözümünün aldığında, aygıt bilgileri Cosmos DB veritabanında güncelleştirir.

### <a name="view-and-edit-device-information-in-the-solution-portal"></a>Çözüm portalında aygıt bilgileri görüntüleyin ve düzenleyin

Çözüm portalı aygıt listesinde aşağıdaki cihaz özelliklerini sütunları olarak varsayılan olarak görüntüler: **durum**, **DeviceID**, **üretici**, **modeli Sayı**, **seri numarası**, **bellenim**, **Platform**, **İşlemci**, ve  **RAM yüklü**. Tıklayarak sütunları özelleştirebilirsiniz **sütun düzenleyicisini**. Cihaz özellikleri **enlem** ve **boylam** Bing harita konumda Panoda sürücü.

![Aygıt listesinde sütun Düzenleyicisi][img-device-list]

İçinde **cihaz ayrıntıları** bölmesi çözüm Portalı'nda, istenen özelliklerini ve etiketleri düzenleyebilirsiniz (özellikleri salt okunur bildirilen).

![Cihaz ayrıntıları bölmesi][img-device-edit]

Bir aygıtı çözümünüzden kaldırmak için çözüm Portalı'nı kullanabilirsiniz. Bir aygıt kaldırdığınızda, çözüm aygıt girişi kimlik kayıt defterinden kaldırır ve cihaz çifti siler. Çözüm Cosmos DB veritabanından cihaza ilgili bilgileri de kaldırır. Bir aygıt kaldırabilmeniz için önce devre dışı bırakmalısınız.

![Aygıt kaldırma][img-device-remove]

## <a name="device-information-message-processing"></a>Cihaz bilgi iletisi işleme

Bir aygıt tarafından gönderilen cihaz bilgileri iletilerini telemetri iletilerini farklıdır. Cihaz bilgileri iletilerini bir aygıtı yanıt verebileceği komutları ve komut geçmişini içerir. IOT Hub kendisini bir cihaz bilgi iletisi bulunan meta veriler olanağıyla sahiptir ve iletiyi herhangi bir cihaz bulut iletisini işler aynı şekilde işler. Uzaktan izleme çözümünde bir [Azure akış analizi] [ lnk-stream-analytics] (ASA) işini IOT Hub'ından iletileri okur. **Deviceınfo** stream analytics iş filtreleri içeren iletileri için **"ObjectType": "Deviceınfo"** ve bunlara iletir **EventProcessorHost** ana bilgisayar örneği bir web işi çalıştırır. Mantık **EventProcessorHost** örneği için belirli bir aygıtı Cosmos DB kayıt bulmak ve kaydı güncelleştirmek için cihaz kimliği kullanır.

> [!NOTE]
> Cihaz bilgi iletisi, bir standart cihaz bulut iletisidir. Çözüm, ASA sorguları kullanarak cihaz bilgileri iletilerini ve telemetri iletilerini arasında ayırır.

## <a name="next-steps"></a>Sonraki adımlar

Önceden yapılandırılmış çözümler nasıl özelleştirebileceğiniz öğrenme bitirdikten sonra artık, bazı diğer özellikler ve yetenekler IOT paketi önceden yapılandırılmış çözümleri gözden geçirebilirsiniz:

* [Önceden yapılandırılmış Tahmine dayalı bakım çözümüne genel bakış][lnk-predictive-overview]
* [IoT Paketi için sık sorulan sorular][lnk-faq]
* [Baştan sona IoT güvenliği][lnk-security-groundup]

<!-- Images and links -->
[img-device-list]: media/iot-suite-v1-remote-monitoring-device-info/image1.png
[img-device-edit]: media/iot-suite-v1-remote-monitoring-device-info/image2.png
[img-device-remove]: media/iot-suite-v1-remote-monitoring-device-info/image3.png

[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-docdb]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-dynamic-telemetry]: iot-suite-v1-dynamic-telemetry.md

[lnk-predictive-overview]:../iot-accelerators/iot-accelerators-predictive-overview.md
[lnk-faq]: iot-suite-v1-faq.md
[lnk-security-groundup]:../iot-accelerators/securing-iot-ground-up.md
