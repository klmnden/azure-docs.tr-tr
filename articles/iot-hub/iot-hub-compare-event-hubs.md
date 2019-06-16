---
title: Azure IOT hub'ı Azure Event hubs'a karşılaştırın | Microsoft Docs
description: İşlevsel farklılıklar ve kullanım örneklerini vurgulama IOT Hub ve Event hubs'ı Azure Hizmetleri karşılaştırması. Desteklenen protokoller, cihaz yönetimi, izleme, karşılaştırma içerir ve dosyayı yükler.
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: kgremban
ms.openlocfilehash: 7a589ba80b61ea5ef9ea1c941e9a0218a1653c99
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60735536"
---
# <a name="connecting-iot-devices-to-azure-iot-hub-and-event-hubs"></a>IOT cihazları Azure'a bağlanıyor: IOT Hub ile Event Hubs

Azure, farklı bağlantı ve verileriniz için bulutun tüm gücü bağlanmanıza yardımcı olması için iletişim türleri için özel olarak geliştirilen hizmetleri sağlar. Azure IOT Hub hem de Azure Event Hubs, büyük miktarlarda veri ve işlem alma veya iş öngörülerine ilişkin verileri depolamak bulut hizmetleridir. Her ikisi de düşük gecikme süresi ve yüksek güvenilirlikle veri alımını destekler, iki hizmet benzerdir, ancak bunlar farklı amaçlar için tasarlanmıştır. IOT Hub, IOT cihazları Azure bulutuna bağlayarak, Event Hubs, büyük veri akışı için tasarlanmıştır ancak benzersiz gereksinimlerini karşılamak için geliştirilmiştir. Microsoft Azure IOT cihazları bağlanmak için Azure IOT hub'ı kullanmanızı önerir

Azure IOT hub'a bağlanan veri ve sürücü iş öngörülerini ve Otomasyon toplamak üzere IOT cihazları bulut ağ geçidi ' dir. Ayrıca, IOT Hub, cihazlarınız ve arka uç sistemlerinizi arasındaki ilişkiyi zenginleştirin özellikleri içerir. Çift yönlü iletişimi özellikleri veri alırken cihazlardan komutları da gönderebilir ve ilkeleri cihazlara geri anlamına gelir. Örneğin, özellikleri güncelleştirmek veya cihaz yönetim eylemleri çağırmak için bulut-cihaz Mesajlaşma kullanın. Bulut-cihaz iletişimi bulut zekasını uç cihazlarınıza Azure IOT Edge ile göndermenize olanak sağlar. Benzersiz cihaz düzeyinde kimlik IOT hub'ı sağlanan yardımcı olur göre daha iyi güvenli IOT çözümünüzden olası saldırıları. 

[Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) akış hizmeti Azure büyük veri. Günlük milyarlarca müşterilerin nerede gönderebilir yüksek aktarım hızı veri akış senaryoları için tasarlanmıştır. Event Hubs, akışınız ölçeklendirmek için bölümlenmiş tüketici modelinin kullanır ve büyük veri ve Azure Databricks, Stream Analytics, ADLS ve HDInsight gibi Analiz Hizmetleri tümleştirilmiştir. Event Hubs yakalama ve otomatik Şişme gibi özelliklerle, bu hizmet, büyük veri uygulamaları ve çözümleri desteklemek için tasarlanmıştır. Ayrıca, böylece IOT çözümünüz hem de Event Hubs'ın Bilim insanları için inanılmaz gücü avantajlardan IOT Hub Event Hubs telemetri akış yolu kullanır.

Özetlemek gerekirse, her iki çözüm de büyük bir ölçekte veri alımı için tasarlanmıştır. Yalnızca IOT Hub, IOT cihazlarınızı Azure bulutuna bağlayarak iş değerini en üst düzeye çıkarmak tasarlanmış zengin bir IOT özgü özellikler sunar.  Veri alımı senaryolarınızı desteklemek için IOT Hub ile başlayarak, IOT yolculuğunuza yeni başlıyor, iş ve teknik gereksinimlerinizi şart sonra tam özellikli IOT özelliklere anında erişim sahip sağlanması.

Aşağıdaki tabloda, IOT özelliklerini değerlendirirken nasıl IOT Hub'ın iki katmanda Event Hubs karşılaştırması hakkında ayrıntılar sağlar. IOT Hub'ın standart ve temel katmanları hakkında daha fazla bilgi için bkz. [doğru IOT Hub katmanını seçme](iot-hub-scaling.md).

| IOT özelliği | IOT hub'ı standart katman | IOT hub'ı temel katman | Event Hubs |
| --- | --- | --- | --- |
| CİHAZDAN buluta ileti gönderme | ![Onay][checkmark] | ![Onay][checkmark] | ![Onay][checkmark] |
| Protokoller: HTTPS, AMQP, AMQP webSockets üzerinden | ![Onay][checkmark] | ![Onay][checkmark] | ![Onay][checkmark] |
| Protokoller: MQTT, MQTT webSockets üzerinden | ![Onay][checkmark] | ![Onay][checkmark] |  |
| Cihaz kimliği başına | ![Onay][checkmark] | ![Onay][checkmark] |  |
| Cihazlardan karşıya dosya yükleme | ![Onay][checkmark] | ![Onay][checkmark] |  |
| Cihaz sağlama hizmeti | ![Onay][checkmark] | ![Onay][checkmark] |  |
| Buluttan cihaza ileti gönderme | ![Onay][checkmark] |  |  |
| Cihaz ikizi ve cihaz Yönetimi | ![Onay][checkmark] |  |  |
| Cihaz akışları (Önizleme) | ![Onay][checkmark] |  |  |
| IoT Edge | ![Onay][checkmark] |  |  |

CİHAZDAN buluta veri alımı yalnızca kullanım örneği olsa bile, IOT cihaz bağlantısı için tasarlanmış bir hizmet sağladığı gibi IOT hub'ı kullanarak öneririz. 

### <a name="next-steps"></a>Sonraki adımlar

IOT hub'ı yeteneklerini daha iyi keşfedilebilmesi için bkz: [IOT Hub Geliştirici kılavuzunun](iot-hub-devguide.md).

<!-- This one reference link is used over and over. --robinsh -->
[checkmark]: ./media/iot-hub-compare-event-hubs/ic195031.png
