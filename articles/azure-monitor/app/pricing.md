---
title: Azure Application Insights için kullanımı ve maliyetleri yönetme | Microsoft Docs
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
ms.date: 12/21/2018
ms.author: mbullwin
ms.openlocfilehash: edf724d6fd659ad4e8887a9c68467d17a33f5ccc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60254531"
---
# <a name="manage-usage-and-costs-for-application-insights"></a>Kullanımı ve maliyetleri için Application Insights'ı yönetme

> [!NOTE]
> Bu makalede, Application Insights veri kullanımını çözümleme açıklar.  İlgili bilgiler için aşağıdaki makalelere göz atın.
> - [Kullanım ve Tahmini maliyetler izleme](../../monitoring-and-diagnostics/monitoring-usage-and-estimated-costs.md) çoklu Azure İzleme özelliklerini farklı fiyatlandırma modelleri için tahmini maliyetleri ve kullanım görüntülemeyi açıklar. Ayrıca, uygulamanızın fiyatlandırma modelinin değiştirilmesi nasıl açıklar.

Application Insights için fiyatlandırma nasıl çalıştığı hakkında sorularınız varsa soru gönderebilir bizim [Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=ApplicationInsights).

## <a name="pricing-model"></a>Fiyatlandırma modeli

Fiyatlandırma [Azure Application Insights] [ start] alınan veri hacmine dayalı. Her bir Application Insights kaynağı ayrı bir hizmet ücretlendirilir ve Azure aboneliğiniz için fatura katkıda.

### <a name="data-volume-details"></a>Veri hacmi ayrıntıları

