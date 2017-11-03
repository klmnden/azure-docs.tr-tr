---
title: "Azure geçiş API genel bakış | Microsoft Docs"
description: "Kullanılabilir Azure geçiş API'larının genel bakış"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: fdaa1d2b-bd80-4e75-abb9-0c3d0773af2d
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2017
ms.author: sethm
ms.openlocfilehash: 60413bd110fd4021b070f94d6338a50e1b321dea
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="available-relay-apis"></a>Kullanılabilir geçiş API'leri

## <a name="runtime-apis"></a>Çalışma zamanı API'leri

Aşağıdaki tabloda şu anda kullanılabilir tüm geçiş çalışma zamanı istemcileri listelenmektedir.

[Ek bilgi](#additional-information) bölüm her çalışma zamanı kitaplığı durumu hakkında daha fazla bilgi içerir.

| Dil/Platform | Kullanılabilir özelliği | İstemci paketi | Depo |
| --- | --- | --- | --- |
| .NET standart | Karma Bağlantılar | [Microsoft.Azure.Relay](https://www.nuget.org/packages/Microsoft.Azure.Relay/) | [GitHub](https://github.com/azure/azure-relay-dotnet) |
| .NET framework | WCF Geçişi | [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | Yok |
| Node | Karma Bağlantılar | [`hyco-ws`](https://www.npmjs.com/package/hyco-ws)<br/>[`hyco-websocket`](https://www.npmjs.com/package/hyco-websocket) | [GitHub](https://github.com/Azure/azure-relay-node) |

### <a name="additional-information"></a>Ek bilgiler

#### <a name="net"></a>.NET
Birden çok çalışma zamanları .NET ekosistemi vardır, bu nedenle olay hub'ları için birden çok .NET kitaplıklarına vardır. .NET standart kitaplığı, .NET Framework kitaplığı yalnızca bir .NET Framework ortamında çalıştırılabilir .NET Core veya .NET Framework kullanarak çalıştırabilirsiniz. .NET Framework ile ilgili daha fazla bilgi için bkz: [framework sürümlerini](/dotnet/articles/standard/frameworks#framework-versions).

## <a name="next-steps"></a>Sonraki adımlar
Azure geçişi hakkında daha fazla bilgi için bu bağlantıları ziyaret edin:
* [Azure Geçiş nedir?](relay-what-is-it.md)
* [Geçiş hakkında SSS](relay-faq.md)