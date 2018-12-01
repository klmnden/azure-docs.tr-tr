---
title: Azure Application Insights için fiyatlandırma planı ayrıntıları eski Kurumsal | Microsoft Docs
description: Telemetri birimleri yönetme ve Application ınsights'ta maliyetleri izleyin.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ebd0d843-4780-4ff3-bc68-932aa44185f6
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/21/2018
ms.author: mbullwin
ms.openlocfilehash: 820c1baaf920dc2ada2aba1a4a8cc3b11ab2db2d
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2018
ms.locfileid: "52726823"
---
# <a name="enterprise-plan-details"></a>Kurumsal plan ayrıntıları

Azure Application Insights, iki fiyatlandırma planı içeriyor: temel ve kurumsal. [Temel](app-insights-pricing.md) fiyatlandırma planını olan varsayılan planı. Bu, ek maliyet olmadan, tüm kurumsal plan özellikleri içerir. Temel plan faturalandırılır öncelikle alınan veri hacmi. 

Kurumsal plan bir düğüm başına ücret vardır ve her düğüm günlük veri kullanım hakkı alır. Kurumsal fiyatlandırma planını, bulunan indirimi alınan veriler için ücretlendirilirsiniz. Operations Management Suite kullanıyorsanız, Kurumsal plan seçmeniz gerekir. 

