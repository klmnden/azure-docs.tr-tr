---
title: Web kancaları kurallarında Azure IOT Central oluşturun | Microsoft Docs
description: Web kancaları, Azure IOT kuralları tetiklendiğinde diğer uygulamaları otomatik olarak bildirim sağlaması için Orta oluşturun.
author: viv-liu
ms.author: viviali
ms.date: 02/20/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 22167de6676837c45c48a0bafd19b1ba69578827
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60888332"
---
# <a name="create-webhook-actions-on-rules-in-azure-iot-central"></a>Web kancası eylemleri kurallarında Azure IOT Central oluşturun

*Bu konu, Oluşturucular ve Yöneticiler için geçerlidir.*

Web kancaları, diğer uygulama ve hizmetlere uzaktan izleme ve bildirimler için IOT Central uygulamanızın bağlamanızı sağlar. Web kancaları, diğer uygulama ve hizmetlerin bir kuralı tetiklendiğinde IOT Central uygulamanızda bağlantı otomatik olarak bildirin. Kural tetiklendiğinde IOT Central uygulamanız diğer uygulamanın HTTP uç noktasına bir POST isteği gönderir. Yük, cihaz ayrıntıları ve kural tetikleyici ayrıntılarını içerir.

## <a name="set-up-the-webhook"></a>Web kancası ' ayarlayın

Bu örnekte, Web kancalarını kullanma kuralları tetiklendiğinde bildirim almak için RequestBin için bağlanın.

1. Açık [RequestBin](https://requestbin.net/).

1. Yeni bir RequestBin ve kopyasını oluşturma **URL'sini**.

1. Oluşturma bir [telemetri kural](howto-create-telemetry-rules.md) veya [olayı kuralı](howto-create-event-rules.md). Kuralı kaydetmek ve yeni bir eylem ekleyin.

    ![Web kancası oluşturma ekranı](media/howto-create-webhooks/webhookcreate.png)

1. Web kancası eylemi seçin ve bir görünen ad girin ve URL'sini geri çağırma URL'si olarak yapıştırın.

1. Kural kaydedin.

Artık kuralı tetiklendiğinde RequestBin içinde görünen yeni bir istek bakın.

## <a name="payload"></a>Yükü

Kural tetiklendiğinde, bir HTTP POST isteği bir json yükü ölçümleri, cihaz, kural ve uygulama ayrıntılarını içeren geri çağırma URL'si için yapılır. Telemetri kuralı için yük aşağıdaki gibi görünür:

```json
{
    "id": "ID",
    "timestamp": "date-time",
    "device" : {
        "id":"ID",
        "name":  "Refrigerator1",
        "simulated" : true,
        "deviceId": "deviceID",
        "deviceTemplate":{
            "id": "ID",
            "version":"1.0.0"
        },
        "properties":{
            "device":{
                "firmwareversion":"1.0"
            },
            "cloud":{
                "location":"One Microsoft Way"
            }
        },
        "measurements":{
            "telemetry":{
                "temperature":20,
                "pressure":10
            }
        }

    },
    "rule": {
        "id": "ID",
        "name": "High temperature alert",
        "enabled": true,
        "deviceTemplate": {
            "id":"GUID",
            "version":"1.0.0"
        }
    },
    "application": {
        "id": "ID",
        "name": "Contoso app",
        "subdomain":"contoso-app"
    }
}
```

## <a name="known-limitations"></a>Bilinen sınırlamalar

Şu anda abone olma ve aboneliği, bir API aracılığıyla bu Web kancaları'ndan programlı hiçbir yolu yoktur.

Bu özelliği geliştirmeye ilişkin fikirler varsa, önerilerinizi gönderin bizim [Uservoice forumumuzu](https://feedback.azure.com/forums/911455-azure-iot-central).

## <a name="next-steps"></a>Sonraki adımlar

Ayarlama ve Web kancalarını kullanma öğrendiniz, önerilen sonraki keşfetmek için adımdır [Microsoft Flow, iş akışları oluşturarak](howto-add-microsoft-flow.md).
