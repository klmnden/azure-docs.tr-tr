---
title: Azure geçişi API'sine genel bakış | Microsoft Docs
description: Kullanılabilir Azure geçişi API'larının genel bakış
services: event-hubs
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: fdaa1d2b-bd80-4e75-abb9-0c3d0773af2d
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/02/2018
ms.author: spelluru
ms.openlocfilehash: 05d7ac56d6c1c48125eb458d0eee852ba396b300
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60593352"
---
# <a name="available-relay-apis"></a>Kullanılabilir geçiş API'leri

## <a name="runtime-apis"></a>Çalışma zamanı API'ları

Aşağıdaki tabloda, şu anda kullanılabilir geçişi çalışma zamanı istemcileri listeler.

[Ek bilgi](#additional-information) bölümü her çalışma zamanı kitaplığının durumu hakkında daha fazla bilgi içerir.

| Dil/Platform | Kullanılabilir özellik | İstemci paketi | Havuz |
| --- | --- | --- | --- |
| .NET Standard | Karma Bağlantılar | [Microsoft.Azure.Relay](https://www.nuget.org/packages/Microsoft.Azure.Relay/) | [GitHub](https://github.com/azure/azure-relay-dotnet) |
| .NET Framework | WCF Geçişi | [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | Yok |
| Düğüm | Karma Bağlantılar | [Websockets: `hyco-ws`](https://www.npmjs.com/package/hyco-ws)<br/>[Websockets: `hyco-websocket`](https://www.npmjs.com/package/hyco-websocket)<br/>[HTTP istekleri: `hyco-https`](https://www.npmjs.com/package/hyco-https) | [GitHub](https://github.com/Azure/azure-relay-node) |

### <a name="additional-information"></a>Ek bilgiler

#### <a name="net"></a>.NET

Birden fazla çalışma zamanı .NET ekosisteminin sahipse, bu nedenle geçiş için birden çok .NET kitaplıkları vardır. .NET Standard kitaplığı, yalnızca .NET Framework kitaplığı bir .NET Framework ortamında çalıştırılabilir sırasında .NET Core veya .NET Framework kullanılarak çalıştırılabilir. .NET Framework hakkında daha fazla bilgi için bkz. [framework sürümlerini](/dotnet/articles/standard/frameworks).

.NET Framework kitaplığı yalnızca WCF programlama modelini destekler ve WCF dayalı özel bir ikili protokolü kullanır `net.tcp` taşıma. Bu protokol ve kitaplık korunduğu için geriye doğru mevcut uygulamalarla uyumluluk.

.NET Standard kitaplığı HTTP ve Websocket'üzerinde ' ı oluşturan karma bağlantılar geçişi açık protokol tanımını temel alır. Kitaplığı bir akış soyutlama Websockets üzerinden ve basit istek-yanıt API hareket yanıtlama HTTP isteklerini destekler. [Web API](https://github.com/Azure/azure-relay-dotnet) örnek karma bağlantılar, web hizmetleri için ASP.NET Core ile tümleştirmek nasıl gösterir.

#### <a name="nodejs"></a>Node.js

Yukarıdaki tabloda listelenen karma bağlantılar modüllerden değiştirin veya mevcut Node.js modüllerini yerel ağ yığını yerine Azure geçişi hizmetini üzerinde dinleyen diğer uygulamaları ile değiştir.

`hyco-https` Modülü amends ve kısmen çekirdek Node.js modüllerini geçersiz kılmalar `http` ve `https`, varolan birçok Node.js modüllerini ve kullanan uygulamaları ile uyumlu bir HTTPS dinleyicisi uygulaması bu çekirdek sağlanması modüller.

`hyco-ws` Ve `hyco-websocket` modülleri popüler düzeltmek `ws` ve `websocket` Node.js, modülleri ve iki modülü, bağlı olan uygulamalar, karma çalışmaya olanak tanıyan alternatif bir dinleyici uygulamaları sağlamak için modüller Geçiş bağlantıları.

Modüller hakkında daha fazla ayrıntı bulunabilir [azure geçişi düğüm](https://github.com/Azure/azure-relay-node) GitHub deposu.

## <a name="next-steps"></a>Sonraki adımlar

Azure geçişi hakkında daha fazla bilgi edinmek için şu bağlantıları ziyaret edin:
* [Azure Geçiş nedir?](relay-what-is-it.md)
* [Geçiş hakkında SSS](relay-faq.md)
