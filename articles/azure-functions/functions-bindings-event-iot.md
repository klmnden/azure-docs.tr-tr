---
title: Azure işlevleri için Azure IOT hub'ı bağlamaları
description: Azure işlevleri'nde IOT hub'ı bağlamaları kullanma hakkında bilgi edinin.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
keywords: Azure işlevleri, İşlevler, olay işleme dinamik işlem, sunucusuz mimari
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 03/05/2019
ms.author: cshoe
ms.openlocfilehash: 87a99651d17e9867130024a5f721c5488cd42a8c
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59426112"
---
# <a name="azure-iot-hub-bindings-for-azure-functions"></a>Azure işlevleri için Azure IOT hub'ı bağlamaları

Bu makalede, Azure işlevleri bağlamaları için IOT Hub ile nasıl çalışılacağı açıklanmaktadır. IOT hub'ı destek dayanır [Azure Event Hubs bağlama](functions-bindings-event-hubs.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paketler - 1.x işlevleri

Azure işlevleri sürüm için 1.x, IOT hub'ı bağlamaları altında sağlanmıştır [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus) NuGet paketi sürüm 2.x. Paket için kaynak kodu konusu [azure webjobs sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs) GitHub deposu.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Paketler - 2.x işlevleri

İşlevler için kullanım 2.x [Microsoft.Azure.WebJobs.Extensions.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs) paketi, sürüm 3.x. Paket için kaynak kodu konusu [azure webjobs sdk](https://github.com/Azure/azure-webjobs-sdk/tree/master/src/Microsoft.Azure.WebJobs.Extensions.EventHubs) GitHub deposu.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

> [!IMPORTANT]
> Aşağıdaki kod örnekleri, olay hub'ı API kullanırken, belirli bir söz dizimi, IOT hub'ı işlevleri için geçerlidir.

[!INCLUDE [functions-bindings-event-hubs](../../includes/functions-bindings-event-hubs.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
