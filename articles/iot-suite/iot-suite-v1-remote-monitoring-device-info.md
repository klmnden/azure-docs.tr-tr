---
title: Uzaktan izleme çözümündeki cihaz bilgileri meta | Microsoft Docs
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
ms.openlocfilehash: 4efea316c05f566add3e175bc5bb18842225ede3
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35758170"
---
# <a name="device-information-metadata-in-the-remote-monitoring-preconfigured-solution"></a>Cihaz önceden yapılandırılmış Uzaktan izleme çözümünü bilgi meta veriler

Azure IOT paketi Uzaktan izleme çözümü, cihaz meta verilerini yönetmek için bir yaklaşımı gösterir. Bu makalede, bu çözüm anlamanız için için gereken bir yaklaşım özetlenmektedir:

* Çözüm hangi cihaz meta verilerini depolar.
* Nasıl bir çözümü, cihaz meta verilerini yönetir.

## <a name="context"></a>Bağlam

Uzaktan izleme önceden yapılandırılmış çözümün kullandığı [Azure IOT hub'ı] [ lnk-iot-hub] cihazlarınızı buluta veri göndermesini sağlamak. Çözüm, üç farklı konumlarda cihazlar hakkındaki bilgileri depolar:

| Konum | Depolanan bilgileri | Uygulama |
| -------- | ------------------ | -------------- |
| Kimlik kayıt defteri | Cihaz kimliği, kimlik doğrulaması anahtarları, durum etkin | IOT Hub'ına yerleşik |
| Cihaz ikizlerini | Meta veri: bildirilen özellikler, istenen özellikleri, etiketler | IOT Hub'ına yerleşik |
| Cosmos DB | Komut ve yöntem geçmişi | Özel çözüm için |

IOT hub'ı içeren bir [cihaz kimliği kayıt defteri] [ lnk-identity-registry] bir IOT hub'ı ve kullanımları erişimi yönetmek için [cihaz ikizlerini] [ lnk-device-twin] cihaz meta verilerini yönetmek için. Ayrıca bir uzaktan izleme çözümü özgü olan *cihaz kayıt defteri* , komut ve yöntem geçmişi saklar. Uzaktan izleme çözümünü kullanan bir [Cosmos DB] [ lnk-docdb] komut ve yöntem geçmişi için özel bir depo uygulamak için veritabanı.

> [!NOTE]
> Önceden yapılandırılmış Uzaktan izleme çözümüne cihaz kimliği kayıt defteri Cosmos DB veritabanında bilgi ile uyumlu kalmasını sağlar. Hem IOT hub'ınıza bağlı olan her cihazın benzersiz olarak tanımlanabilmesi için aynı cihaz kimliği kullanın.

## <a name="device-metadata"></a>Cihaz meta verileri

IOT hub'ı tutan bir [cihaz ikizi] [ lnk-device-twin] her sanal ve fiziksel cihaz için bir uzaktan izleme çözümüne bağlı. Çözüm, cihazlar ile ilişkili meta verileri yönetmek için cihaz ikizlerini kullanır. Cihaz ikizi IOT Hub tarafından tutulan bir JSON belgesidir ve çözüm, cihaz çiftleri ile etkileşim kurmak için IOT hub'ı API kullanır.

Cihaz ikizi üç tür meta verileri depolar:

- *Bildirilen özellikler* IOT hub'a bir cihaz tarafından gönderilir. Uzaktan izleme çözümünde, sanal cihazlar bildirilen özellikleri başlatma ve yanıt olarak Gönder **cihaz durumunu değiştir** komutları ve yöntemleri. Bildirilen özellikler görüntüleyebileceğiniz **cihaz listesi** ve **cihaz ayrıntıları** çözüm portalında. Bildirilen özellikler salt okunur.
- *İstenen özellikleri* cihazlar tarafından IOT hub'ından alınır. Tüm gerekli yapılandırma cihazda değişikliği yapmak için cihazın sorumluluğundadır. Ayrıca değişikliği bildirilen özellik olarak hub'a geri bildirmek için cihazın sorumluluğundadır. Çözüm Portalı aracılığıyla istenen özellik değerini ayarlayabilirsiniz.
- *Etiketleri* yalnızca cihaz ikizinde bulunan ve hiçbir zaman bir cihaz ile eşitlenir. Çözüm portalında etiket değerlerini ayarlamak ve cihaz listesini filtreleme bunları kullanın. Çözüm ayrıca çözüm portalındaki bir cihaz için görüntülenecek simge tanımlamak için bir etiket kullanır.

Örnek, sanal cihazlar özelliklerinden üreticisini, model numarası, enlem ve boylam dahil bildirdi. Sanal cihaz, bildirilen özellik olarak da desteklenen yöntemlerin listesi döndürür.

> [!NOTE]
> Sanal cihaz kodu, IoT Hub’ına geri gönderilen bildirilen özellikleri güncelleştirmek üzere yalnızca istenen **Desired.Config.TemperatureMeanValue** ve **Desired.Config.TelemetryInterval** özelliklerini kullanır. Diğer tüm istenen özellik değişiklik istekleri göz ardı edilir.

Cihaz kayıt defteri Cosmos DB veritabanında depolanan bir cihaz bilgi meta verileri JSON belgesi aşağıdaki yapıya sahiptir:

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
> Cihaz bilgilerini, IOT Hub'ına CİHAZDAN gönderilen telemetri açıklamak için meta verileri de içerebilir. Uzaktan izleme çözümü, panoyu biçimini özelleştirmek için bu telemetri meta verileri kullanır. [dinamik telemetri][lnk-dynamic-telemetry].

## <a name="lifecycle"></a>Yaşam döngüsü

Çözüm portalında bir cihaz ilk oluşturduğunuzda, çözümü komut ve yöntem geçmişi depolamak için Cosmos DB veritabanında bir giriş oluşturur. Bu noktada, çözüm ayrıca cihaz için bir giriş cihazı IOT Hub ile kimlik doğrulaması için kullandığı anahtarları oluşturan cihaz kimliği kayıt defterinde oluşturur. Ayrıca, cihaz ikizi oluşturur.

Bir cihaz çözüme ilk kez bağlandığında, bildirilen özellikleri ve bir cihaz bilgi iletisi gönderir. Bildirilen özellik değerleri, cihaz ikizinde otomatik olarak kaydedilir. Bildirilen özellikler, cihaz üreticisi, model numarası, seri numarası ve desteklenen yöntemlerin listesini içerir. Komut parametreleri hakkında bilgiler dahil olmak üzere, cihazın desteklediği komutların listesini cihaz bilgi iletisini içerir. Çözüm bu ileti aldığında, Cosmos DB veritabanındaki cihaz bilgilerini güncelleştirir.

### <a name="view-and-edit-device-information-in-the-solution-portal"></a>Çözüm portalında cihaz bilgilerini görüntüleyin ve düzenleyin

Çözüm portalında cihaz listesini aşağıdaki cihaz özelliklerini sütunlar halinde varsayılan olarak görüntüler: **durumu**, **DeviceID**, **üretici**, **modeli Sayı**, **seri numarası**, **bellenim**, **Platform**, **İşlemci**, ve  **Yüklü RAM**. Tıklayarak sütunları özelleştirebilirsiniz **sütun Düzenleyicisi**. Cihaz özellikleri **enlem** ve **boylam** konumu Bing Haritası'nda Panoda sürücü.

![Cihaz listesinde sütun Düzenleyicisi][img-device-list]

İçinde **cihaz ayrıntıları** bölmesinde çözüm portalında istenen özellikleri ve etiketleri düzenleyebilirsiniz (bildirilen özellikler yalnızca okunur).

![Cihaz ayrıntıları bölmesi][img-device-edit]

Çözümünüze ait bir cihazı kaldırmak için çözüm portalı kullanabilirsiniz. Bir cihazı kaldırmak, çözüm cihaz girişi kimlik kayıt defterinden kaldırır ve ardından cihaz ikizinde siler. Çözüm, Cosmos DB veritabanı cihazla ilgili bilgileri de kaldırır. Bir cihaz kaldırabilmeniz için önce devre dışı bırakmalısınız.

![Cihazı Kaldır][img-device-remove]

## <a name="device-information-message-processing"></a>Cihaz bilgi iletisi işleme

Bir cihaz tarafından gönderilen cihaz bilgileri iletilerini telemetri iletilerini farklıdır. Cihaz bilgileri iletilerini, bir cihazın yanıt verebildiği komutların ve herhangi bir komut geçmişi içerir. Kendisini IOT hub'a bir cihaz bilgi iletisini içinde bulunan meta veriler hakkında bilgiye sahip ve iletiyi herhangi bir CİHAZDAN buluta ileti işler aynı şekilde işler. Uzaktan izleme çözümünde bir [Azure Stream Analytics] [ lnk-stream-analytics] (ASA) işini iletileri IOT hub'dan okur. **Deviceınfo** stream analytics iş filtreleri içeren iletileri için **"ObjectType": "Deviceınfo"** ve bunlara iletir **EventProcessorHost** ana bilgisayar örneği bir web işi çalıştırır. Mantık **EventProcessorHost** örneği söz konusu cihaz için Cosmos DB kaydı bulun ve kaydı güncelleştirmek için cihaz Kimliğini kullanır.

> [!NOTE]
> Bir cihaz bilgi iletisini standart bir CİHAZDAN buluta ileti ' dir. Çözüm, ASA sorguları kullanarak cihaz bilgileri iletilerini ve telemetri iletilerini arasında ayırır.

## <a name="next-steps"></a>Sonraki adımlar

Önceden yapılandırılmış çözümleri nasıl özelleştirebileceğiniz öğrenme bitirdikten sonra bazı diğer özelliklerini ve yeteneklerini IOT paketi önceden yapılandırılmış çözümleri keşfedin:

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
[lnk-security-groundup]:/azure/iot-fundamentals/iot-security-ground-up
