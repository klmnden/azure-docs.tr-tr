---
title: Azure Event Hubs API'sine genel bakış | Microsoft Docs
description: Kullanılabilir Azure olay hub'ları API'larının genel bakış
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: 3f221a0c-182d-4e39-9f3d-3a3c16c5c6ed
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/02/2018
ms.author: shvija
ms.openlocfilehash: 80566b0246179064d2a479b8c9bf3c79a2a93aac
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60822628"
---
# <a name="available-event-hubs-apis"></a>Kullanılabilir olay hub'ları API'leri

Bu makalede, Event Hubs kaynaklarını yönetmek için kullanabileceğiniz var olan API istemcilerin kümesini açıklar.

## <a name="runtime-apis"></a>Çalışma zamanı API'ları

Aşağıdaki bölümde, şu anda kullanılabilir tüm Azure Event Hubs çalışma zamanı istemcileri açıklanmaktadır. Bu kitaplıklar da sınırlı yönetim işlevselliğine bazıları vardır ayrıca [belirli kitaplıkları](#management-apis) yönetim işlemleri için ayrılmış. Bu kitaplıklar temel odak noktası, göndermek ve bir olay hub'ından iletiler alan sağlamaktır.

Her çalışma zamanı kitaplığının geçerli durumuyla ilgili daha fazla bilgi için bkz. [ek bilgi](#additional-information).

| Dil/Platform | İstemci paketi | EventProcessorHost paket | Depo |
| --- | --- | --- | --- |
| .NET Standard | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) | [GitHub](https://github.com/azure/azure-event-hubs-dotnet) |
| .NET Framework | [NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) | Yok |
| Java | [Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) | [Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22) | [GitHub](https://github.com/Azure/azure-event-hubs-java) |
| Node | [NPM](https://www.npmjs.com/package/azure-event-hubs) | Yok | [GitHub](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs) |
| C | Yok | Yok | [GitHub](https://github.com/Azure/azure-event-hubs-c) |

### <a name="additional-information"></a>Ek bilgiler

#### <a name="net"></a>.NET

Olay hub'ları için birden çok .NET kitaplıkları olduklarından .NET ekosisteminin birden çok çalışma zamanları vardır. .NET Standard kitaplığı, yalnızca .NET Framework kitaplığı bir .NET Framework ortamında çalıştırılabilir sırasında .NET Core veya .NET Framework kullanılarak çalıştırılabilir. .NET Framework sürümleri hakkında daha fazla bilgi için bkz. [framework sürümlerini](https://docs.microsoft.com/dotnet/articles/standard/frameworks).

#### <a name="node"></a>Node

[Node.js Kitaplığı](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs) şu anda Önizleme aşamasındadır ve yan projesi olarak Microsoft çalışanları ve harici katkıda bulunanlar tarafından korunur. Kaynak kodu da dahil olmak üzere tüm katkılar kabul edilir ve gözden geçirilir.

## <a name="management-apis"></a>Yönetim API’leri

Aşağıdaki tabloda, şu anda kullanılabilir olan tüm yönetim özgü kitaplıkları listeler. Hiçbiri şu kitaplıkların çalışma zamanı işlemleri içeren ve Event Hubs varlıkları yönetme amacı olan.

| Dil/Platform | Yönetim Paketi | Depo |
| --- | --- | --- |
| .NET Standard | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Management.EventHub) | [GitHub](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/ResourceManagement/EventHub) |

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)
* [Olay Hub’ı oluşturma](event-hubs-create.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
