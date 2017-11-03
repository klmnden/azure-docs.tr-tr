---
title: "Azure zaman serisi Öngörüler ortamınız için bir IOT hub'ı olay kaynağı ekleme | Microsoft Docs"
description: "Bu öğretici bir IOT hub'ına zaman serisi Öngörüler ortamınıza bağlı bir olay kaynağı ekleme kapsar"
keywords: 
services: time-series-insights
documentationcenter: 
author: sandshadow
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/19/2017
ms.author: edett
ms.openlocfilehash: 3b98728ced81256d05b1bed2c92fc66c5ca61b98
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-add-an-iot-hub-event-source"></a>Bir IOT hub'ı olay kaynağı ekleme

Bu öğretici zaman serisi Öngörüler ortamınız için IOT Hub'ından okuyan bir olay kaynağı eklemek için Azure portalını kullanmayı kapsar.

## <a name="prerequisites"></a>Ön koşullar

Bir IOT Hub oluşturdunuz ve olaylar için yazma. IOT hub'ları hakkında daha fazla bilgi için bkz: <https://azure.microsoft.com/services/iot-hub/>

> [Tüketici grupları] Herhangi bir tüketiciye paylaşılmayan kendi ayrılmış bir tüketici grubundaki her zaman serisi Öngörüler olay kaynağı olmalıdır. Birden çok okuyucular aynı tüketici grubu olaylarından kullanırsa, tüm okuyucular hatalar görmeniz olasıdır. Ayrıntılar için bkz [IOT Hub Geliştirici Kılavuzu](../iot-hub/iot-hub-devguide.md).

## <a name="choose-an-import-option"></a>Bir içe aktarma seçeneği seçin

Olay kaynağı ayarlarını el ile girilebilir veya IOT hub'ı kullanılabilen IOT hub'ları öğesinden seçilebilir.
İçinde **alma seçeneği** Seçici, aşağıdaki seçeneklerden birini seçin:

* IOT hub'ı ayarları sağlayın el ile
* Kullanılabilir aboneliklerden IOT hub'ı kullanın

### <a name="select-an-available-iot-hub"></a>Kullanılabilir IOT hub'ı seçin

Aşağıdaki tabloda, bir olay kaynağı olarak kullanılabilir IOT hub'ı seçerken açıklamasını yeni olay kaynağı sekmesiyle her seçenekte açıklanmaktadır:

| ÖZELLİK ADI | AÇIKLAMA |
| --- | --- |
| Olay kaynağı adı | Olay kaynağı adı. Bu ad zaman serisi Öngörüler ortamınızda benzersiz olması gerekir.
| Kaynak | Seçin **IOT hub'ı** bir IOT hub'ı olay kaynağı oluşturmak için.
| Abonelik kimliği | Bu IOT hub'ının oluşturulduğu abonelik seçin.
| IOT hub'ı adı | IOT Hub adını seçin.
| IOT hub ilke adı | IOT hub'ı Ayarlar sekmesinde bulunan paylaşılan erişim ilkesi seçin. Her paylaşılan erişim ilkesinin bir adı, ayarlayın ve erişim anahtarları izinleri vardır. Olay kaynağı için paylaşılan erişim ilkesi *gerekir* sahip **service bağlanma** izinleri.
| IOT hub tüketici grubu | IOT Hub'ından olayları okumak için tüketici grubu. Olay kaynağı için ayrılmış bir tüketici grubu kullanmak için önerilir.

### <a name="provide-iot-hub-settings-manually"></a>IOT hub'ı ayarları sağlayın el ile

Aşağıdaki tabloda ayarları girerken açıklamasını ile yeni bir olay kaynağı sekmesindeki her bir özellik açıklanmaktadır el ile:

| ÖZELLİK ADI | AÇIKLAMA |
| --- | --- |
| Olay kaynağı adı | Olay kaynağı adı. Bu ad zaman serisi Öngörüler ortamınızda benzersiz olması gerekir.
| Kaynak | Seçin **IOT hub'ı** bir IOT hub'ı olay kaynağı oluşturmak için.
| Abonelik kimliği | Bu IOT hub'ının oluşturulduğu abonelik.
| Kaynak grubu | Bu IOT hub'ının oluşturulduğu abonelik.
| IOT hub'ı adı | IOT Hub'ınızın adıdır. IOT hub'ınızı oluşturduğunuzda, ona bir özel ad da vermiş
| IOT hub ilke adı | Paylaşılan Erişim İlkesi IOT hub'ı ayarları sekmesinde oluşturulabilir. Her paylaşılan erişim ilkesinin bir adı, ayarlayın ve erişim anahtarları izinleri vardır. Olay kaynağı için paylaşılan erişim ilkesi *gerekir* sahip **service bağlanma** izinleri.
| IOT hub ilke anahtarı | Hizmet veri yolu ad alanına erişimi kimlik doğrulaması için kullanılan paylaşılan erişim anahtarı. Birincil veya ikincil anahtarı buraya girin.
| IOT hub tüketici grubu | IOT Hub'ından olayları okumak için tüketici grubu. Olay kaynağı için ayrılmış bir tüketici grubu kullanmak için önerilir.

## <a name="next-steps"></a>Sonraki adımlar

1. Ortamınıza veri erişim ilkesine eklemek [tanımla veri erişim ilkeleri](time-series-insights-data-access.md)
1. Ortamınızda erişim [zaman serisi Insights portalı](https://insights.timeseries.azure.com)