---
title: Azure dijital İkizlerini genel Önizleme hizmet sınırları | Microsoft Docs
description: Hizmet sınırlamaları anlama Azure dijital İkizlerini genel Önizleme
author: dwalthermsft
manager: deshner
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: dwalthermsft
ms.openlocfilehash: aa5f6053bf1c98d2b84c02617da30f5d856ed3fc
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324303"
---
# <a name="public-preview-service-limits"></a>Genel Önizleme hizmet sınırları

Genel Önizleme süresince Azure dijital İkizlerini geçici abonelik, örneği ve aşağıda açıklanan oran sınırları olacaktır.

Bu kısıtlamalar, yeni hizmet ve birçok özelliklerini öğrenmeye kolaylaştırmaya yardımcı olmak üzere mevcut.

> [!NOTE]
> Bu sınırlar artar ve/veya genel kullanılabilirlik (GA tarafından) kaldırıldı.

## <a name="per-subscription-limits"></a>Abonelik başına limitler

Genel Önizleme sırasında her bir Azure aboneliği oluşturabilir veya aynı anda tam olarak bir Azure dijital İkizlerini örneği çalışıyor olması.

> [!TIP]
> Örneğiniz silme, yeni bir tane oluşturmak izin verir.

## <a name="per-instance-limits"></a>Örnek başına limitler

Buna karşılık her Azure dijital İkizlerini örneğine sahip olabilir:

- Bir `IoTHub` kaynak
- Bir `EventHub` DeviceMessage olay türü için uç nokta
- En fazla üç `EventHub`, `ServiceBus`, veya `EventGrid` olay türündeki uç noktalar `SensorChange`, `SpaceChange`, `TopologyOperation`, veya `UdfCustom`

## <a name="management-api-limits"></a>Yönetim API'si sınırları

Yönetim API'niz için istek oran sınırları şunlardır:

- Yönetim API'si saniyede 100 istek
- Tek bir yönetim API sorgu en çok 1000 nesne döndürebilir.

> [!IMPORTANT]
> 1000 nesne sınırı aşarsanız, hata iletisi alırsınız ve sorgunuzu basitleştirmek gerekir.

## <a name="udf-rate-limits"></a>UDF oran sınırları

Azure dijital İkizlerini Örneğinize yapılan tüm kullanıcı tanımlı işlev çağrılarının toplam sayısı aşağıdaki sınırları ayarlayın:

- 400 kitaplık istemci çağrısı / saniye
- Saniye başına 100 SendNotification çağrı

> [!NOTE]
> Aşağıdaki işlemleri geçici olarak uygulanacak ek oran sınırları neden olabilir:
> - Topoloji nesne meta verilerini düzenleme
> - UDF tanımı güncelleştirmeleri
> - İlk kez telemetri gönderen cihazları

## <a name="device-telemetry-limits"></a>Cihazın telemetri sınırları

Aşağıda uç tüm iletilerin toplam sayısını sınırlar cihazlarınızı Azure dijital İkizlerini Örneğinize gönderebilirsiniz:

- saniye başına 100 iletileri

## <a name="next-steps"></a>Sonraki adımlar

Bir Azure dijital İkizlerini örnek denemek için Git [kullanılabilir odaları bulmak için Hızlı Başlangıç](./quickstart-view-occupancy-dotnet.md).