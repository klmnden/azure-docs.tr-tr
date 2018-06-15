---
title: Azure Service Fabric uygulama düzeyinde izleme | Microsoft Docs
description: Uygulama ve hizmet düzeyi olayları ve günlükleri izlemek ve Azure Service Fabric kümeleri tanılamak için kullanılan hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/20/2018
ms.author: dekapur
ms.openlocfilehash: b8118d83e2be452c6aa5bbc8b7a3c220d26903a1
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34204340"
---
# <a name="application-and-service-level-logging"></a>Uygulama ve hizmet düzeyinde günlüğe kaydetme

Kod işaretleme çoğu diğer yönlerini hizmetlerinizi izleme temelidir. İzleme bildiğiniz bir şeylerin yanlış olduğunu tek yoludur ve neyi düzeltilmesi gerektiğini tanılamak için. Teknik olarak, bir hata ayıklayıcısı bir üretim hizmetine bağlanmak mümkün olsa da, ortak bir uygulama değildir. Bu nedenle, izleme verileri ayrıntılı önemlidir.

Bazı ürünler kodunuzu otomatik olarak işaretleme. Bu çözüm de olsa da, el ile araçları neredeyse her zaman gereklidir. Sonunda forensically uygulamada hata ayıklama için yeterli bilgiye sahip olmalıdır. Bu belgede, kodunuz ve ne zaman bir yaklaşım başka bir ad seçin düzenleme için farklı yaklaşımlara açıklanmaktadır.

Bu öneriler kullanma hakkında daha fazla örnekler için bkz: [günlüğü Service Fabric uygulamanızı ekleme](service-fabric-how-to-diagnostics-log.md).

## <a name="eventsource"></a>EventSource

Visual Studio'da bir şablondan bir Service Fabric çözüm oluşturduğunuzda bir **EventSource**-türetilen sınıfı (**ServiceEventSource** veya **ActorEventSource**) oluşturulur. Bir şablon oluşturduğunuzu, uygulama veya hizmet için olayları ekleyebilirsiniz. **EventSource** adı **gerekir** benzersiz olmalı ve varsayılan şablon dizeden Şirketim - adlandırılmamalıdır&lt;çözüm&gt;-&lt;proje&gt;. Birden çok sahip **EventSource** aynı adı kullanan tanımları çalışma zamanında bir sorunu neden olur. Her tanımlanan olay benzersiz bir tanımlayıcı olması gerekir. Tanımlayıcı benzersiz değilse, bir çalışma zamanı hatası oluşur. Bazı kuruluşlar ayrı geliştirme ekipleri arasındaki çakışmaları önleme tanımlayıcıları için değerlerin aralıkları preassign. Daha fazla bilgi için bkz: [Vance'nın blogu](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) veya [MSDN belgelerine](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).

## <a name="aspnet-core-logging"></a>ASP.NET oturum çekirdek

Kodunuzu nasıl izleme dikkatle planlamanız önemlidir. Sağ araçları planı büyük olasılıkla kod temeliniz destabilizing ve kod reinstrument gerek önlemenize yardımcı olabilir. Riskini azaltmak için bir araç kitaplığı gibi seçebilirsiniz [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), Microsoft ASP.NET Core parçası olduğu. ASP.NET Core sahip bir [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) var olan kodu üzerindeki etkisini en aza indirerek tercih ettiğiniz sağlayıcısı ile birlikte kullanabileceğiniz arabirimi. Windows ve Linux üzerinde ASP.NET Core kodu kullanabilirsiniz ve tam .NET Framework, bu nedenle araçları kodunuzu standartlaştırılmıştır.

## <a name="application-insights-sdk"></a>Application Insights SDK'sı

Application Insights kutu dışı Service Fabric ile zengin bir tümleştirme vardır. Kullanıcıların AI Service Fabric nuget paketleri ekleyebilir ve veri ve oluşturulan günlükleri alırsınız ve Azure Portalı'nda görüntülenebilir toplanır. Ayrıca, kullanıcılar, uygulamaları ve Hizmetleri ve bunların uygulama bölümleri olan izleme hata ayıklama ve tanılamak için kendi telemetri en kullanılan eklemek için önerilir. [TelemetryClient](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient?view=azure-dotnet) sınıfı SDK telemetri uygulamalarınızda izlemek için birçok yol sağlar. İzleme ve öğreticimizi için uygulamanızda application ınsights ekleme konusunda bir örnek kullanıma [izleme ve .NET uygulama tanılama](service-fabric-tutorial-monitoring-aspnet.md)


## <a name="next-steps"></a>Sonraki adımlar

Uygulamalar ve hizmetler işaretlemesini, oturum açma sağlayıcısı seçtikten sonra herhangi bir çözümleme platform gönderilmeden önce toplanacak günlüklerini ve olayları gerekir. Hakkında bilgi edinin [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md), [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md), ve [WAD](service-fabric-diagnostics-event-aggregation-wad.md) önerilen seçeneklerden bazıları daha iyi anlamak için.
