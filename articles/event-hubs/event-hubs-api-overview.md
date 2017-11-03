---
title: "Azure olay hub'ları API genel bakış | Microsoft Docs"
description: "Kullanılabilir Azure olay hub'ları API'larının genel bakış"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 3f221a0c-182d-4e39-9f3d-3a3c16c5c6ed
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: 40cd76e1aacb68d6051cae4a3c90a8970f5449f0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="available-event-hubs-apis"></a>Kullanılabilir olay hub'ları API'leri

## <a name="runtime-apis"></a>Çalışma zamanı API'leri

Şu anda kullanılabilir tüm Azure Event Hubs çalışma zamanı istemcileri bir açıklaması verilmiştir. Bu kitaplıklar de sınırlı yönetim işlevselliğine bazıları olsa da vardır [belirli kitaplıkları](#management-apis) yönetimi işlemleri için ayrılmış. Bir olay hub'ından ileti alıp göndermek için bu kitaplıkların çekirdek odak noktasıdır.

Bkz: [ek bilgi](#additional-information) her çalışma zamanı kitaplığı geçerli durumu hakkında daha fazla ayrıntı için.

| Dil/Platform | İstemci paketi | EventProcessorHost paketi | Depo |
| --- | --- | --- | --- |
| .NET standart | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) | [GitHub](https://github.com/azure/azure-event-hubs-dotnet) |
| .NET framework | [NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) | Yok |
| Java | [Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) | [Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22) | [GitHub](https://github.com/Azure/azure-event-hubs-java) |
| Node | [NPM](https://www.npmjs.com/package/azure-event-hubs) | Yok | [GitHub](https://github.com/Azure/azure-event-hubs-node) |
| C | Yok | Yok | [GitHub](https://github.com/Azure/azure-event-hubs-c) |

### <a name="additional-information"></a>Ek bilgiler

#### <a name="net"></a>.NET
Birden çok çalışma zamanları .NET ekosistemi vardır, bu nedenle olay hub'ları için birden çok .NET kitaplıklarına vardır. .NET standart kitaplığı, .NET Framework kitaplığı yalnızca bir .NET Framework ortamında çalıştırılabilir .NET Core veya .NET Framework kullanarak çalıştırabilirsiniz. .NET Framework ile ilgili daha fazla bilgi için bkz: [framework sürümlerini](https://docs.microsoft.com/dotnet/articles/standard/frameworks#framework-versions).

#### <a name="node"></a>Node

Node.js kitaplığı şu anda önizlemede ve yan projesi olarak Microsoft çalışanlarına ve dış katkıda bulunanlar tarafından korunur. Kaynak kodu da dahil olmak üzere tüm katılımlar Hoş Geldiniz ve incelenecektir.

## <a name="management-apis"></a>Yönetim API'leri

Tüm şu anda kullanılabilir yönetim belirli kitaplıkları bir listesi verilmiştir. Bu kitaplıklar hiçbiri çalışma zamanı işlemleri içerir ve Event Hubs varlıkları yönetme tek amacı olan.

| Dil/Platform | Yönetim Paketi | Depo |
| --- | --- | --- | --- |
| .NET standart | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Management.EventHub) | [GitHub](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/ResourceManagement/EventHub) |

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)
* [Olay Hub’ı oluşturma](event-hubs-create.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)