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
ms.openlocfilehash: 303a2bde0a1e0b25ca6eb145e7b0cd6c91fff351
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="comparison-of-azure-iot-hub-and-azure-event-hubs"></a>Azure IOT Hub ve Azure Event Hubs karşılaştırması

Azure IOT Hub ve Azure Event Hubs, büyük miktarlarda veri ve işlem alma veya iş öngörüleri için bu verileri depolamak bulut hizmetleridir. Her ikisi de destekler, iki hizmet benzer düşük gecikme süreli ve yüksek güvenilirlikle olay ve telemetri verilerini işleme. Ancak, yalnızca IOT Hub ölçekli Internet şeyler senaryoları desteklemek için gereken belirli özellikleri ile geliştirilmiştir. 

Azure IOT Hub cihazları bağlanan ve işletme öngörüleri ve otomasyonu için verileri toplar bulut ağ geçididir. Bu bulut için veri akışı ve ölçekte, cihazlarınızı yönetmek kolaylaştırır. IOT hub'ı ve diğer veri alım hizmetleri arasında önemli bir fark yatan unsur IOT Hub, cihazlarınız ve arka uç sistemleri arasındaki ilişkiyi zenginleştirmek özellikleri dahildir. Çift yönlü iletişim özellikler aygıtlardan veri alırken de iletileri özelliklerini güncelleştirmek veya bir eylemi çağırmak için cihazlara geri gönderebilirsiniz, anlamına gelir. Cihaz düzeyinde kimlik sisteminizin güvenliğini korumaya yardımcı olur. Sınır cihazları üzerine bilgi işlem taşır bulut hizmeti mantığı dağıtılmış.

[Azure Event Hubs] [ Azure Event Hubs] işleyen ve büyük miktarlarda veri ve telemetri depolamak bir olay alım hizmetidir. Olay hub'ları bir büyük ölçekte, hem ağlar arası veri merkezi ve içi veri merkezi senaryoları bağlamında olay alımı için tasarlanmıştır ancak IOT Hub ile kullanılabilen zengin IOT özgü özellikleri sağlamaz. Bu nedenle, Event Hubs, IOT çözümleriniz için önermiyoruz. 

Aşağıdaki tabloda IOT yeteneklerini değerlendirirken nasıl Event Hubs'a IOT Hub'ın iki katmanı karşılaştırmak hakkında ayrıntılar sağlar. IOT Hub'ın standart ve temel katmanları hakkında daha fazla bilgi için bkz: [sağ IOT hub'ı katmanı seçme][lnk-scaling].

| IOT özelliği | IOT hub'ı standart katmanı | IOT hub'ı temel katmanı | Event Hubs |
| --- | --- | --- | --- |
| Cihaz bulut Mesajlaşma | ![İşaretli][1] | ![İşaretli][1] | ![İşaretli][1] |
| İletişim kuralları: Websockets üzerinden HTTPS, AMQP, AMQP | ![İşaretli][1] | ![İşaretli][1] | ![İşaretli][1] |
| Protokolleri: MQTT, MQTT websockets üzerinden | ![İşaretli][1] | ![İşaretli][1] |  |
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