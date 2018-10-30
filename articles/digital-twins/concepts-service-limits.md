---
title: Azure dijital İkizlerini genel Önizleme hizmet sınırları | Microsoft Docs
description: Hizmet sınırlamaları anlama Azure dijital İkizlerini genel Önizleme
author: dwalthermsft
manager: deshner
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: dwalthermsft
ms.openlocfilehash: f9a3d934de47630ac3fd2356001014d006c2a4eb
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212276"
---
# <a name="public-preview-service-limits"></a>Genel önizleme hizmet sınırları

Sırasında **genel Önizleme**, Azure dijital İkizlerini geçici abonelik, örneği ve aşağıda açıklanan oran sınırları vardır.

Bu kısıtlamalar, yeni hizmet ve birçok özelliklerini öğrenmeye kolaylaştırmaya yardımcı olmak üzere mevcut.

> [!NOTE]
> Bu sınırlar artar ve/veya tarafından kaldırılan **genel kullanılabilirlik** (**GA**).

## <a name="per-subscription-limits"></a>Abonelik başına limitler

Sırasında **genel Önizleme**, her bir Azure aboneliği oluşturun veya aynı anda tam olarak bir Azure dijital İkizlerini örneği çalışıyor olması.

> [!TIP]
> Örneğiniz silme, yeni bir tane oluşturmak izin verir.

## <a name="per-instance-limits"></a>Örnek başına limitler

Buna karşılık her Azure dijital İkizlerini örneğine sahip olabilir:

- Bir **IoTHub** kaynak
- Bir **EventHub** olay türü için uç nokta **DeviceMessage**
- En fazla üç **EventHub**, **ServiceBus**, veya **EventGrid** olay türündeki uç noktalar **SensorChange**, **SpaceChange** , **TopologyOperation**, veya **UdfCustom**

## <a name="management-api-limits"></a>Yönetim API'si sınırları

Yönetim API'niz için istek oran sınırları şunlardır:

- Yönetim API'si saniyede 100 istek
- Tek bir yönetim API sorgu en çok 1000 nesne döndürebilir.

> [!IMPORTANT]
> 1000 nesne sınırı aşarsanız, hata iletisi alırsınız ve sorgunuzu basitleştirmek gerekir.

## <a name="udf-rate-limits"></a>UDF oran sınırları

Azure dijital İkizlerini Örneğinize yapılan tüm kullanıcı tanımlı işlev çağrılarının toplam sayısı aşağıdaki sınırları ayarlayın:

- 400 kitaplık istemci çağrısı / saniye
- 100 **SendNotification** saniye başına çağrı

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