---
title: Azure Application Insights için fiyatlandırmayı ve veri hacmini yönetme | Microsoft Docs
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
ms.reviewer: Dale.Koetke
ms.date: 08/11/2018
ms.author: mbullwin
ms.openlocfilehash: a81cb9041b905cfb00183981036116fbc61f376a
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "53000885"
---
# <a name="manage-pricing-and-data-volume-in-application-insights"></a>Application ınsights fiyatlandırma ve veri hacmini yönetme

> [!NOTE]
> Bu makalede, Application Insights veri kullanımını çözümleme açıklar.  İlgili bilgiler için aşağıdaki makalelere göz atın.
> - [Kullanım ve Tahmini maliyetler izleme](../monitoring-and-diagnostics/monitoring-usage-and-estimated-costs.md) çoklu Azure İzleme özelliklerini farklı fiyatlandırma modelleri için tahmini maliyetleri ve kullanım görüntülemeyi açıklar. Ayrıca, uygulamanızın fiyatlandırma modelinin değiştirilmesi nasıl açıklar.

Fiyatlandırma [Azure Application Insights] [ start] uygulama başına veri hacmine dayanır. Her bir Application Insights kaynağı ayrı bir hizmet ücretlendirilir ve Azure aboneliğiniz için fatura katkıda.

Application Insights, iki fiyatlandırma planı içeriyor: temel ve kurumsal. Temel fiyatlandırma planı varsayılan plandır. Bu, ek maliyet olmadan, tüm kurumsal plan özellikleri içerir. Temel plan faturalandırılır öncelikle alınan veri hacmi. 

Kurumsal plan bir düğüm başına ücret vardır ve her düğüm günlük veri kullanım hakkı alır. Kurumsal fiyatlandırma planını, bulunan indirimi alınan veriler için ücretlendirilirsiniz. Operations Management Suite kullanırsanız, Kurumsal plan seçmeniz gerekir. 

Application Insights için fiyatlandırma nasıl çalıştığı hakkında sorularınız varsa soru gönderebilir bizim [Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=ApplicationInsights).

## <a name="pricing-plans"></a>Fiyatlandırma planları

Para birimi ve bölge için geçerli fiyatlarını görmek [Application Insights fiyatlandırması][pricing].

> [!NOTE]
> Nisan 2018'de biz [sunulan](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/) Azure izleme için yeni bir fiyatlandırma modeli. Bu model, hizmetleri izleme tam Portföyü genelinde basit bir "Kullandıkça Öde" modelini devralır. Daha fazla bilgi edinin [yeni fiyatlandırma modeline](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs), nasıl için [bu modeline taşıma etkisini değerlendirmek](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#assessing-the-impact-of-the-new-pricing-model) , kullanım modellerini ve [yeni modele nasıl](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#moving-to-the-new-pricing-model).

### <a name="basic-plan"></a>Temel plan

Temel planda yeni bir Application Insights kaynağı oluşturulduğunda fiyatlandırma planını varsayılandır. Temel plan, bir Operations Management Suite aboneliğine sahip olanlar dışındaki tüm müşteriler için idealdir.

