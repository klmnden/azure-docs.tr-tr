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
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/20/2018
ms.author: dekapur
ms.openlocfilehash: 258aac722aa1c94ecf2cbf0524a3e4b53b8a788c
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="application-and-service-level-logging"></a>Uygulama ve hizmet düzeyinde günlüğe kaydetme

Kod işaretleme çoğu diğer yönlerini hizmetlerinizi izleme temelidir. İzleme bildiğiniz bir şeylerin yanlış olduğunu tek yoludur ve neyi düzeltilmesi gerektiğini tanılamak için. Teknik olarak, bir hata ayıklayıcısı bir üretim hizmetine bağlanmak mümkün olsa da, ortak bir uygulama değildir. Bu nedenle, izleme verileri ayrıntılı önemlidir.

Bazı ürünler kodunuzu otomatik olarak işaretleme. Bu çözüm de olsa da, el ile araçları neredeyse her zaman gereklidir. Sonunda forensically uygulamada hata ayıklama için yeterli bilgiye sahip olmalıdır. Bu belgede, kodunuz ve ne zaman bir yaklaşım başka bir ad seçin düzenleme için farklı yaklaşımlara açıklanmaktadır.

## <a name="eventsource"></a>EventSource

Visual Studio'da bir şablondan bir Service Fabric çözüm oluşturduğunuzda bir **EventSource**-türetilen sınıfı (**ServiceEventSource** veya **ActorEventSource**) oluşturulur. Bir şablon oluşturduğunuzu, uygulama veya hizmet için olayları ekleyebilirsiniz. **EventSource** adı **gerekir** benzersiz olmalı ve varsayılan şablon dizeden Şirketim - adlandırılmamalıdır&lt;çözüm&gt;-&lt;proje&gt;. Birden çok sahip **EventSource** aynı adı kullanan tanımları çalışma zamanında bir sorunu neden olur. Her tanımlanan olay benzersiz bir tanımlayıcı olması gerekir. Tanımlayıcı benzersiz değilse, bir çalışma zamanı hatası oluşur. Bazı kuruluşlar ayrı geliştirme ekipleri arasındaki çakışmaları önleme tanımlayıcıları için değerlerin aralıkları preassign. Daha fazla bilgi için bkz: [Vance'nın blogu](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) veya [MSDN belgelerine](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).

## <a name="aspnet-core-logging"></a>ASP.NET oturum çekirdek

Kodunuzu nasıl izleme dikkatle planlamanız önemlidir. Sağ araçları planı büyük olasılıkla kod temeliniz destabilizing ve kod reinstrument gerek önlemenize yardımcı olabilir. Riskini azaltmak için bir araç kitaplığı gibi seçebilirsiniz [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), Microsoft ASP.NET Core parçası olduğu. ASP.NET Core sahip bir [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) var olan kodu üzerindeki etkisini en aza indirerek tercih ettiğiniz sağlayıcısı ile birlikte kullanabileceğiniz arabirimi. Windows ve Linux üzerinde ASP.NET Core kodu kullanabilirsiniz ve tam .NET Framework, bu nedenle araçları kodunuzu standartlaştırılmıştır.

## <a name="choosing-a-logging-provider"></a>Oturum açma sağlayıcısı seçme

Uygulamanızı yüksek performans üzerinde dayalıysa **EventSource** genellikle iyi bir yaklaşımdır. **EventSource** *genellikle* daha az kaynak kullanır ve ASP.NET Core günlüğü ya da herhangi bir kullanılabilir üçüncü taraf çözümleri daha iyi gerçekleştirir.  Bu hizmet performans kullanarak dayalı olup olmadığını ancak birçok Hizmetleri için bir sorun değildir **EventSource** daha iyi bir seçim olabilir. Ancak, bu yararları almak için günlüğe kaydetme, yapılandırılmış **EventSource** mühendislik ekibi daha büyük bir yatırım gerektirir. Mümkünse, birkaç günlüğe kaydetme seçeneklerini hızlı prototipi yapın ve ardından ihtiyaçlarınıza en uygun olanı seçin.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamalar ve hizmetler işaretlemesini, oturum açma sağlayıcısı seçtikten sonra herhangi bir çözümleme platform gönderilmeden önce toplanacak günlüklerini ve olayları gerekir. Hakkında bilgi edinin [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) ve [WAD](service-fabric-diagnostics-event-aggregation-wad.md) önerilen seçeneklerden bazıları daha iyi anlamak için.
