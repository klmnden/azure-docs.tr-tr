---
title: Azure IOT hub'ı Azure Event hubs'a karşılaştırın | Microsoft Docs
description: İşlevsel farklılıklar ve kullanım örneklerini vurgulama IOT Hub ve Event hubs'ı Azure Hizmetleri karşılaştırması. Desteklenen protokoller, cihaz yönetimi, izleme, karşılaştırma içerir ve dosyayı yükler.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: kgremban
ms.openlocfilehash: 20bb0cb6982bcbea6b18989099322cfd3389b0b0
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54819654"
---
# <a name="connecting-iot-devices-to-azure-iot-hub-and-event-hubs"></a>IOT cihazları Azure'a bağlanıyor: IOT Hub ile Event Hubs

Azure, farklı bağlantı ve verileriniz için bulutun tüm gücü bağlanmanıza yardımcı olması için iletişim türleri için özel olarak geliştirilen hizmetleri sağlar. Azure IOT Hub hem de Azure Event Hubs, büyük miktarlarda veri ve işlem alma veya iş öngörülerine ilişkin verileri depolamak bulut hizmetleridir. Her ikisi de düşük gecikme süresi ve yüksek güvenilirlikle veri alımını destekler, iki hizmet benzerdir, ancak bunlar farklı amaçlar için tasarlanmıştır. IOT Hub, özellikle, Azure Event Hubs, büyük veri akışı için tasarlanmıştır ancak bulutta IOT cihazları ölçekli, bağlama benzersiz gereksinimlerini karşılamak için geliştirilmiştir. Microsoft, IOT cihazları Azure'a bağlanmak için Azure IOT Hub'ı kullanarak önerir. Bu yüzden

Azure IOT hub'a bağlanan sürücü iş öngörülerini ve Otomasyon için veri toplamak üzere IOT cihazları bulut ağ geçidi ' dir. Ayrıca, IOT Hub, cihazlarınız ve arka uç sistemlerinizi arasındaki ilişkiyi zenginleştirin özellikleri içerir. Çift yönlü iletişim yeteneklerini cihazlardan veri alırken Ayrıca, ortalama gönderin komutları ve ilkeleri geri cihazlara gibi özellikleri güncelleştirmek veya cihaz yönetim eylemleri çağırmak için.  Bu bulut-cihaz bağlantısı, ayrıca önemli bulut zekasını uç cihazlarınıza Azure IOT Edge ile teslim etme yeteneği çalıştırır. Benzersiz cihaz düzeyinde kimlik IOT hub'ı sağlanan yardımcı olur göre daha iyi güvenli IOT çözümünüzden olası saldırıları. 

[Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) akış hizmeti Azure büyük veri. Günlük milyarlarca müşterilerin nerede gönderebilir yüksek aktarım hızı veri akış senaryoları için tasarlanmıştır. Event Hubs, akışınız ölçeklendirmek için bölümlenmiş tüketici modelinin kullanır ve büyük veri ve Azure Databricks, Stream Analytics, ADLS ve HDInsight gibi Analiz Hizmetleri tümleştirilmiştir. Event Hubs yakalama ve otomatik Şişme gibi özelliklerle, bu hizmet, büyük veri uygulamaları ve çözümleri desteklemek için tasarlanmıştır. Ayrıca, böylece IOT çözümünüz hem de Event Hubs'ın Bilim insanları için inanılmaz gücü avantajlardan IOT Hub Event Hubs telemetri akış yolu yararlanır.

Her iki çözüm de büyük bir ölçekte veri alımı için tasarlandığında özetlemek gerekirse, yalnızca IOT Hub, IOT cihazlarınızı Azure bulutuna bağlayarak iş değerini en üst düzeye çıkarmak tasarlanmış zengin bir IOT özgü özellikler sağlar.  Veri alımı senaryolarınızı desteklemek için IOT Hub ile başlayarak, IOT yolculuğunuza yeni başlıyor, iş ve teknik gereksinimlerinizi şart sonra tam özellikli IOT özelliklere anında erişim sahip sağlanması.

Aşağıdaki tabloda, IOT özelliklerini değerlendirirken nasıl IOT Hub'ın iki katmanda Event Hubs karşılaştırması hakkında ayrıntılar sağlar. IOT Hub'ın standart ve temel katmanları hakkında daha fazla bilgi için bkz. [doğru IOT Hub katmanını seçme](iot-hub-scaling.md).

| IOT özelliği | IOT hub'ı standart katman | IOT hub'ı temel katman | Event Hubs |
| --- | --- | --- | --- |
| CİHAZDAN buluta ileti gönderme | ![İşaretli][checkmark] | ![İşaretli][checkmark] | ![İşaretli][checkmark] |
| Protokoller: HTTPS, AMQP, AMQP webSockets üzerinden | ![İşaretli][checkmark] | ![İşaretli][checkmark] | ![İşaretli][checkmark] |
| Protokoller: MQTT, MQTT webSockets üzerinden | ![İşaretli][checkmark] | ![İşaretli][checkmark] |  |
| Cihaz kimliği başına | ![İşaretli][checkmark] | ![İşaretli][checkmark] |  |
| Cihazlardan karşıya dosya yükleme | ![İşaretli][checkmark] | ![İşaretli][checkmark] |  |
| Cihaz Sağlama Hizmeti | ![İşaretli][checkmark] | ![İşaretli][checkmark] |  |
| Buluttan cihaza ileti gönderme | ![İşaretli][checkmark] |  |  |
| Cihaz ikizi ve cihaz Yönetimi | ![İşaretli][checkmark] |  |  |
| Cihaz akışları (Önizleme) | ![İşaretli][checkmark] |  |  |
| IoT Edge | ![İşaretli][checkmark] |  |  |

CİHAZDAN buluta veri alımı yalnızca kullanım örneği olsa bile, IOT cihaz bağlantısı için tasarlanmış bir hizmet sağladığı gibi IOT hub'ı kullanarak öneririz. 

### <a name="next-steps"></a>Sonraki adımlar

IOT hub'ı yeteneklerini daha iyi keşfedilebilmesi için bkz: [IOT Hub Geliştirici kılavuzunun](iot-hub-devguide.md).

<!-- This one reference link is used over and over. --robinsh -->
[checkmark]: ./media/iot-hub-compare-event-hubs/ic195031.png
