---
title: Azure Service Fabric uygulama düzeyinde izleme | Microsoft Docs
description: Uygulama ve hizmet düzeyi olayları ve günlükleri izleme ve tanılama Azure Service Fabric kümeleri için kullanılan hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: 613faf5bbc9498b82bc04460d30b2e94c30340db
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60393104"
---
# <a name="application-logging"></a>Uygulama günlüğüne kaydetme

Kodunuzu düzenleme yalnızca kullanıcılarınızla ilgili Öngörüler elde etmek için bir yol, ancak aynı zamanda bir şey yanlış uygulamanızda ve düzeltilmesi için gerekenler tanılamak için olup olmadığını bilmek tek yolu değildir. Teknik olarak, bir hata ayıklayıcı bir üretim hizmetine bağlanmak mümkün olsa da, ortak bir uygulama değildir. Bu nedenle, ölçümlü izleme verilerini ayrıntılı önemlidir.

Bazı ürünler, kodunuzu otomatik olarak izleyin. Bu çözümler iyi çalışabilir ancak el ile izleme neredeyse her zaman iş mantığınızı belirli olması gerekir. Sonunda forensically uygulamada hata ayıklamak için yeterli bilgiye sahip olmalıdır. Service Fabric uygulamaları herhangi bir günlük framework ile izleme eklenmiş. Bu belgede, kodunuzu ve diğerlerine göre bir yaklaşım tercih ne zaman düzenleme için birkaç farklı yaklaşım açıklanmaktadır. 

Bu öneriler kullanma hakkında daha fazla örnek için bkz: [Service Fabric uygulamanızı günlük ekleme](service-fabric-how-to-diagnostics-log.md).

## <a name="application-insights-sdk"></a>Application Insights SDK'sı

Application Insights hazır Service Fabric ile zengin bir tümleştirmesi vardır. Kullanıcıları, yapay ZEKA Service Fabric nuget paketleri eklemek ve verileri ve günlükleri oluşturulan alırlar ve Azure portalında görüntülenebilir toplanır. Ayrıca, kullanıcıların kendi telemetri, tanılama ve uygulamalarını ve Hizmetleri ve bölümlerini olan izleme hata ayıklama için en çok kullanılan eklemek için önerilir. [TelemetryClient](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient?view=azure-dotnet) sınıfı SDK telemetri uygulamalarınızda izlemek için birçok yol sağlar. İzleme ve müşterilerimize öğreticide için uygulamanıza application ınsights ekleme örneği kullanıma [izleme ve tanılama bir .NET uygulaması](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="eventsource"></a>EventSource

Visual Studio'da bir şablondan bir Service Fabric çözümü oluşturduğunuzda bir **EventSource**-türetilmiş sınıf (**ServiceEventSource** veya **ActorEventSource**) oluşturulur . Şablon oluşturulması, uygulamanız veya hizmetiniz için olaylar ekleyin. **EventSource** adı **gerekir** benzersiz olmalı ve varsayılan şablon dizeden Şirketim - adlandırılmamalıdır&lt;çözüm&gt;-&lt;proje &gt;. Birden çok sahip **EventSource** adının aynısını kullanın tanımları çalışma zamanında soruna neden olur. Tanımlanan her olay, benzersiz bir tanımlayıcı olmalıdır. Tanımlayıcı benzersiz değilse, bir çalışma zamanı hatası oluşur. Bazı kuruluşlar ayrı geliştirme takımları arasındaki çakışmaları önleme tanımlayıcıları için değerleri aralığı preassign. Daha fazla bilgi için [Vance'nın blog](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) veya [MSDN belgelerine](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).

## <a name="aspnet-core-logging"></a>ASP.NET Core günlüğe kaydetme

Kodunuzu nasıl izleme dikkatli bir şekilde planlamanız önemlidir. Doğru izleme planı büyük olasılıkla kod tabanınızın destabilizing ve kod reinstrument gerek önlemenize yardımcı olabilir. Riski azaltmak için bir araç kitaplığı gibi seçebilirsiniz [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), Microsoft ASP.NET Core bir parçası değildir. ASP.NET Core sahip bir [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) mevcut kod üzerindeki etkisini en aza indirerek tercih ettiğiniz sağlayıcı ile kullanabileceğiniz arabirimi. Windows ve Linux'ta ASP.NET Core, kod kullanabilirsiniz ve tam .NET Framework, bu nedenle, izleme kodu standartlaştırılmıştır.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamalarınızı ve hizmetlerinizi izleme günlüğü sağlayıcınızdan seçtikten sonra herhangi bir analiz platform gönderilmeden önce toplanacak olayları ve günlükleri gerekir. Hakkında bilgi edinin [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) ve [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) önerilen seçeneklere Azure İzleyici bazıları daha iyi anlamak için.
