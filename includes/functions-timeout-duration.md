---
title: include dosyası
description: include dosyası
services: functions
author: nzthiago
ms.service: azure-functions
ms.topic: include
ms.date: 02/21/2018
ms.author: nzthiago
ms.custom: include file
ms.openlocfilehash: 189683a9e98f161ce537284cc7b0349c94be2bf0
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57410919"
---
## <a name="timeout"></a>İşlev uygulaması zaman aşımı süresi 

Bir işlev uygulaması zaman aşımı süresini functionTimeout özelliği tarafından tanımlanan [host.json](../articles/azure-functions/functions-host-json.md#functiontimeout) proje dosyası. Aşağıdaki tablo, her iki plan için ve her iki çalışma zamanı sürümleri varsayılan ve en yüksek değerleri gösterir:

| Planlama | Çalışma zamanı sürümü | Varsayılan | Maksimum |
|------|---------|---------|---------|
| Tüketim | 1.x | 5 | 10 |
| Tüketim | 2.x | 5 | 10 |
| App Service | 1.x | Sınırsız | Sınırsız |
| App Service | 2.x | 30 | Sınırsız |

> [!NOTE] 
> İşlev uygulaması zaman aşımı ayarı bağımsız olarak 230 saniyedir HTTP ile tetiklenen bir işlev bir isteğe cevap vermesi alabileceği en uzun süreyi. Bu nedenle, [boşta kalma zaman aşımı Azure Load Balancer'ın varsayılan](../articles/app-service/faq-availability-performance-application-issues.md#why-does-my-request-time-out-after-230-seconds). Daha uzun işleme sürelerine kullanmayı [dayanıklı işlevler zaman uyumsuz desen](../articles/azure-functions/durable/durable-functions-concepts.md#async-http) veya [asıl işi ertele ve anlık bir yanıt döndürür](../articles/azure-functions/functions-best-practices.md#avoid-long-running-functions).
