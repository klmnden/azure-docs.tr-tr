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
ms.topic: conceptual
ms.date: 06/21/2018
ms.author: mbullwin
ms.openlocfilehash: 65307eab0bf1b5f502f11c14c369826cd12e0966
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309837"
---
# <a name="enterprise-plan-details"></a>Kurumsal plan ayrıntıları

Azure Application Insights sahip iki fiyatlandırma planı: Basic ve Enterprise. [Temel](app-insights-pricing.md) planı fiyatlandırma olan varsayılan planı. Hiçbir ek ücret ödemeden tüm kurumsal plan özellikleri içerir. Temel plan faturaları alınan veri biriminde öncelikle. 

Bir düğüm başına ücret Kurumsal planına sahip ve her düğüm bir günlük veri indirimi alır. Kuruluşta planı, fiyatlandırma, dahil indirimi alınan veriler için sizden ücret kesilir. Operations Management Suite kullanıyorsanız, Kurumsal plan seçmeniz gerekir. 

Para birimi ve bölge geçerli fiyatlar için bkz: [Application Insights fiyatlandırma](http://azure.microsoft.com/pricing/details/application-insights/).

> [!NOTE]
> Nisan 2018, biz [sunulan](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/) Azure izlemek için yeni bir fiyatlandırma modeli. Bu model Hizmetleri izleme tam Portföy arasında basit bir "Kullandıkça Öde" modelinin devralır. Daha fazla bilgi edinmek [yeni fiyatlandırma modeli](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs), nasıl için [bu modeline taşıma etkisini değerlendirmenize](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#assessing-the-impact-of-the-new-pricing-model) , kullanım düzenlerini esas alarak ve [yeni modeline kabul etme](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#moving-to-the-new-pricing-model)

## <a name="how-the-enterprise-plan-works"></a>Kurumsal planı nasıl çalışır?

* Kurumsal planda tüm uygulamalar için telemetri gönderen her düğüm için ücret ödersiniz.
 * A *düğüm* bir fiziksel veya sanal sunucu makinesi ya da uygulamanızı barındıran bir hizmet olarak platform rol örneği.
 * Geliştirme makineler, istemci tarayıcıları ve mobil cihazları düğümleri olarak sayılmaz.
 * Uygulamanızı bir web hizmeti ve arka uç çalışan gibi telemetri göndermesine çeşitli bileşenleri varsa bileşenleri ayrı olarak sayılır.
 * [Canlı ölçümleri akış](app-insights-live-stream.md) veri yok sayılan amacıyla fiyatlandırma için. Bir abonelikte, uygulama başına düğüm başına ücretlerinizi şunlardır. 12 telemetri göndermesine beş düğüm varsa uygulamalar, ücret olduğu için beş düğüm.
* Aylık ücret tırnak içine rağmen yalnızca içinde ve bir düğüm telemetri bir uygulamadan gönderir her saat için ücret ödersiniz. Saatlik ücret 744 (saat cinsinden 31 gün ay sayısı) bölü tırnak işaretli aylık ücret ' dir.
* 200 MB günde bir veri birimi ayırma (saatlik ayrıntı) algılanan her bir düğüm için verilir. Kullanılmayan veri ayırma bir günden sonraki devreden değil.
 * Planı fiyatlandırma Kurumsal seçerseniz, her abonelik bir günlük indirimi veri aboneliğin Application Insights kaynaklarına telemetri göndermesine düğüm sayısını temel alır. Bu nedenle, tüm gün verileri gönder beş düğüm varsa, bu Abonelikteki tüm Application Insights kaynaklarına uygulanan 1 GB havuza alınmış bir indirimi sahip olacaksınız. Dahil edilen veri tüm düğümleri arasında paylaşıldığından belirli düğümler diğer düğümlere daha fazla veri gönderirseniz, önemli değildir. Belirli bir tarihteki Application Insights kaynakları Bu abonelik için günlük veri ayırma dahil daha fazla veri almaya devam ederseniz, GB başına fazla kullanım veri ücretleri uygulanır. 
 * Günlük verileri indirimi saat (UTC kullanarak) gün cinsinden sayı olarak hesaplanır her düğüm tarafından 200 MB çarpılan 24 bölü telemetri gönderir. Bu nedenle, günün 24 saatlik 15 sırasında telemetri göndermesine dört düğüm varsa, o gün için dahil edilen veri olması ((4 &#215; 15) / 24) &#215; 200 MB = 500 MB. Düğümleri o gün 1 GB veri gönderirseniz veri fazlalık için fiyat GB başına 2.30 ABD Doları, ücret 1,15 ABD Doları olacaktır.
 * Kurumsal plan günlük indirimi temel plan seçtiğiniz uygulamalarla paylaşılmaz. Kullanılmayan indirimi gelen günlük devreden değil. 

## <a name="examples-of-how-to-determine-distinct-node-count"></a>Ayrı düğüm sayısını belirlemek nasıl örnekleri

| Senaryo                               | Günlük toplam düğüm sayısı |
|:---------------------------------------|:----------------:|
| 1 uygulama 3 Azure uygulama hizmeti örnekleri ve 1 sanal sunucu kullanma | 4 |
| 2 VM'ler üzerinde çalıştırılan 3 uygulamaları; Bu uygulamalar için Application Insights kaynak aynı abonelikte ve kurumsal planı yok | 2 | 
| 4 uygulamaları aynı abonelikte, uygulamaları Öngörüler kaynaklardır; 16 saatlerde 2 örnekleri ve 8 yoğun saatlerde 4 örneği çalıştıran her bir uygulama | 13.33 | 
| 1 çalışan rolü ve her 2 örneklerini çalıştıran 1 Web rolü ile birlikte bulut Hizmetleri | 4 | 
| 50 mikro çalıştıran 5 düğümlü Azure Service Fabric kümesi; 3 örneği çalıştıran her mikro hizmet | 5|

* Sayım kesin düğümü, uygulamanızın hangi Application Insights SDK kullanarak bağlıdır. 
  * SDK sürüm 2.2 ve daha sonra Application Insights [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) ve [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) her uygulama ana bilgisayarı bir düğüm olarak bildirin. Örnek fiziksel sunucu ve VM ana bilgisayar adı veya örnek adı bulut Hizmetleri için verilebilir.  Tek özel durum yalnızca kullanan bir uygulama olan [.NET Core](https://dotnet.github.io/) ve Application Insights çekirdek SDK'sı. Bu durumda, ana bilgisayar adı kullanılamadığı için yalnızca bir düğüm tüm ana bilgisayarlar için bildirilir. 
  * SDK ' nın önceki sürümler için [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) yeni SDK sürümleri gibi davranır ancak [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) uygulama konakları sayısından bağımsız olarak yalnızca tek bir düğüme bildirir. 
  * Uygulamanız ayarlamak için SDK'sı kullanıyorsa **roleInstance** özel bir değer, düğüm sayısını belirlemek için aynı değeri varsayılan olarak kullanılır. 
  * İstemci makineler veya mobil cihazlardan çalışan bir uygulama ile yeni bir SDK sürümünü kullanıyorsanız, düğüm sayısı (çok sayıda istemci makineleri veya mobil cihazlar nedeniyle) çok büyük bir sayı döndürebilir. 
