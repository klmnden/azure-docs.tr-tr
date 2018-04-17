---
title: Iothub explorer ile Azure IOT cihaz Yönetimi | Microsoft Docs
description: Iothub explorer CLI aracı doğrudan yöntemleri ve Twin'ın istenen özellikleri yönetim seçenekleri özelliklerine sahip Azure IOT Hub cihaz yönetimi için kullanın.
services: iot-hub
documentationcenter: ''
author: rangv
manager: timlt
tags: ''
keywords: Azure IOT cihaz yönetimi, azure IOT hub cihaz yönetimi, cihaz yönetim IOT, IOT hub cihaz Yönetimi
ms.assetid: b34f799a-fc14-41b9-bf45-54751163fffe
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/11/2018
ms.author: rangv
ms.openlocfilehash: 26e08c3d6b1c96e2d508c87f188118aec02bab6a
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="use-iothub-explorer-for-azure-iot-hub-device-management"></a>Azure IOT Hub cihaz yönetimi için ıothub explorer'ı kullanın

![Uçtan uca diyagramı](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[ıothub explorer](https://github.com/azure/iothub-explorer) bir ana bilgisayarda çalıştırmak bir CLI araçtır, IOT hub defterinde cihaz kimlikleri yönetmek için bilgisayar. Çeşitli görevleri gerçekleştirmek için kullanabileceğiniz yönetim seçenekleri ile gelir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

| Yönetim seçeneği          | Görev                                                                                                                            |
|----------------------------|------------------------------------------------------------------------------------------------------------------------------|
| Doğrudan yöntemler             | Başlatma veya ileti gönderme veya cihaz yeniden başlatıldığı durdurma gibi davranacak bir aygıtı olun.                                        |
| İstenen twin özellikleri    | Bir aygıt için yeşil bir Işığı ayarlama veya telemetri gönderme aralığı 30 dakika gibi bazı durumların yerleştirin.         |
| Özellikler Twin bildirdi   | Bir aygıt bildirilen durumunu alın. Örneğin, aygıt LED şimdi yanıp sönen bildirir.                                    |
| Twin etiketleri                  | Aygıta özgü meta verileri bulutta depolayın. Örneğin, bir satış makinenin dağıtım konumu.                         |
| Bulut-cihaz iletilerini   | Bir aygıt için bildirimler Gönder. Örneğin, ", Bugün sizi büyük olasılıktır. Bir şemsiyesi getirmeyi unutmayın."              |
| Cihaz çifti sorguları        | Kullanılabilir aygıtları tanımlama gibi rasgele koşullarla almak için tüm cihaz çiftlerini sorgulamak. |

Daha ayrıntılı açıklama farklara ve bu seçenekleri kullanma konusunda yönergeler için bkz: [cihaz bulut iletişimi Kılavuzu](iot-hub-devguide-d2c-guidance.md) ve [bulut-cihaz iletişimi Kılavuzu](iot-hub-devguide-c2d-guidance.md).

Cihaz çiftleri, cihaz durumu bilgilerini (meta veriler, yapılandırmalar ve koşullar) depolayan JSON belgelerdir. IOT Hub cihaz çifti ona bağlanan her aygıt için devam ettirir. Cihaz çiftlerini hakkında daha fazla bilgi için bkz: [cihaz çiftlerini ile çalışmaya başlama](iot-hub-node-node-twin-getstarted.md).

## <a name="what-you-learn"></a>Öğrenecekleriniz

Geliştirme makinenizde çeşitli yönetim seçenekleri ile ıothub explorer'ı kullanarak öğrenin.

## <a name="what-you-do"></a>Neler

Iothub explorer çeşitli yönetim seçenekleri ile çalıştırın.

## <a name="what-you-need"></a>Ne gerekiyor

- Öğretici [Cihazınızı](iot-hub-raspberry-pi-kit-node-get-started.md) , aşağıdaki gereksinimleri ele alınmaktadır tamamlandı:
- Etkin bir Azure aboneliği.
- Azure IOT hub'ı aboneliğinizdeki.
- Azure IOT hub'ına iletileri gönderen bir istemci uygulaması.
- Cihazınız Bu öğretici sırasında istemci uygulaması ile çalıştığından emin olun.
- iothub-explorer [yükleme ıothub explorer](https://github.com/azure/iothub-explorer) geliştirme makinenizde.

## <a name="connect-to-your-iot-hub"></a>IOT hub'ınıza bağlanın

Aşağıdaki komutu çalıştırarak IOT hub'ınıza bağlanın:

```bash
iothub-explorer login <your IoT hub connection string>
```

## <a name="use-iothub-explorer-with-direct-methods"></a>Doğrudan yöntemleriyle ıothub Gezgini'ni kullanın

Çağırma `start` yöntemi aşağıdaki komutu çalıştırarak IOT hub'ınıza ileti göndermek için aygıt uygulamasında:

```bash
iothub-explorer device-method <your device Id> start
```

Çağırma `stop` gönderilmesini durdurmak için cihaz uygulaması yönteminde aşağıdaki komutu çalıştırarak IOT hub'ına iletileri:

```bash
iothub-explorer device-method <your device Id> stop
```

## <a name="use-iothub-explorer-with-twins-desired-properties"></a>Twin'ın istenen özelliklere sahip ıothub Gezgini'ni kullanın

İstenen özellik aralığını ayarlayın aşağıdaki komutu çalıştırarak 3000 =:

```bash
iothub-explorer update-twin <your device id> {\"properties\":{\"desired\":{\"interval\":3000}}}
```

Bu özellik, cihazınız tarafından okunabilir.

## <a name="use-iothub-explorer-with-twins-reported-properties"></a>Twin'ın bildirilen özelliklerle ıothub Gezgini'ni kullanın

Aşağıdaki komutu çalıştırarak bildirilen cihaz özelliklerini alın:

```bash
iothub-explorer get-twin <your device id>
```

$Metadata özellikleri biridir. Bu aygıtın son zamanı gösteren $lastUpdated gönderir veya bir ileti alır.

## <a name="use-iothub-explorer-with-twins-tags"></a>Twin'ın etiketleriyle ıothub Gezgini'ni kullanın

Aşağıdaki komutu çalıştırarak etiketleri ve cihaz özelliklerini görüntüle:

```bash
iothub-explorer get-twin <your device id>
```

Bir alan rolünü ekleyin aşağıdaki komutu çalıştırarak sıcaklık ve nem aygıta =:

```bash
iothub-explorer update-twin <your device id> "{\"tags\":{\"role\":\"temperature&humidity\"}}"
```

## <a name="use-iothub-explorer-with-cloud-to-device-messages"></a>Bulut cihaza iletileriyle ıothub Gezgini'ni kullanın

"Hello World" iletisini, aşağıdaki komutu çalıştırarak cihaza gönder:

```bash
iothub-explorer send <device-id> "Hello World"
```

Bkz: [cihaz ve IOT hub'ı arasında ileti gönderme ve alma için iothub-Gezgini'ni kullanma](iot-hub-explorer-cloud-device-messaging.md) bu komutu kullanarak, gerçek bir senaryo için.

## <a name="use-iothub-explorer-with-device-twins-queries"></a>Cihaz çiftlerini sorgularıyla ıothub Gezgini'ni kullanın

Sorgu bir etiket rolünün aygıtlarla 'sıcaklık ve nem' aşağıdaki komutu çalıştırarak =:

```bash
iothub-explorer query-twin "SELECT * FROM devices WHERE tags.role = 'temperature&humidity'"
```

Bir etiketi rolünün olanlar dışında tüm cihazlar sorgu aşağıdaki komutu çalıştırarak 'sıcaklık ve nem' =:

```bash
iothub-explorer query-twin "SELECT * FROM devices WHERE tags.role != 'temperature&humidity'"
```

## <a name="next-steps"></a>Sonraki adımlar

Çeşitli yönetim seçenekleri ile ıothub Gezgini'ni kullanma öğrendiniz.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
