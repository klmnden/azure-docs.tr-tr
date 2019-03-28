---
title: Azure IOT Central bir kuraldan birden fazla eylem çalıştırma | Microsoft Docs
description: Tek bir IOT Central kuraldan birden fazla eylem çalıştırabilir ve birden çok kurallardan çalıştırmak üzere eylemleri yeniden kullanılabilir grupları oluşturun.
services: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 03/19/2019
ms.topic: conceptual
ms.service: iot-central
manager: philmea
ms.openlocfilehash: 857d747fa691d1ec2b386d5931a7edea08b7e609
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58523128"
---
# <a name="group-multiple-actions-to-run-from-one-or-more-rules"></a>Bir veya daha fazla kurallardan çalıştırmak için birden fazla eylem grubu

*Bu makalede, Oluşturucular ve Yöneticiler için geçerlidir.*

Azure IOT Central içinde bir koşul karşılandığında eylemleri çalıştırmak için kurallar oluşturun. Kurallar, cihaz telemetrisi veya olayları temel alır. Örneğin, bir cihaz sıcaklık bir eşiği aştığında bir işleç bildirebilir. Bu makalede nasıl kullanılacağını [Azure İzleyici](../azure-monitor/overview.md) *Eylem grupları* bir IOT Central kuralı için birden fazla eylem eklemek için. Bir eylem grubu için birden çok kural ekleyebilirsiniz. Bir [eylem grubu](../azure-monitor/platform/action-groups.md) Azure aboneliğinin sahibi tarafından tanımlanan bildirim tercihleri oluşan bir koleksiyondur.

## <a name="prerequisites"></a>Önkoşullar

- Bir Kullandıkça Öde uygulama
- Bir Azure hesabı ve Azure İzleyici Eylem grupları oluşturmak ve yönetmek için abonelik

## <a name="create-action-groups"></a>Eylem grubu oluşturma

Yapabilecekleriniz [Azure portalında Eylem grupları oluşturma ve yönetme](../azure-monitor/platform/action-groups.md) veya bir [Azure Resource Manager şablonu](../azure-monitor/platform/action-groups-create-resource-manager-template.md).

Bir eylem grubu yapabilirsiniz:

- E-posta, SMS, gibi bildirimler göndereceğini ya da bir sesli arama yapmak.
- Bir Web kancasına çağrı gibi bir eylem çalıştırın.

Aşağıdaki ekran görüntüsünde, e-posta ve SMS bildirimleri gönderir ve bir Web kancası çağıran bir eylem grubu gösterilmektedir:

![Eylem grubu](media/howto-use-action-groups/actiongroup.png)

Bir IOT Central kuralda bir eylem grubu kullanmak için aynı abonelikte Azure IOT Central uygulamasına eylem grubu olmalıdır.

## <a name="use-an-action-group"></a>Bir eylem grubu kullanın

IOT Central uygulamanızda bir eylem grubu kullanmak için önce bir telemetri veya olay kuralı oluşturun. Kural için bir eylem eklediğinizde, seçin **Azure İzleyici Eylem grupları**:

![Eylem seçin](media/howto-use-action-groups/chooseaction.png)

Bir eylem grubu, Azure aboneliğinizi seçin:

![Eylem grubu seçin](media/howto-use-action-groups/chooseactiongroup.png)

**Kaydet**’i seçin. Eylem grubu kuralı tetiklendiğinde çalıştırılacak Eylemler listesinde görünür:

![Eylem grubu kaydedildi](media/howto-use-action-groups/savedactiongroup.png)

Aşağıdaki tabloda desteklenen eylem türleri için gönderilen bilgiler özetlenmektedir:

| Eylem türü | Çıkış biçimi |
| ----------- | -------------- |
| Email       | Standart IOT Central e-posta şablonu |
| SMS         | Azure IOT Central uyarısı: ${applicationName} - "tetikleyen"${deviceName}"konumunda {triggerTime} ${triggerDate} üzerinde {ruleName}" |
| Ses       | Azure I.O.T Central uyarısı: uygulama ${applicationName} "${triggerTime}, ${triggerDate} adresindeki"${deviceName}"cihazda tetiklenen {ruleName}" kuralı |
| Web Kancası     | {"schemaId": "AzureIoTCentralRuleWebhook", "veri": {[normal Web kancası yükü](#payload)}} |

Bir eylem grubundan örnek SMS iletisi aşağıda gösterilmiştir:

`iotcentral: Azure IoT Central alert: Sample Contoso 22xu4spxjve - "Low pressure alert" triggered on "Refrigerator 2" at March 20, 2019 10:12 UTC`

<a id="payload"></a> Aşağıdaki JSON bir örnek Web kancası eylemi yükü gösterir:

```json
{
  "schemaId":"AzureIoTCentralRuleWebhook",
  "data":{
    "id":"97ae27c4-17c5-4e13-9248-65c7a2c57a1b",
    "timestamp":"2019-03-20T10:53:17.059Z",
    "rule":{
      "id":"031b660e-528d-47bb-b33d-f1158d7e31bf",
      "name":"Low pressure alert",
      "enabled":true,
      "deviceTemplate":{
        "id":"c318d580-39fc-4aca-b995-843719821049",
        "version":"1.0.0"
      }
    },
    "device":{
      "id":"2383d8ba-c98c-403a-b4d5-8963859643bb",
      "name":"Refrigerator 2",
      "simulated":true,
      "deviceId":"2383d8ba-c98c-403a-b4d5-8963859643bb",
      "deviceTemplate":{
        "id":"c318d580-39fc-4aca-b995-843719821049",
        "version":"1.0.0"
      },
      "measurements":{
        "telemetry":{
           "pressure":343.269190673549
        }
      }
    },
    "application":{
      "id":"8e70742b-0d5c-4a1d-84f1-4dfd42e61c7b",
      "name":"Sample Contoso",
      "subdomain":"sample-contoso"
    }
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Kuralları ile Eylem grupları kullanmayı öğrendiniz, önerilen sonraki adıma öğrenmektir nasıl [cihazlarınızı yönetme](howto-manage-devices.md).