* Temel planda veri hacmine göre ücretlendirilir. Veri hacmi Application Insights tarafından alınan telemetrinin bayt sayısıdır. Veri hacmi, Application Insights tarafından uygulamanızdan alınan sıkıştırılmamış JSON veri paketi boyutu olarak ölçülür. İçin [Analytics'e içe tablo veri](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-import), veri hacmi Application Insights'a gönderilen dosyalarının sıkıştırılmamış boyut olarak ölçülür.
* Uygulamanızın veri hacmi ücreti artık adlı bir yeni fatura ölçümünde üzerinde bildirilen **veri alımı** Nisan 2018'den itibaren. Bu yeni ölçüm, izleme uygulamaları Insights ve Log Analytics gibi teknolojiler arasında paylaşılan ve şu anda hizmet adı altında **uygulama hizmetleri** (ve yakında değiştirerek **Log Analytics**). 
* [Canlı ölçümler Stream](app-insights-live-stream.md) veri amacıyla sayılan değil.
* [Sürekli dışarı aktarma](app-insights-export-telemetry.md) ve [Azure Log Analytics Bağlayıcısı](https://go.microsoft.com/fwlink/?LinkId=833039&amp;clcid=0x409) Nisan 2018'den itibaren temel planda başka bir ücret ödemeden kullanılabilir.

### <a name="enterprise-plan-and-operations-management-suite-subscription-entitlements"></a>Kurumsal plan ve Operations Management Suite aboneliği destek hakları

Operations Management Suite E1 ve E2 satın almış olan müşteriler, ek ücret ödemeden başka bir bileşen olarak Application Insights Kurumsal alabilirsiniz [daha önce duyurulduğu gibi](https://blogs.technet.microsoft.com/msoms/2017/05/19/azure-application-insights-enterprise-as-part-of-operations-management-suite-subscription/). Özellikle, her bir birimi, Operations Management Suite E1 ve E2 Application Insights Kurumsal planına bir düğüm hakkı içerir. Her Application Insights düğüm başına ek ücret ödemeden 90 günlük veri saklama (ayrı), Log Analytics veri alma günlük içe alınan veri 200 MB'a kadar içerir. Plan daha ayrıntılı makalenin sonraki bölümlerinde açıklanmıştır. 

Bu plan yalnızca bir Operations Management Suite aboneliği olan müşteriler için geçerli olduğundan, bir Operations Management Suite aboneliğiniz yoksa müşteriler bu planı seçmek için bir seçenek görmezsiniz.

> [!NOTE]
> Bu yetkilendirme aldığından emin olmak için Application Insights kaynaklarınızı fiyatlandırma planını Kurumsal olması gerekir. Bu yetkilendirme yalnızca düğümleri uygulanır. Application Insights kaynakları temel planda edilmesinden bir fayda farkında değil. Bu yetkilendirme gösterilen tahmini maliyetleri de görünür olmayan **kullanım ve tahmini maliyet** bölmesi. Ayrıca, yeni fiyatlandırma modeli izleme Nisan 2018'de Azure abonelik taşıma, temel plan yalnızca plan kullanılabilir olur. Bir aboneliği yeni fiyatlandırma modeli izleme Azure'a taşıma, bir Operations Management Suite aboneliğiniz varsa önerilir değil.

Kurumsal plan hakkında daha fazla bilgi için bkz. [Kurumsal fiyatlandırma ayrıntıları](app-insights-pricing-enterprise-details.md).

### <a name="multi-step-web-tests"></a>Çok adımlı web testleri

[Çok adımlı web testleri](app-insights-monitor-web-app-availability.md#multi-step-web-tests) ek bir ücret uygulanmaz. Çok adımlı web testleri bir dizi eylem gerçekleştiren web testlerdir.

İçin ayrı ücret *ping testleri* tek bir sayfa. Alınan telemetri çalıştırılabilen ping testleri ve çok adımlı testler aynı diğer uygulamanızdan alınan telemetri olarak ücretlendirilir.

## <a name="review-pricing-plans-and-estimate-costs"></a>Fiyatlandırma planlarını gözden geçirin ve maliyetleri tahmin edin

Application Insights kullanılabilir fiyatlandırma planları anlamak kolaylaştırır ve hangi maliyetlerinizi son kullanım düzenlerini esas alarak muhtemeldir. Azure portalında Application Insights kaynağı için başlamak için Git **kullanım ve Tahmini maliyetler** bölmesi:

![Fiyatlandırma seçin](./media/app-insights-pricing/pricing-001.png)

A. Veri hacminiz ay için gözden geçirin. Bu alındı ve korunan tüm verileri içerir (sonra yerleştirmenizi [örnekleme](app-insights-sampling.md)) sunucusu ve istemci uygulamaları ve kullanılabilirlik testleri.  
B. Ayrı bir ücret yapılan [çok adımlı web testleri](app-insights-monitor-web-app-availability.md#multi-step-web-tests). (Bu veri birimi ücreti dahil olan basit kullanılabilirlik testleri dahil değildir.)  
C. Geçen ay için veri birimi eğilimlerini görüntüleyin.  
D. Veri alımı etkinleştirme [örnekleme](app-insights-sampling.md).   
E. Günlük veri hacmi üst sınırı ayarlayın.  

Application Insights ücretleri Azure faturanızı eklenir. Azure, fatura ayrıntılarını görebilirsiniz **faturalama** bölümü içinde veya Azure portal'ın [Azure faturalama portalını](https://account.windowsazure.com/Subscriptions). 

![Faturalandırma sol taraftaki menüde'yı seçin](./media/app-insights-pricing/02-billing.png)

## <a name="data-rate"></a>Veri oranı
Gönderdiğiniz veri hacmi üç şekilde sınırlıdır:

* **Örnekleme**: örnekleme ölçüm en az bozulma ile sunucu ve istemci uygulamalardan gönderilen telemetri miktarını azaltmak için kullanabilirsiniz. Örnekleme, gönderdiğiniz veri miktarını ayarlamak için kullanabileceğiniz birincil araçtır. Daha fazla bilgi edinin [özellikleri örnekleme](app-insights-sampling.md). 
* **Günlük üst sınır**: Azure portalında bir Application Insights kaynağı oluşturduğunuzda, günlük üst sınır 100 GB/gün olarak ayarlanır. Visual Studio'da Application Insights kaynağı oluşturduğunuzda, varsayılan küçüktür (yalnızca fazla 32,3 MB/gün). Günlük sınır varsayılan test edilmesini kolaylaştırmak için ayarlanır. Uygulamayı üretim ortamına dağıtmadan önce kullanıcı günlük üst sınır oluşturacak yöneliktir. 

    Yüksek trafik uygulama için daha yüksek bir maksimum istek sürece, üst sınır 1000 GB/gün olur. 

    Günlük üst sınır ayarlarken dikkatli olun. Amacınız olmalıdır *günlük üst sınır hiç isabet*. Günlük üst sınır ulaşırsanız, günün geri kalanında için veri kaybı ve uygulamanızı izleyemez. Günlük üst sınırını değiştirmek için kullanın **günlük hacim üst sınırını** seçeneği. Bu seçenekte erişebileceğiniz **kullanım ve Tahmini maliyetler** bölmesi (Bu açıklanan makalenin ilerleyen bölümlerinde daha ayrıntılı).
    Kısıtlama için Application Insights kullanılamadı alacak olan bazı abonelik türleri üzerinde kaldırdık. Daha önce abonelik bir harcama limiti varsa, günlük sınır iletişim 32,3 MB/gün oluşturulması günlük üst sınırını etkinleştirebilir ve harcama sınırını kaldırmak için yönergeleri sahiptir.
* **Azaltma**: veri hızı 32.000 olayları, saniyede ortalama azaltma sınırları izleme anahtarı başına 1 dakika içinde.

*Uygulamamı azaltma oranı aşarsa ne olur?*

* Uygulamanızın gönderdiği veri hacmi dakikada değerlendirilir. Dakika üzerinden ortalaması alınan Saniyedeki oranı aşarsa, sunucunun bazı istekleri reddeder. SDK, verileri arabelleğe alır ve onu yeniden dener. Bu birkaç dakika boyunca aşırı yükleme talebiyle yayar. Uygulamanızı birden çok hızlandırma oranına, verileri tutarlı bir şekilde gönderir, bazı veriler atılır. (ASP.NET, Java ve JavaScript SDK'ları veri bu şekilde yeniden göndermeyi deneyin; diğer SDK'lar yalnızca kısıtlanan bırakma veri olabilir.) Azaltma ortaya çıkarsa, bir bildirim uyarısı sizi bu oluştu sizi uyarır.

*Uygulamamın gönderdiği veri miktarını olduğunu nasıl öğrenebilirim?*

Uygulamanızın gönderdiği veri miktarını görmek için aşağıdaki seçeneklerden birini kullanabilirsiniz:

* Git **kullanım ve tahmini maliyet** bölmesinde günlük verileri toplu grafiği görmek için. 
* Ölçüm Gezgini'nde, yeni bir grafik ekleyin. Grafik ölçüsünü seçin **veri noktası birimi**. Açma **gruplandırma**ve ardından gruplandırma ölçütü **veri türü**.

## <a name="reduce-your-data-rate"></a>Veri hızınızı azaltın
Veri hacminiz azaltmak için yapabileceğiniz bazı işlemler aşağıda verilmiştir:

* Kullanım [örnekleme](app-insights-sampling.md). Bu teknoloji, veri hızı ölçümlerinizi eğriltme olmadan azaltır. Arama ilgili öğeleri arasında gezinme olanağı kaybetmeyin. Sunucu uygulamalarında örnekleme otomatik olarak çalışır.
* [Raporlanabilir Ajax çağrılarının sayısını sınırlamak](app-insights-javascript.md#detailed-configuration) her sayfa görünümü veya Ajax raporlama kapatma düğmesi.
* [Applicationınsights.config dosyasını düzenlemek](app-insights-configuration-with-applicationinsights-config.md) ihtiyacınız olmayan toplama modülleri açmak için. Örneğin, performans sayaçlarını veya bağımlılık verileri gerekli olmayan karar verebilirsiniz.
* Telemetrinizi ayrı bir izleme anahtarı arasında bölün. 
* Önceden toplu ölçümleri. Uygulamanızda TrackMetric çağrıları yerleştirirseniz, ortalama, hesaplama ve standart sapmasını toplu ölçümler kabul eden aşırı yüklemesi kullanarak trafiğini azaltabilir. Veya, kullanabileceğiniz bir [önceden toplama paket](https://www.myget.org/gallery/applicationinsights-sdk-labs).

## <a name="manage-the-maximum-daily-data-volume"></a>Maksimum günlük veri hacmini yönetme

Günlük hacim üst sınırını toplanan verileri sınırlamak için kullanabilirsiniz. Ancak, cap karşılanırsa, uygulamanızdan günün geri kalanı boyunca gönderilen tüm telemetri kaybı oluşur. Bu *değil kapatılacağından* uygulamanız için günlük üst sınır basın. Günlük üst sınır ulaştıktan sonra uygulamanızın performansı ve sistem durumu izleyemezsiniz.

Günlük hacim üst sınırını kullanmak yerine, [örnekleme](app-insights-sampling.md) veri hacmi için istediğiniz düzeyini ayarlamak için. Ardından, çok daha yüksek birimleri telemetri göndermek uygulamanızı beklenmedik bir şekilde başlar durumda günlük üst sınır "son çare" kullanın.

Günlük üst sınırını değiştirmek için **yapılandırma** Application Insights kaynağınıza bölümünü, **kullanım ve Tahmini maliyetler** bölmesinde **günlük üst sınır**.

![Günlük telemetri hacim üst sınırını Ayarla](./media/app-insights-pricing/pricing-003.png)

## <a name="sampling"></a>Örnekleme
[Örnekleme](app-insights-sampling.md) hangi telemetri gönderilir, uygulamanıza bulmasını sırasında tanılama aramaları ilgili olaylar korurken hızının düşürülmesi bir yöntemdir. Ayrıca, doğru olay sayısı de korur.

Örnekleme giderlerini azaltmak ve aylık kota içinde kalmak için etkili bir yoludur. Örnekleme algoritması ilgili telemetri öğelerinin şekilde korur, örneğin, arama kullandığınızda, belirli bir özel durumla ilişkili istek bulabilirsiniz. Ölçüm Gezgini'nde doğru değerleri istek hızları, özel durum oranları ve diğer sayıları görmek için algoritma doğru sayılarını da korur.

Örnekleme birkaç biçimi vardır.

* [Uyarlamalı örnekleme](app-insights-sampling.md) ASP.NET SDK'sı için varsayılandır. Uyarlamalı örnekleme, uygulamanızın gönderdiği telemetri hacmi otomatik olarak ayarlar. Telemetri trafiğini ağda küçülmesini SDK web uygulamanıza otomatik olarak çalışır. 
* *Alım örneklemesi* burada uygulamanızdan alınan telemetri girer Application Insights hizmetine noktada çalışır bir alternatiftir. Alım örneklemesi uygulamanızdan gönderilen telemetri hacmini etkilemez, ancak hizmet tarafından korunan birimin azaltır. Alım örneklemesi alınan telemetri tarayıcılar ve diğer SDK'lar tarafından kullanılan kotayı azaltmak için kullanabilirsiniz.

Alım örneklemesi ayarlamak için Git **fiyatlandırma** bölmesi:

![Kota ve fiyatlandırma bölmesinde, örnekleri kutucuğunu seçin ve ardından bir örnekleme kesir seçin](./media/app-insights-pricing/pricing-004.png)

> [!WARNING]
> **Veri örnekleme** bölmesinde yalnızca alım örneklemesi değerini denetler. Bu, örnekleme hızını uygulamanızda Application Insights SDK'sı tarafından uygulanan yansıtmaz. Gelen telemetri SDK'yı örneklenen, alım örneklemesi uygulanmaz.
>

Uygulanmış, ne olursa olsun gerçek örnekleme oranını bulmak için kullanmak bir [Analytics sorgusu](app-insights-analytics.md). Sorguyu şu şekilde görünür:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h)
    | render areachart

Her kaydı tutulur `itemCount` temsil ettiği özgün kayıt sayısını belirtir. 1 + önceki atılan kayıt sayısı eşittir. 

## <a name="automation"></a>Otomasyon

Azure kaynak Yönetimi'ni kullanarak fiyatını planı ayarlamak için bir betik yazabilirsiniz. [Nasıl olduğunu öğrenin](app-insights-powershell.md#price).

## <a name="limits-summary"></a>Sınırları özeti

[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

## <a name="disable-daily-cap-e-mails"></a>Günlük sınır e-postalar devre dışı bırak

Günlük birim sınırı e-postalar, altında devre dışı bırakmak için **yapılandırma** Application Insights kaynağınıza bölümünü, **kullanım ve Tahmini maliyetler** bölmesinde **günlük üst sınır** . Sınırına ulaşıldığında, ayarlanabilir bir uyarı düzeyine ulaşıldı yanı sıra e-posta göndermek için ayarları vardır. Tüm günlük devre dışı bırakmak istiyorsanız her iki e-posta kutusunun işaretini kaldırın uç birim ilgili.

## <a name="next-steps"></a>Sonraki adımlar

* [Örnekleme](app-insights-sampling.md)

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: app-insights-overview.md
[pricing]: https://azure.microsoft.com/pricing/details/application-insights/