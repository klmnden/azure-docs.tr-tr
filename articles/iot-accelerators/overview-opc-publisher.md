---
title: OPC Publisher - Azure nedir | Microsoft Docs
description: OPC Publisher'ın genel bakış
author: dominicbetts
ms.author: dobett
ms.date: 06/10/2019
ms.topic: overview
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: c738e927a352373d7f5a4aeb5697e07134a98cba
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603660"
---
# <a name="what-is-opc-publisher"></a>OPC Publisher nedir?

OPC Publisher olduğunu gösteren bir başvuru uygulaması nasıl yapılır:

- Var olan OPC UA sunucularına bağlanır.
- Azure IOT hub'a bir JSON yükü kullanarak, OPC UA Pub/Sub biçiminde JSON olarak kodlanmış telemetri verilerini OPC UA sunucuları yayımlayın.

Azure IOT Hub istemci SDK'sı destekler aktarım protokolden herhangi birini kullanabilirsiniz: HTTPS, AMQP ve MQTT.

Başvuru uygulamasını içerir:

- OPC UA *istemci* ağınızda sahip olduğunuz mevcut OPC UA sunucuları için bağlama.
- OPC UA *sunucu* bağlantı noktasında hangi yayımlanır ve IOT hub'ı sunar yönetmek için kullanabileceğiniz 62222 yapmak yöntemlerini doğrudan.

İndirebileceğiniz [OPC Publisher başvuru uygulaması](https://github.com/Azure/iot-edge-opc-publisher) github'dan.

Uygulama, .NET Core teknolojisi kullanılarak uygulanır ve .NET Core tarafından desteklenen herhangi bir platformda çalışabilir.

OPC Publisher, belirli sayıda canlı tutma istekleri için yanıt yoksa uç noktalarına bağlantıları kurmak için yeniden deneme mantığını uygular. Örneğin, bir OPC UA sunucusu bir güç kesintisi nedeniyle yanıt vermemeye başlarsa.

Bir OPC UA sunucusuna her farklı yayımlama aralığı için uygulama üzerinde tüm düğümleri Bu yayımlama aralığı ile güncelleştirilir ayrı bir abonelik oluşturur.

OPC Publisher, ağ yükünü azaltmak için IOT Hub'ına gönderilen veriler toplu işlerini destekler. Bu toplu işleme yalnızca yapılandırılmış paket boyutu üst sınırına ulaşıldığında bir paketi, IOT Hub'ına gönderir.

Bu uygulama, OPC Foundation OPC UA başvurusu yığınında NuGet paketleri olarak kullanır. Bkz: [ https://opcfoundation.org/license/redistributables/1.3/ ](https://opcfoundation.org/license/redistributables/1.3/) lisans koşulları için.

### <a name="next-steps"></a>Sonraki adımlar

OPC Publisher ne olduğunu öğrendiğinize göre artık önerilen sonraki adıma öğrenmektir nasıl [OPC yayımcı yapılandırma](howto-opc-publisher-configure.md).
