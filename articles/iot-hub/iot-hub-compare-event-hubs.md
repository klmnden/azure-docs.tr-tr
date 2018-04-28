---
title: Azure IOT hub'ı Azure Event hubs'a karşılaştırma | Microsoft Docs
description: İşlevsel farklılıklar ve kullanım örnekleri vurgulama IOT Hub ve olay hub'ları Azure Hizmetleri karşılaştırması. Desteklenen protokoller, cihaz yönetimi, izleme, karşılaştırma içerir ve dosyayı yükler.
services: iot-hub
documentationcenter: ''
author: kgremban
manager: timlt
editor: ''
ms.assetid: aeddea62-8302-48e2-9aad-c5a0e5f5abe9
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/01/2018
ms.author: kgremban
ms.openlocfilehash: b86132b42aef981e6218b27e271e6db645d14071
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="connecting-iot-devices-to-azure-iot-hub-and-event-hubs"></a>IOT cihazları Azure'a bağlanılıyor: IOT Hub ve Event Hubs

Azure farklı bağlantısı ve verilerinizi bulut kuvvetine bağlanmanıza yardımcı olması için iletişim türleri için özel olarak geliştirilen hizmetleri sağlar. Azure IOT Hub ve Azure Event Hubs, büyük miktarlarda veri ve işlem alma veya iş öngörüleri için bu verileri depolamak bulut hizmetleridir. Her ikisi de düşük gecikme süreli ve yüksek güvenilirlikle veri alım destekleyen iki hizmet benzer ancak farklı amaçlar için tasarlanmıştır. IOT Hub, özellikle, IOT cihazları, ölçekli, Azure Event Hubs büyük veri akışı için tasarlanmasına rağmen bulut bağlanma benzersiz gereksinimlerini karşılayacak şekilde geliştirilmiştir. Microsoft Azure IOT cihazları bağlamak için Azure IOT hub'ı kullanmanızı önerir bu yüzden

Azure IOT Hub sürücü iş Öngörüler ve otomasyonu için veri toplamak üzere IOT cihazları bağlayan bulut ağ geçididir. Ayrıca, IOT Hub, cihazlarınız ve arka uç sistemleri arasındaki ilişkiyi zenginleştirmek özellikler içerir. Çift yönlü iletişim özellikler aygıtlardan veri alırken, ayrıca olabileceği anlamına gelir Gönder komutlar ve ilkeleri geri aygıtlar için örneğin, özellikleri güncelleştirmek veya aygıt yönetimi eylemleri çağırma.  Bu bulut-cihaz bağlantısı bulut Intelligence kenar cihazlarınızı Azure IOT Edge teslim önemli özelliği de çalıştırır. Benzersiz cihaz düzeyinde kimlik IOT çözümünüzden olası saldırılara karşı daha iyi güvenli IOT hub'ı yardımcı tarafından sağlanır. 

[Azure Event Hubs] [ Azure Event Hubs] büyük veri hizmeti Azure akış. Müşteriler günlük milyarlarca burada gönderebilir yüksek verimlilik veri akış senaryoları için tasarlanmıştır. Event Hubs, akış ölçeklemek için bölümlenmiş tüketici modeli kullanır ve büyük veri ve Analiz Hizmetleri Databricks, akış analizi, ADLS ve Hdınsight gibi Azure tümleşiktir. Olay hub'ları yakalamak ve otomatik Şişir gibi özellikler ile bu hizmet, büyük veri uygulama ve çözümler destekleyecek şekilde tasarlanmıştır. Ayrıca, IOT çözümünüzü olay hub'ları inanılmaz gücünden de avantaj şekilde IOT hub'ı olay hub'ları telemetri akışı yolu yararlanır.

Her iki çözüm de büyük bir ölçekte veri alımı için tasarlanmıştır ancak özetlemek için yalnızca IOT Hub, IOT cihazlarınızı Azure buluta bağlamak iş değerini en üst düzeye çıkarmak için tasarlanmış zengin bir IOT özgü özellikleri sağlar.  Veri alımı senaryolarınızı desteklemek için IOT Hub ile başlayarak, IOT Yolculuğunuzun yalnızca başlayarak, işletme ve teknik gereksinimlerinizi şart sonra tam özellikli IOT yetenekleri anlık erişiminiz olması garanti edecek.

Aşağıdaki tabloda IOT yeteneklerini değerlendirirken nasıl Event Hubs'a IOT Hub'ın iki katmanı karşılaştırmak hakkında ayrıntılar sağlar. IOT Hub'ın standart ve temel katmanları hakkında daha fazla bilgi için bkz: [sağ IOT hub'ı katmanı seçme][lnk-scaling].

| IOT özelliği | IOT hub'ı standart katmanı | IOT hub'ı temel katmanı | Event Hubs |
| --- | --- | --- | --- |
| Cihaz bulut Mesajlaşma | ![İşaretli][1] | ![İşaretli][1] | ![İşaretli][1] |
| İletişim kuralları: WebSockets üzerinden HTTPS, AMQP, AMQP | ![İşaretli][1] | ![İşaretli][1] | ![İşaretli][1] |
| Protokolleri: MQTT, MQTT webSockets üzerinden | ![İşaretli][1] | ![İşaretli][1] |  |
| Cihaz kimliği başına | ![İşaretli][1] | ![İşaretli][1] |  |
| Cihazlardan karşıya dosya yükleme | ![İşaretli][1] | ![İşaretli][1] |  |
| Cihaz Sağlama Hizmeti | ![İşaretli][1] | ![İşaretli][1] |  |
| Buluttan cihaza ileti gönderme | ![İşaretli][1] |  |  |
| Cihaz çifti ve aygıt yönetimi | ![İşaretli][1] |  |  |
| IoT Edge | ![İşaretli][1] |  |  |

Yalnızca kullanım örneği cihaz bulut veri alımı olsa bile, IOT cihaz bağlantısı için tasarlanmış bir hizmet sağladığı gibi IOT hub'ı kullanarak öneririz. 

### <a name="next-steps"></a>Sonraki adımlar

IOT hub'ı yeteneklerini daha fazlasını keşfetmek için bkz: [IOT Hub Geliştirici Kılavuzu][lnk-devguide]


[Azure Event Hubs]: ../event-hubs/event-hubs-what-is-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md

<!--Image references-->
[1]: ./media/iot-hub-compare-event-hubs/ic195031.png