Para birimi ve bölge için geçerli fiyatlarını görmek [Application Insights fiyatlandırması](https://azure.microsoft.com/pricing/details/application-insights/).

> [!NOTE]
> Nisan 2018'de biz [sunulan](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/) Azure izleme için yeni bir fiyatlandırma modeli. Bu model, hizmetleri izleme tam Portföyü genelinde basit bir "Kullandıkça Öde" modelini devralır. Daha fazla bilgi edinin [yeni fiyatlandırma modeline](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs), nasıl için [bu modeline taşıma etkisini değerlendirmek](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#assessing-the-impact-of-the-new-pricing-model) , kullanım modellerini ve [yeni modele nasıl](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#moving-to-the-new-pricing-model)

## <a name="how-the-enterprise-plan-works"></a>Kurumsal plan nasıl çalışır?

* Kurumsal plandaki tüm uygulamaları için telemetri gönderen her düğüm için ödeme yaparsınız.
 * A *düğüm* sunucusu fiziksel veya sanal makine veya uygulamanızı barındıran bir hizmet olarak platform rolü örneği.
 * Geliştirme makineler, istemci tarayıcıları ve mobil cihazlar düğüm olarak sayılmaz.
 * Uygulamanızı bir web hizmeti ve bir arka uç çalışan gibi telemetriyi göndermek birkaç bileşeni varsa bileşenleri ayrı olarak sayılır.
 * [Canlı ölçümler Stream](app-insights-live-stream.md) veri amacıyla sayılan değil. Bir abonelikte, uygulama başına değil, düğüm başına ücretlerdir. 12 için telemetri gönderen beş düğüm varsa uygulamalar, beş düğüm için ücretlendirme yapılır.
* Aylık ücret tırnak içinde olsa da, yalnızca içinde ve bir düğümün telemetri bir uygulamadan gönderdiği saat için ücret ödersiniz. Saatlik bir ücret 744 (ayda 31 gün saat sayısı) bölü tırnak işaretli aylık sabit ücrettir.
* 200 MB veri birimi ayrılması günde (saatlik ayrıntı düzeyiyle) algılanan her düğüm için verilir. Kullanılmayan verileri ayırma günden sonraki devreden değil.
 * Kurumsal fiyatlandırma planını seçerseniz, her abonelik bir günlük kullanım hakkı bu abonelikte Application Insights kaynaklara telemetri gönderen düğüm sayısına göre verileri alır. Bu nedenle, tüm gün veri gönderen beş düğümünüz varsa, havuza alınmış bir indirimini bu Abonelikteki tüm Application Insights kaynakları için uygulanan 1 GB gerekir. Dahil edilen veri tüm düğümler arasında paylaşıldığından bazı düğümlerin diğerlerine göre daha fazla veri gönderdiğiniz olup olmaması önemli değildir. Belirli bir gün, Application Insights kaynakları Bu abonelik için günlük veri ayırma dahil edilmiş miktardan daha fazla veri almak, GB başına fazla kullanım veri ücretleri uygulanır. 
 * Günlük veri Kullanım Hakkı (UTC saat kullanarak) bir gündeki saat sayısı hesaplanır her düğüm 200 MB ile çarpılarak 24 bölü telemetri gönderir. Bu nedenle, 15 gün içinde 24 saat boyunca telemetri gönderen dört düğümünüz varsa, o gün için dahil edilen veri olması ((4 &#215; 15) / 24) &#215; 200 MB = 500 MB. Düğümleri o gün 1 GB veri gönderiyorsanız fiyatına 2.30 ABD Doları / GB veri fazla kullanımı için ücret 1,15 ABD Doları olacaktır.
 * Kurumsal plan günlük kullanım hakkı, temel plan tercih ettiğiniz uygulamalarla paylaşılmaz. Kullanılmayan indirimi gelen günlük devreden değil. 

## <a name="examples-of-how-to-determine-distinct-node-count"></a>Ayrı bir düğüm sayısını belirlemek nasıl örnekleri

| Senaryo                               | Günlük toplam düğüm sayısı |
|:---------------------------------------|:----------------:|
| 1 uygulama 3 Azure App Service örneği ve 1 sanal sunucusu kullanma | 4 |
| 2 VM'ler üzerinde çalışan 3 uygulama; Bu uygulamalar için Application Insights kaynaklarını aynı abonelik ve kurumsal plan olan | 2 | 
| aynı abonelikte olan uygulamaları Insights kaynaklarıdır 4 uygulamaları; 16 saatlerde 2 örneğini ve 8 yoğun saatler sırasında 4 örneği çalıştıran her bir uygulama | 13.33 | 
| Bulut hizmetleriyle 1 çalışan rolü ve 2 örneğini çalıştıran her 1 Web rolü | 4 | 
| 50 mikro Hizmetleri çalıştıran 5 düğümlü Azure Service Fabric kümesi; Her mikro hizmet 3 örnek çalışıyor | 5|

* Sayım kesin düğüm üzerinde hangi Application Insights SDK, uygulamanızın kullandığı bağlıdır. 
  * SDK sürümleri 2.2 ve daha sonra Application Insights içinde [Core SDK'sı](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) ve [Web SDK'sı](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) her uygulama konağı bir düğüm olarak bildirin. Fiziksel sunucu ve VM konakları için bilgisayar adı veya örnek adı bulut Hizmetleri için verilebilir.  Tek özel durum yalnızca kullanan bir uygulamadır [.NET Core](https://dotnet.github.io/) ve Application Insights Core SDK'sı. Bu durumda, konak adı kullanılamaz çünkü yalnızca tek bir düğüm için tüm konaklar bildirilir. 
  * SDK ' nın önceki sürümler için [Web SDK'sı](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) yeni SDK sürümleri gibi davranır ancak [Core SDK'sı](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) uygulama ana bilgisayarı sayısından bağımsız olarak yalnızca tek bir düğüme bildirir. 
  * Uygulamanızı ayarlamak için SDK'sı kullanıyorsa **Roleınstance** özel bir değer, düğüm sayısını belirlemek için aynı değeri varsayılan olarak kullanılır. 
  * İstemci makineler veya mobil cihazlardan çalışan bir uygulama ile yeni bir SDK sürümü kullanıyorsanız, düğüm sayısı (çok sayıda istemci makinelere veya mobil cihazları nedeniyle) çok büyük bir sayı döndürebilir. 
