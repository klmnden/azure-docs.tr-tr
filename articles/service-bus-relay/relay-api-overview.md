---
title: Azure geçiş API genel bakış | Microsoft Docs
description: Kullanılabilir Azure geçiş API'larının genel bakış
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: fdaa1d2b-bd80-4e75-abb9-0c3d0773af2d
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/02/2018
ms.author: sethm
ms.openlocfilehash: 00496ca6c0138a840322c053d7d20944df228e9f
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
ms.locfileid: "33893459"
---
# <a name="available-relay-apis"></a>Kullanılabilir geçiş API'leri

## <a name="runtime-apis"></a>Çalışma zamanı API'leri

Aşağıdaki tabloda şu anda kullanılabilir tüm geçiş çalışma zamanı istemcileri listelenmektedir.

[Ek bilgi](#additional-information) bölüm her çalışma zamanı kitaplığı durumu hakkında daha fazla bilgi içerir.

| Dil/Platform | Kullanılabilir özelliği | İstemci paketi | Havuz |
| --- | --- | --- | --- |
| .NET standart | Karma Bağlantılar | [Microsoft.Azure.Relay](https://www.nuget.org/packages/Microsoft.Azure.Relay/) | [GitHub](https://github.com/azure/azure-relay-dotnet) |
| .NET Framework | WCF Geçişi | [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | Yok |
| Node | Karma Bağlantılar | [Websockets: `hyco-ws`](https://www.npmjs.com/package/hyco-ws)<br/>[Websockets: `hyco-websocket`](https://www.npmjs.com/package/hyco-websocket)<br/>[HTTP istekleri: `hyco-https`](https://www.npmjs.com/package/hyco-https) | [GitHub](https://github.com/Azure/azure-relay-node) |

### <a name="additional-information"></a>Ek bilgiler

#### <a name="net"></a>.NET

Birden çok çalışma zamanları .NET ekosistemi vardır, bu nedenle geçişi için birden çok .NET kitaplıkları vardır. .NET standart kitaplığı, .NET Framework kitaplığı yalnızca bir .NET Framework ortamında çalıştırılabilir .NET Core veya .NET Framework kullanarak çalıştırabilirsiniz. .NET Framework ile ilgili daha fazla bilgi için bkz: [framework sürümlerini](/dotnet/articles/standard/frameworks#framework-versions).

.NET Framework kitaplığı yalnızca WCF programlama modelini destekler ve WCF dayalı özel bir ikili Protokolü dayanır `net.tcp` taşıma. Bu protokol ve kitaplık korunduğu için geriye dönük uyumluluk mevcut uygulamalarla.

.NET standart kitaplığı, HTTP ve WebSockets yapıları karma bağlantıları geçişi için açık protokol tanımı dayanır. Kitaplık yanıtlama HTTP istekleri için bir akış soyutlama Websockets üzerinden ve basit istek-yanıt API hareketi destekler. [Web API](https://github.com/Azure/azure-relay-dotnet) örnek karma bağlantılar, web hizmetleri için ASP.NET Core ile tümleştirmek nasıl gösterir.

#### <a name="nodejs"></a>Node.js

Yukarıdaki tabloda listelenen karma bağlantılar modülleri değiştirin veya varolan Node.js modüllerini yerel ağ yığınını yerine Azure geçiş hizmeti üzerinde dinleme alternatif uygulamalarıyla birlikte düzeltmek.

`hyco-https` Modülü amends ve kısmen çekirdek Node.js modüllerini geçersiz kılmaları `http` ve `https`, var olan birçok Node.js modüllerini ve kullanan uygulamalar ile uyumlu bir HTTPS dinleyicisi uygulaması bu çekirdek sağlanması modüller.

`hyco-ws` Ve `hyco-websocket` modülleri düzeltmek popüler `ws` ve `websocket` modülleri Node.js, modüller ve her iki modülü, bağlı olan uygulamaların karma çalışması için etkinleştirmek diğer dinleyicisi uygulamaları sağlamak için Bağlantıları geçiş.

Bu modüller hakkında ayrıntılar bulunabilir [azure Geçiş düğümü](https://github.com/Azure/azure-relay-node) GitHub depo.

## <a name="next-steps"></a>Sonraki adımlar

Azure geçişi hakkında daha fazla bilgi için bu bağlantıları ziyaret edin:
* [Azure Geçiş nedir?](relay-what-is-it.md)
* [Geçiş hakkında SSS](relay-faq.md)