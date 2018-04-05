---
title: Azure Application Insights için fiyatlandırma planı ayrıntıları eski Kurumsal | Microsoft Docs
description: Telemetri birimleri yönetin ve Application Insights maliyetlerini izleyin.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ebd0d843-4780-4ff3-bc68-932aa44185f6
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: mbullwin
ms.openlocfilehash: 6e7591ccf0f21099474a08dda088422c377135f6
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="enterprise-plan-details"></a>Kurumsal plan ayrıntıları

Application Insights iki fiyatlandırma planı vardır. Varsayılan plan adlı [temel](app-insights-pricing.md), tüm kurumsal planda hiçbir ek maliyet ve faturalar öncelikle alınan verilerin hacmi aynı özellikleri içerir. Operations Management Suite kullanıyorsanız, bir düğüm başına sahip plan günlük veri kesintileri birlikte gider ve ardından dahil indirimi alınan veriler için ücretinin kuruluş için tercih.

Bkz: [Application Insights fiyatlandırma sayfası](http://azure.microsoft.com/pricing/details/application-insights/) geçerli fiyatlar para birimi ve bölge için.

## <a name="heres-how-the-enterprise-plan-works"></a>İşte Kurumsal planı nasıl çalışır?

* Telemetri tüm uygulamaların Kurumsal planda gönderiyor düğüm başına ücret ödersiniz.
 * A *düğüm* bir fiziksel veya sanal sunucu makinesi ya da uygulamanızı barındıran bir hizmet olarak Platform rol örneği.
 * Geliştirme makineler, istemci tarayıcıları ve mobil cihazları düğümleri olarak sayılmaz.
 * Uygulamanızı bir web hizmeti ve arka uç çalışan gibi telemetri göndermesine çeşitli bileşenleri varsa bunlar ayrı olarak sayılır.
 * [Canlı ölçümleri akış](app-insights-live-stream.md) veri yok sayılan yaratılır abonelik üzerinden fiyatlandırma için uygulama başına düğüm başına ücretlerinizi. 12 telemetri göndermesini beş düğümleriniz varsa uygulamalar ücret olduğundan için beş düğüm.
* Aylık ücret tırnak içine rağmen yalnızca içinde ve bir düğüm telemetri bir uygulamadan gönderir her saat için ücret ödersiniz. Tırnak işaretli aylık ücret saatlik ücret olduğu / 744 (saat cinsinden 31 gün ay sayısı).
* 200 MB günde bir veri birimi ayırma (saatlik ayrıntı) algılanan her düğüm için verilir. Kullanılmayan veri ayırma bir günden sonraki devredilir değil.
 * Kurumsal fiyatlandırma seçeneği belirlerseniz, her abonelik bir günlük indirimi veri aboneliğin Application Insights kaynaklarına telemetri göndermesini düğüm sayısını temel alır. Bu nedenle tüm günlük verileri gönderme 5 düğümleriniz varsa, bu Abonelikteki tüm Application Insights kaynaklarına uygulanan 1 GB havuza alınmış bir indirimi gerekir. Dahil edilen veri tüm düğümleri arasında paylaşıldığından belirli düğümler diğer düğümlere daha fazla veri gönderiyorsanız önemli değildir. Belirli bir tarihteki Application Insights kaynakları Bu abonelik için günlük veri ayırma dahil daha fazla veri almaya devam ederseniz, GB başına fazla kullanım veri ücretleri uygulanır. 
 * Günlük verileri indirimi saat (UTC kullanarak) gün cinsinden sayı olarak hesaplanır her düğüm tarafından 24 kez 200 MB bölünmüş telemetri gönderiyor. 15 gün içinde 24 saatlik sırasında telemetri göndermesini 4 düğüm varsa, o gün için dahil edilen veri olacak şekilde ((4 x 15) / 24) 200 MB = 500 MB x. Düğümleri o gün 1 GB veri gönderirseniz veri fazlalık için fiyat GB başına 2.30 ABD Doları, ücret 1,15 ABD Doları olacaktır.
 * Planın günlük indirimi için temel seçenek ve kullanılmayan indirimi seçtiniz uygulamalarla paylaşılmayan Kurumsal gelen günlük devredilir değil. 

## <a name="here-are-some-examples-of-determining-distinct-node-count"></a>İşte bazı örnekler ayrı düğüm sayısını belirleme

| Senaryo                               | Günlük toplam düğüm sayısı |
|:---------------------------------------|:----------------:|
| 1 uygulama 3 Azure uygulama hizmeti örnekleri ve 1 sanal sunucu kullanma | 4 |
| aynı abonelikte ve kurumsal planındaki 2 VM ve bu uygulamalar için Application Insights kaynaklar üzerinde çalışan 3 uygulamaları geçerlidir | 2 | 
| 4 uygulamalar aynı abonelikte, uygulamaları Öngörüler kaynaklardır. Her uygulama 16 saatlerde 2 örnekleri ve 4 örnekleri 8 yoğun saatlerde çalışır. | 13.33 | 
| 1 çalışan rolü ve her 2 örneklerini çalıştıran 1 Web rolü ile birlikte bulut Hizmetleri | 4 | 
| 3 örneği çalıştıran her mikro hizmet 50 mikro services çalıştıran 5-node Service Fabric kümesi | 5|

* Application Insights SDK'sı, uygulamanızın kullandığı davranışı sayım kesin düğümü bağlıdır. 
  * SDK sürümlerinde 2.2 ve sonraki sürümlerde, Application Insights [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) veya [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) rapor her uygulama düğümü, örneğin fiziksel sunucu ve VM ana bilgisayar adı olarak barındıracak veya Bulut Hizmetleri söz konusu olduğunda örnek adı.  Tek özel durum uygulamaları kullanarak yalnızca: [.NET Core](https://dotnet.github.io/) ve Application Insights çekirdek, servis talebi yalnızca bir düğüm bildirilir tüm ana bilgisayarlar için ana bilgisayar adı kullanılabilir olmadığından SDK. 
  * SDK ' nın önceki sürümler için [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) yalnızca daha yeni SDK sürümlerini davranır ancak [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) gerçek uygulama konakları sayısından bağımsız olarak yalnızca tek bir düğüme rapor eder. 
  * Uygulamanız, roleInstance özel bir değere ayarlamak için SDK'sı kullanıyorsa, varsayılan olarak aynı değeri düğüm sayısını belirlemek üzere kullanılır. 
  * İstemci makineler veya mobil cihazlardan çalışan bir uygulama ile yeni bir SDK sürümünü kullanıyorsanız, düğüm sayısı (çok sayıda istemci makineleri veya mobil cihazlar) çok büyük bir sayı döndürebilir mümkündür. 
