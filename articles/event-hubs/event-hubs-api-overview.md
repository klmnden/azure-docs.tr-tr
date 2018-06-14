---
title: Azure olay hub'ları API genel bakış | Microsoft Docs
description: Kullanılabilir Azure olay hub'ları API'larının genel bakış
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: 3f221a0c-182d-4e39-9f3d-3a3c16c5c6ed
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2017
ms.author: sethm
ms.openlocfilehash: abd44fd0c9cbfab2365b1552e3cd90e84a5348d7
ms.sourcegitcommit: 651a6fa44431814a42407ef0df49ca0159db5b02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
ms.locfileid: "25957536"
---
# <a name="available-event-hubs-apis"></a>Kullanılabilir olay hub'ları API'leri

Bu makalede olay hub'ları kaynakları yönetmek için kullanabileceğiniz API istemciler tarafından kullanılabilmesini açıklar.

## <a name="runtime-apis"></a>Çalışma zamanı API'leri

Şu anda kullanılabilir tüm Azure Event Hubs çalışma zamanı istemcileri bir açıklaması verilmiştir. Bu kitaplıklar de sınırlı yönetim işlevselliğine bazıları olsa da vardır [belirli kitaplıkları](#management-apis) yönetimi işlemleri için ayrılmış. Bir olay hub'ından ileti alıp göndermek için bu kitaplıkların çekirdek odak noktasıdır.

Bkz: [ek bilgi](#additional-information) her çalışma zamanı kitaplığı geçerli durumu hakkında daha fazla ayrıntı için.

| Dil/Platform | İstemci paketi | EventProcessorHost paketi | Havuz |
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

| Dil/Platform | Yönetim Paketi | Havuz |
| --- | --- | --- | --- |
| .NET standart | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Management.EventHub) | [GitHub](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/ResourceManagement/EventHub) |

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)
* [Olay Hub’ı oluşturma](event-hubs-create.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)