* Veri hacmi Application Insights tarafından alınan telemetrinin bayt sayısıdır. Veri hacmi, Application Insights tarafından uygulamanızdan alınan sıkıştırılmamış JSON veri paketi boyutu olarak ölçülür. İçin [Analytics'e içe tablo veri](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-import), veri hacmi Application Insights'a gönderilen dosyalarının sıkıştırılmamış boyut olarak ölçülür.
* Uygulamanızın veri hacmi ücreti artık adlı bir yeni fatura ölçümünde üzerinde bildirilen **veri alımı** Nisan 2018'den itibaren. Bu yeni ölçüm, izleme uygulamaları Insights ve Log Analytics gibi teknolojiler arasında paylaşılan ve şu anda hizmet adı altında **Log Analytics**. 
* [Canlı ölçümler Stream](../../azure-monitor/app/live-stream.md) veri amacıyla sayılan değil.

Para birimi ve bölge için geçerli fiyatlarını görmek [Application Insights fiyatlandırması][pricing].

### <a name="multi-step-web-tests"></a>Çok adımlı web testleri

[Çok adımlı web testleri](../../azure-monitor/app/monitor-web-app-availability.md#multi-step-web-tests) ek bir ücret uygulanmaz. Çok adımlı web testleri bir dizi eylem gerçekleştiren web testlerdir.

İçin ayrı ücret *ping testleri* tek bir sayfa. Alınan telemetri çalıştırılabilen ping testleri ve çok adımlı testler aynı diğer uygulamanızdan alınan telemetri olarak ücretlendirilir.

## <a name="review-usage-and-estimate-costs"></a>Kullanım ve tahmini maliyetleri gözden geçirme

Application Insights maliyetlerinizi son kullanım düzenlerini esas alarak olasılığı neler olduğunu anlama kolaylaştırır. Azure portalında Application Insights kaynağı için başlamak için Git **kullanım ve Tahmini maliyetler** sayfası:

![Fiyatlandırma seçin](./media/pricing/pricing-001.png)

A. Veri hacminiz ay için gözden geçirin. Bu alındı ve korunan tüm verileri içerir (sonra yerleştirmenizi [örnekleme](../../azure-monitor/app/sampling.md)) sunucusu ve istemci uygulamaları ve kullanılabilirlik testleri.  
B. Ayrı bir ücret yapılan [çok adımlı web testleri](../../azure-monitor/app/monitor-web-app-availability.md#multi-step-web-tests). (Bu veri birimi ücreti dahil olan basit kullanılabilirlik testleri dahil değildir.)  
C. Geçen ay için veri birimi eğilimlerini görüntüleyin.  
D. Veri alımı etkinleştirme [örnekleme](../../azure-monitor/app/sampling.md).   
E. Günlük veri hacmi üst sınırı ayarlayın.  

Application ınsights'ı kullanımınızı daha derin bir şekilde incelemek için açık **ölçümleri** sayfasında ölçüm adlandırılmış "veri noktası hacmi" ekleyin ve ardından *uygulamak bölme* Telemetri"verileri bölme seçeneği "öğe türü. 

Application Insights ücretleri Azure faturanızı eklenir. Azure, fatura ayrıntılarını görebilirsiniz **faturalama** bölümü içinde veya Azure portal'ın [Azure faturalama portalını](https://account.windowsazure.com/Subscriptions). 

![Faturalandırma sol taraftaki menüde'yı seçin](./media/pricing/02-billing.png)

## <a name="data-rate"></a>Veri oranı
Gönderdiğiniz veri hacmi üç şekilde sınırlıdır:

* **Örnekleme**: Örnekleme, sunucu ve istemci uygulamalarıyla en az bozulma ölçüm gönderilen telemetri miktarını azaltmak için kullanabilirsiniz. Örnekleme, gönderdiğiniz veri miktarını ayarlamak için kullanabileceğiniz birincil araçtır. Daha fazla bilgi edinin [özellikleri örnekleme](../../azure-monitor/app/sampling.md). 
* **Günlük üst sınır**: Azure portalında bir Application Insights kaynağı oluşturduğunuzda, 100 GB/gün için günlük üst sınır ayarlanır. Visual Studio'da Application Insights kaynağı oluşturduğunuzda, varsayılan küçüktür (yalnızca fazla 32,3 MB/gün). Günlük sınır varsayılan test edilmesini kolaylaştırmak için ayarlanır. Uygulamayı üretim ortamına dağıtmadan önce kullanıcı günlük üst sınır oluşturacak yöneliktir. 

    Yüksek trafik uygulama için daha yüksek bir maksimum istek sürece, üst sınır 1000 GB/gün olur. 

    Günlük üst sınır ayarlarken dikkatli olun. Amacınız olmalıdır *günlük üst sınır hiç isabet*. Günlük üst sınır ulaşırsanız, günün geri kalanında için veri kaybı ve uygulamanızı izleyemez. Günlük üst sınırını değiştirmek için kullanın **günlük hacim üst sınırını** seçeneği. Bu seçenekte erişebileceğiniz **kullanım ve Tahmini maliyetler** bölmesi (Bu açıklanan makalenin ilerleyen bölümlerinde daha ayrıntılı).
    Kısıtlama için Application Insights kullanılamadı alacak olan bazı abonelik türleri üzerinde kaldırdık. Daha önce abonelik bir harcama limiti varsa, günlük sınır iletişim 32,3 MB/gün oluşturulması günlük üst sınırını etkinleştirebilir ve harcama sınırını kaldırmak için yönergeleri sahiptir.
* **Azaltma**: Azaltma sınırları veri hızı, saniyede 32.000 olayları izleme anahtarı başına 1 dakika içinde ortalanır.

*Uygulamamı azaltma oranı aşarsa ne olur?*

* Uygulamanızın gönderdiği veri hacmi dakikada değerlendirilir. Dakika üzerinden ortalaması alınan Saniyedeki oranı aşarsa, sunucunun bazı istekleri reddeder. SDK, verileri arabelleğe alır ve onu yeniden dener. Bu birkaç dakika boyunca aşırı yükleme talebiyle yayar. Uygulamanızı birden çok hızlandırma oranına, verileri tutarlı bir şekilde gönderir, bazı veriler atılır. (ASP.NET, Java ve JavaScript SDK'ları veri bu şekilde yeniden göndermeyi deneyin; diğer SDK'lar yalnızca kısıtlanan bırakma veri olabilir.) Azaltma ortaya çıkarsa, bir bildirim uyarısı sizi bu oluştu sizi uyarır.

*Uygulamamın gönderdiği veri miktarını olduğunu nasıl öğrenebilirim?*

Uygulamanızın gönderdiği veri miktarını görmek için aşağıdaki seçeneklerden birini kullanabilirsiniz:

* Git **kullanım ve tahmini maliyet** bölmesinde günlük verileri toplu grafiği görmek için. 
* Ölçüm Gezgini'nde, yeni bir grafik ekleyin. Grafik ölçüsünü seçin **veri noktası birimi**. Açma **gruplandırma**ve ardından gruplandırma ölçütü **veri türü**.

## <a name="reduce-your-data-rate"></a>Veri hızınızı azaltın
Veri hacminiz azaltmak için yapabileceğiniz bazı işlemler aşağıda verilmiştir:

* Kullanım [örnekleme](../../azure-monitor/app/sampling.md). Bu teknoloji, veri hızı ölçümlerinizi eğriltme olmadan azaltır. Arama ilgili öğeleri arasında gezinme olanağı kaybetmeyin. Sunucu uygulamalarında örnekleme otomatik olarak çalışır.
* [Raporlanabilir Ajax çağrılarının sayısını sınırlamak](../../azure-monitor/app/javascript.md#detailed-configuration) her sayfa görünümü veya Ajax raporlama kapatma düğmesi.
* [Applicationınsights.config dosyasını düzenlemek](../../azure-monitor/app/configuration-with-applicationinsights-config.md) ihtiyacınız olmayan toplama modülleri açmak için. Örneğin, performans sayaçlarını veya bağımlılık verileri gerekli olmayan karar verebilirsiniz.
* Telemetrinizi ayrı bir izleme anahtarı arasında bölün. 
* Önceden toplu ölçümleri. Uygulamanızda TrackMetric çağrıları yerleştirirseniz, ortalama, hesaplama ve standart sapmasını toplu ölçümler kabul eden aşırı yüklemesi kullanarak trafiğini azaltabilir. Veya, kullanabileceğiniz bir [önceden toplama paket](https://www.myget.org/gallery/applicationinsights-sdk-labs).

## <a name="manage-the-maximum-daily-data-volume"></a>Maksimum günlük veri hacmini yönetme

Günlük hacim üst sınırını toplanan verileri sınırlamak için kullanabilirsiniz. Ancak, cap karşılanırsa, uygulamanızdan günün geri kalanı boyunca gönderilen tüm telemetri kaybı oluşur. Bu *değil kapatılacağından* uygulamanız için günlük üst sınır basın. Günlük üst sınır ulaştıktan sonra uygulamanızın performansı ve sistem durumu izleyemezsiniz.

Günlük hacim üst sınırını kullanmak yerine, [örnekleme](../../azure-monitor/app/sampling.md) veri hacmi için istediğiniz düzeyini ayarlamak için. Ardından, çok daha yüksek birimleri telemetri göndermek uygulamanızı beklenmedik bir şekilde başlar durumda günlük üst sınır "son çare" kullanın.

Günlük üst sınırını değiştirmek için **yapılandırma** Application Insights kaynağınıza bölümünü, **kullanım ve Tahmini maliyetler** bölmesinde **günlük üst sınır**.

![Günlük telemetri hacim üst sınırını Ayarla](./media/pricing/pricing-003.png)

## <a name="sampling"></a>Örnekleme
[Örnekleme](../../azure-monitor/app/sampling.md) hangi telemetri gönderilir, uygulamanıza bulmasını sırasında tanılama aramaları ilgili olaylar korurken hızının düşürülmesi bir yöntemdir. Ayrıca, doğru olay sayısı de korur.

Örnekleme giderlerini azaltmak ve aylık kota içinde kalmak için etkili bir yoludur. Örnekleme algoritması ilgili telemetri öğelerinin şekilde korur, örneğin, arama kullandığınızda, belirli bir özel durumla ilişkili istek bulabilirsiniz. Ölçüm Gezgini'nde doğru değerleri istek hızları, özel durum oranları ve diğer sayıları görmek için algoritma doğru sayılarını da korur.

Örnekleme birkaç biçimi vardır.

* [Uyarlamalı örnekleme](../../azure-monitor/app/sampling.md) ASP.NET SDK'sı için varsayılandır. Uyarlamalı örnekleme, uygulamanızın gönderdiği telemetri hacmi otomatik olarak ayarlar. Telemetri trafiğini ağda küçülmesini SDK web uygulamanıza otomatik olarak çalışır. 
* *Alım örneklemesi* burada uygulamanızdan alınan telemetri girer Application Insights hizmetine noktada çalışır bir alternatiftir. Alım örneklemesi uygulamanızdan gönderilen telemetri hacmini etkilemez, ancak hizmet tarafından korunan birimin azaltır. Alım örneklemesi alınan telemetri tarayıcılar ve diğer SDK'lar tarafından kullanılan kotayı azaltmak için kullanabilirsiniz.

Alım örneklemesi ayarlamak için Git **fiyatlandırma** bölmesi:

![Kota ve fiyatlandırma bölmesinde, örnekleri kutucuğunu seçin ve ardından bir örnekleme kesir seçin](./media/pricing/pricing-004.png)

> [!WARNING]
> **Veri örnekleme** bölmesinde yalnızca alım örneklemesi değerini denetler. Bu, örnekleme hızını uygulamanızda Application Insights SDK'sı tarafından uygulanan yansıtmaz. Gelen telemetri SDK'yı örneklenen, alım örneklemesi uygulanmaz.
>

Uygulanmış, ne olursa olsun gerçek örnekleme oranını bulmak için kullanmak bir [Analytics sorgusu](analytics.md). Sorguyu şu şekilde görünür:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h)
    | render areachart

Her kaydı tutulur `itemCount` temsil ettiği özgün kayıt sayısını belirtir. 1 + önceki atılan kayıt sayısı eşittir. 

## <a name="automation"></a>Otomasyon

Azure kaynak Yönetimi'ni kullanarak fiyatını planı ayarlamak için bir betik yazabilirsiniz. [Nasıl olduğunu öğrenin](powershell.md#price).

## <a name="limits-summary"></a>Sınırları özeti

[!INCLUDE [application-insights-limits](../../../includes/application-insights-limits.md)]

## <a name="disable-daily-cap-e-mails"></a>Günlük sınır e-postalar devre dışı bırak

Günlük birim sınırı e-postalar, altında devre dışı bırakmak için **yapılandırma** Application Insights kaynağınıza bölümünü, **kullanım ve Tahmini maliyetler** bölmesinde **günlük üst sınır** . Sınırına ulaşıldığında, ayarlanabilir bir uyarı düzeyine ulaşıldı yanı sıra e-posta göndermek için ayarları vardır. Tüm günlük devre dışı bırakmak istiyorsanız her iki e-posta kutusunun işaretini kaldırın uç birim ilgili.

## <a name="legacy-enterprise-pricing-plan"></a>Eski Kurumsal fiyatlandırma planı

Azure Application Insights'ın erken Benimseyenler için yine de iki olası iki fiyatlandırma planı vardır: Temel ve kurumsal. Temel fiyatlandırma planı, yukarıda açıklandığı gibi aynıdır ve varsayılan plan. Bu, ek maliyet olmadan, tüm kurumsal plan özellikleri içerir. Temel plan faturalandırılır öncelikle alınan veri hacmi. 

Kurumsal plan bir düğüm başına ücret vardır ve her düğüm günlük veri kullanım hakkı alır. Kurumsal fiyatlandırma planını, bulunan indirimi alınan veriler için ücretlendirilirsiniz. Operations Management Suite kullanıyorsanız, Kurumsal plan seçmeniz gerekir. 

Para birimi ve bölge için geçerli fiyatlarını görmek [Application Insights fiyatlandırması](https://azure.microsoft.com/pricing/details/application-insights/).

> [!NOTE]
> Nisan 2018'de biz [sunulan](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/) Azure izleme için yeni bir fiyatlandırma modeli. Bu model, hizmetleri izleme tam Portföyü genelinde basit bir "Kullandıkça Öde" modelini devralır. Daha fazla bilgi edinin [yeni fiyatlandırma modeline](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs), nasıl için [bu modeline taşıma etkisini değerlendirmek](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#assessing-the-impact-of-the-new-pricing-model) , kullanım modellerini ve [yeni modele nasıl](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#moving-to-the-new-pricing-model)

### <a name="enterprise-plan-and-operations-management-suite-subscription-entitlements"></a>Kurumsal plan ve Operations Management Suite aboneliği destek hakları

Operations Management Suite E1 ve E2 satın almış olan müşteriler, ek ücret ödemeden başka bir bileşen olarak Application Insights Kurumsal alabilirsiniz [daha önce duyurulduğu gibi](https://blogs.technet.microsoft.com/msoms/2017/05/19/azure-application-insights-enterprise-as-part-of-operations-management-suite-subscription/). Özellikle, her bir birimi, Operations Management Suite E1 ve E2 Application Insights Kurumsal planına bir düğüm hakkı içerir. Her Application Insights düğüm başına ek ücret ödemeden 90 günlük veri saklama (ayrı), Log Analytics veri alma günlük içe alınan veri 200 MB'a kadar içerir. Plan daha ayrıntılı makalenin sonraki bölümlerinde açıklanmıştır. 

Bu plan yalnızca bir Operations Management Suite aboneliği olan müşteriler için geçerli olduğundan, bir Operations Management Suite aboneliğiniz yoksa müşteriler bu planı seçmek için bir seçenek görmezsiniz.

> [!NOTE]
> Bu yetkilendirme aldığından emin olmak için Application Insights kaynaklarınızı fiyatlandırma planını Kurumsal olması gerekir. Bu yetkilendirme yalnızca düğümleri uygulanır. Application Insights kaynakları temel planda edilmesinden bir fayda farkında değil. Bu yetkilendirme gösterilen tahmini maliyetleri de görünür olmayan **kullanım ve tahmini maliyet** bölmesi. Ayrıca, yeni fiyatlandırma modeli izleme Nisan 2018'de Azure abonelik taşıma, temel plan yalnızca plan kullanılabilir olur. Bir aboneliği yeni fiyatlandırma modeli izleme Azure'a taşıma, bir Operations Management Suite aboneliğiniz varsa önerilir değil.

### <a name="how-the-enterprise-plan-works"></a>Kurumsal plan nasıl çalışır?

* Kurumsal plandaki tüm uygulamaları için telemetri gönderen her düğüm için ödeme yaparsınız.
  * A *düğüm* sunucusu fiziksel veya sanal makine veya uygulamanızı barındıran bir hizmet olarak platform rolü örneği.
  * Geliştirme makineler, istemci tarayıcıları ve mobil cihazlar düğüm olarak sayılmaz.
  * Uygulamanızı bir web hizmeti ve bir arka uç çalışan gibi telemetriyi göndermek birkaç bileşeni varsa bileşenleri ayrı olarak sayılır.
  * [Canlı ölçümler Stream](../../azure-monitor/app/live-stream.md) veri amacıyla sayılan değil. Bir abonelikte, uygulama başına değil, düğüm başına ücretlerdir. 12 için telemetri gönderen beş düğüm varsa uygulamalar, beş düğüm için ücretlendirme yapılır.
* Aylık ücret tırnak içinde olsa da, yalnızca içinde ve bir düğümün telemetri bir uygulamadan gönderdiği saat için ücret ödersiniz. Saatlik bir ücret 744 (ayda 31 gün saat sayısı) bölü tırnak işaretli aylık sabit ücrettir.
* 200 MB veri birimi ayrılması günde (saatlik ayrıntı düzeyiyle) algılanan her düğüm için verilir. Kullanılmayan verileri ayırma günden sonraki devreden değil.
  * Kurumsal fiyatlandırma planını seçerseniz, her abonelik bir günlük kullanım hakkı bu abonelikte Application Insights kaynaklara telemetri gönderen düğüm sayısına göre verileri alır. Bu nedenle, tüm gün veri gönderen beş düğümünüz varsa, havuza alınmış bir indirimini bu Abonelikteki tüm Application Insights kaynakları için uygulanan 1 GB gerekir. Dahil edilen veri tüm düğümler arasında paylaşıldığından bazı düğümlerin diğerlerine göre daha fazla veri gönderdiğiniz olup olmaması önemli değildir. Belirli bir gün, Application Insights kaynakları Bu abonelik için günlük veri ayırma dahil edilmiş miktardan daha fazla veri almak, GB başına fazla kullanım veri ücretleri uygulanır. 
  * Günlük veri Kullanım Hakkı (UTC saat kullanarak) bir gündeki saat sayısı hesaplanır her düğüm 200 MB ile çarpılarak 24 bölü telemetri gönderir. Bu nedenle, 15 gün içinde 24 saat boyunca telemetri gönderen dört düğümünüz varsa, o gün için dahil edilen veri olması ((4 &#215; 15) / 24) &#215; 200 MB = 500 MB. Düğümleri o gün 1 GB veri gönderiyorsanız fiyatına 2.30 ABD Doları / GB veri fazla kullanımı için ücret 1,15 ABD Doları olacaktır.
  * Kurumsal plan günlük kullanım hakkı, temel plan tercih ettiğiniz uygulamalarla paylaşılmaz. Kullanılmayan indirimi gelen günlük devreden değil. 

### <a name="examples-of-how-to-determine-distinct-node-count"></a>Ayrı bir düğüm sayısını belirlemek nasıl örnekleri

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

## <a name="next-steps"></a>Sonraki adımlar

* [Örnekleme](../../azure-monitor/app/sampling.md)

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: ../../azure-monitor/app/app-insights-overview.md
[pricing]: https://azure.microsoft.com/pricing/details/application-insights/