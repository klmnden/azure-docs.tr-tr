---
title: "Azure zaman serisi Öngörüler ortamınız için bir olay hub'ı olay kaynağı ekleme | Microsoft Docs"
description: "Bu öğretici, bir olay hub'ına zaman serisi Öngörüler ortamınıza bağlı bir olay kaynağı eklemek alınmaktadır"
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
ms.openlocfilehash: f6a993b3858cfb94dd9795f5e55f15fa6ec7dcb2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-add-an-event-hub-event-source"></a>Bir olay hub'ı olay kaynağı ekleme

Bu öğretici zaman serisi Öngörüler ortamınız için bir olay Hub'ından okuyan bir olay kaynağı eklemek için Azure portalını kullanmayı kapsar.

## <a name="prerequisites"></a>Ön koşullar

Bir Event Hub oluşturdunuz ve olaylar için yazma. Event Hubs hakkında daha fazla bilgi için bkz: <https://azure.microsoft.com/services/event-hubs/>

> [Tüketici grupları] Herhangi bir tüketiciye paylaşılmayan kendi ayrılmış bir tüketici grubundaki her zaman serisi Öngörüler olay kaynağı olmalıdır. Birden çok okuyucular aynı tüketici grubu olaylarından kullanırsa, tüm okuyucular hatalar görmeniz olasıdır. Ayrıca olay hub'ı başına 20 tüketici grupları sınırı yoktur. Ayrıntılar için bkz [Event Hubs Programlama Kılavuzu](../event-hubs/event-hubs-programming-guide.md).

## <a name="choose-an-import-option"></a>Bir içe aktarma seçeneği seçin

Olay kaynağı ayarlarını el ile girilebilir veya kullanılabilen event hubs bir olay hub'ı seçilebilir.
İçinde **alma seçeneği** Seçici, aşağıdaki seçeneklerden birini seçin:

* Olay hub'ı ayarları sağlayın el ile
* Kullanılabilir aboneliklerden olay hub'ı kullanın

### <a name="select-an-available-event-hub"></a>Kullanılabilir bir olay hub'ı seçin

Aşağıdaki tabloda, bir olay kaynağı olarak kullanılabilir bir Event Hub seçerken açıklamasını yeni olay kaynağı sekmesiyle her seçenekte açıklanmaktadır:

| ÖZELLİK ADI | AÇIKLAMA |
| --- | --- |
| Olay kaynağı adı | Olay kaynağı adı. Bu ad zaman serisi Öngörüler ortamınızda benzersiz olması gerekir.
| Kaynak | Seçin **olay hub'ı** bir olay hub'ı olay kaynağı oluşturmak için.
| Abonelik kimliği | Bu olay hub'ının oluşturulduğu abonelik seçin.
| Hizmet veri yolu ad alanı | Olay hub'ı içeren Service Bus ad alanı seçin.
| Olay hub'ı adı | Olay hub'ı adını seçin.
| Olay hub'ı ilke adı | Olay hub'ı yapılandırma sekmesinde oluşturulabilmesi için paylaşılan erişim ilkesi seçin. Her paylaşılan erişim ilkesinin bir adı, ayarlayın ve erişim anahtarları izinleri vardır. Olay kaynağı için paylaşılan erişim ilkesi *gerekir* sahip **okuma** izinleri.
| Olay hub tüketici grubu | Olay Hub'ından olayları okumak için tüketici grubu. Olay kaynağı için ayrılmış bir tüketici grubu kullanmak için önerilir.

### <a name="provide-event-hub-settings-manually"></a>Olay hub'ı ayarları sağlayın el ile

Aşağıdaki tabloda ayarları girerken açıklamasını ile yeni bir olay kaynağı sekmesindeki her bir özellik açıklanmaktadır el ile:

| ÖZELLİK ADI | AÇIKLAMA |
| --- | --- |
| Olay kaynağı adı | Olay kaynağı adı. Bu ad zaman serisi Öngörüler ortamınızda benzersiz olması gerekir.
| Kaynak | Seçin **olay hub'ı** bir olay hub'ı olay kaynağı oluşturmak için.
| Abonelik kimliği | Bu olay hub'ının oluşturulduğu abonelik.
| Kaynak grubu | Bu olay hub'ının oluşturulduğu abonelik.
| Hizmet veri yolu ad alanı | Bir hizmet veri yolu ad alanı, Mesajlaşma varlıkları kümesine ilişkin bir kapsayıcıdır. Yeni bir olay hub'ı oluşturduğunuzda, hizmet veri yolu ad alanı da oluşturmuş olursunuz.
| Olay hub'ı adı | Olay Hub'ınızı adı. Olay hub'ınızı oluşturduğunuzda, ona bir özel ad da vermiş
| Olay hub'ı ilke adı | Paylaşılan Erişim İlkesi olay hub'ı yapılandırma sekmesinde oluşturulabilir. Her paylaşılan erişim ilkesinin bir adı, ayarlayın ve erişim anahtarları izinleri vardır. Olay kaynağı için paylaşılan erişim ilkesi *gerekir* sahip **okuma** izinleri.
| Olay hub'ı İlkesi anahtarı | Hizmet veri yolu ad alanına erişimi kimlik doğrulaması için kullanılan paylaşılan erişim anahtarı. Birincil veya ikincil anahtarı buraya girin.
| Olay hub tüketici grubu | Olay Hub'ından olayları okumak için tüketici grubu. Olay kaynağı için ayrılmış bir tüketici grubu kullanmak için önerilir.

## <a name="next-steps"></a>Sonraki adımlar

1. Ortamınıza veri erişim ilkesine eklemek [tanımla veri erişim ilkeleri](time-series-insights-data-access.md)
1. Ortamınızda erişim [zaman serisi Insights portalı](https://insights.timeseries.azure.com)