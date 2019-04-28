---
title: Azure işlevleri için Azure Event Hubs bağlamaları
description: Azure işlevleri'nde Azure Event Hubs bağlamaları kullanma hakkında bilgi edinin.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
keywords: Azure işlevleri, İşlevler, olay işleme dinamik işlem, sunucusuz mimari
ms.assetid: daf81798-7acc-419a-bc32-b5a41c6db56b
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
origin.date: 11/08/2017
ms.date: 03/20/2019
ms.author: v-junlch
ms.openlocfilehash: 7ca3ab8e3becf884fdba460f1981430e0c66ef99
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62104467"
---
# <a name="azure-event-hubs-bindings-for-azure-functions"></a>Azure işlevleri için Azure Event Hubs bağlamaları

Bu makalede ile nasıl çalışılacağı açıklanmaktadır [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) bağlamalar için Azure işlevleri. Tetikleme ve çıkış bağlamaları Event Hubs için Azure işlevleri destekler.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paketler - 1.x işlevleri

Azure işlevleri sürüm için 1.x, Event Hubs bağlamaları altında sağlanmıştır [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus) NuGet paketi sürüm 2.x.
Paket için kaynak kodu konusu [azure webjobs sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs) GitHub deposu.


[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Paketler - 2.x işlevleri

İşlevler için kullanım 2.x [Microsoft.Azure.WebJobs.Extensions.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs) paketi, sürüm 3.x.
Paket için kaynak kodu konusu [azure webjobs sdk](https://github.com/Azure/azure-webjobs-sdk/tree/master/src/Microsoft.Azure.WebJobs.Extensions.EventHubs) GitHub deposu.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

[!INCLUDE [functions-bindings-event-hubs](../../includes/functions-bindings-event-hubs.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)

<!-- Update_Description: wording update -->