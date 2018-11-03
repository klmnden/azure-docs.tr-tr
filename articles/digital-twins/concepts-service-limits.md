---
title: Azure dijital İkizlerini genel Önizleme hizmet sınırları | Microsoft Docs
description: Azure dijital İkizlerini genel Önizleme hizmet sınırları anlama
author: dwalthermsft
manager: deshner
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: dwalthermsft
ms.openlocfilehash: 86ae75118dd1311ea2ae92fb718fe4c58b8e5673
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50961764"
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

- Bir **IoTHub** kaynak.
- Bir **EventHub** olay türü için uç nokta **DeviceMessage**.
- En fazla üç **EventHub**, **ServiceBus**, veya **EventGrid** olay türündeki uç noktalar **SensorChange**, **SpaceChange** , **TopologyOperation**, veya **UdfCustom**.

## <a name="management-api-limits"></a>Yönetim API'si sınırları

Yönetim API'niz için istek oran sınırları şunlardır:

- Yönetim API'si saniyede 100 istek.
- Tek bir yönetim API sorgu tarafından döndürülen en fazla 1.000 nesneleri. 

> [!IMPORTANT]
> 1000 nesne sınırı aşarsa, bir hatayla karşılaştıysanız ve sorgunuzu basitleştirmeniz gerekir.

## <a name="udf-rate-limits"></a>UDF oran sınırları

Azure dijital İkizlerini Örneğinize yapılan tüm kullanıcı tanımlı işlev çağrılarının toplam sayısı aşağıdaki sınırları ayarlayın:

- 400 kitaplık istemci çağrısı / saniye
- 100 **SendNotification** saniye başına çağrı

> [!NOTE]
> Aşağıdaki işlemleri geçici olarak uygulanacak ek oran sınırları neden olabilir:
> - Topoloji nesne meta verilerde yapılan düzenlemeler
> - UDF tanımı için yapılan güncelleştirmeler
> - İlk kez telemetri gönderen cihazları

## <a name="device-telemetry-limits"></a>Cihazın telemetri sınırları

Aşağıdaki sınırlar cihazlarınızı Azure dijital İkizlerini Örneğinize gönderebilirsiniz tüm iletileri toplam sayısı cap:

- saniye başına 100 iletileri

## <a name="next-steps"></a>Sonraki adımlar

Bir Azure dijital İkizlerini örnek denemek için Git [kullanılabilir odaları bulmak için Hızlı Başlangıç](./quickstart-view-occupancy-dotnet.md).