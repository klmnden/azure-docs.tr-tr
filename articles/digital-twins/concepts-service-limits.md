---
title: Azure dijital İkizlerini genel Önizleme hizmet sınırları | Microsoft Docs
description: Azure dijital İkizlerini genel Önizleme hizmet sınırları anlayın.
author: dwalthermsft
manager: deshner
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/03/2019
ms.author: dwalthermsft
ms.openlocfilehash: 7d9686b9bcc6cb89fabf4fdaa79bf5b8c6c45ddc
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54020634"
---
# <a name="public-preview-service-limits"></a>Genel önizleme hizmet sınırları

Genel Önizleme sırasında Azure dijital İkizlerini şu geçici abonelik, örneği ve hız sınırları vardır.

Bu kısıtlamalar, yeni hizmet ve birçok özelliklerini öğrenmeye kolaylaştırmaya yardımcı olmak üzere mevcut.

> [!NOTE]
> Bu sınırlar artırabilir veya genel kullanılabilirlik (GA) tarafından kaldırıldı.

## <a name="per-subscription-limits"></a>Abonelik başına limitler

Genel Önizleme sırasında her bir Azure aboneliği oluşturun veya aynı anda yalnızca bir Azure dijital İkizlerini örneği çalıştırın.

> [!TIP]
> Örneğiniz silerseniz, yeni bir tane oluşturabilirsiniz.

## <a name="per-instance-limits"></a>Örnek başına limitler

Buna karşılık her Azure dijital İkizlerini örneğine sahip olabilir:

- Tam olarak bir **IoTHub** kaynak.
- Tam olarak bir **EventHub** olay türü için uç nokta **DeviceMessage**.
- En fazla üç **EventHub**, **ServiceBus**, veya **EventGrid** olay türündeki uç noktalar **SensorChange**, **SpaceChange** , **TopologyOperation**, veya **UdfCustom**.

> [!NOTE]
> Yukarıdaki Azure IOT varlıklarını oluşturma genellikle tanımlanan bazı parametreler genel Önizleme sırasında gerekli değildir.
> - Başvurun [Swagger başvuru belgeleri](./how-to-use-swagger.md) en son API belirtimleri için.

## <a name="azure-digital-twins-management-api-limits"></a>Azure dijital İkizlerini yönetim API sınırları

Azure dijital İkizlerini yönetim API isteği oran sınırlarını şunlardır:

- Azure dijital İkizlerini yönetim API'si saniyede 100 istek.
- Tek bir Azure dijital İkizlerini yönetim API'si sorgu tarafından döndürülen en fazla 1.000 nesneleri.

> [!IMPORTANT]
> 1000 nesne sınırı aşarsa, bir hatayla karşılaştıysanız ve sorgunuzu basitleştirmeniz gerekir.

## <a name="user-defined-functions-rate-limits"></a>Kullanıcı tanımlı işlevleri oran sınırları

Azure dijital İkizlerini Örneğinize yapılan tüm kullanıcı tanımlı işlev çağrılarının toplam sayısı aşağıdaki sınırları ayarlayın:

- 400 kitaplık istemci çağrısı / saniye
- 100 **SendNotification** saniye başına çağrı

> [!NOTE]
> Aşağıdaki işlemleri geçici olarak uygulanacak ek oran sınırları neden olabilir:
> - Topoloji nesne meta verilerde yapılan düzenlemeler
> - Kullanıcı tanımlı işlev tanımında yapılan güncelleştirmeler
> - İlk kez telemetri gönderen cihazları

## <a name="device-telemetry-limits"></a>Cihazın telemetri sınırları

Aşağıdaki sınırlar cihazlarınızı Azure dijital İkizlerini Örneğinize gönderebilirsiniz tüm iletileri toplam sayısı cap:

- saniye başına 100 iletileri

## <a name="next-steps"></a>Sonraki adımlar

Bir Azure dijital İkizlerini örnek denemek için Git [kullanılabilir odaları bulmak için Hızlı Başlangıç](./quickstart-view-occupancy-dotnet.md).