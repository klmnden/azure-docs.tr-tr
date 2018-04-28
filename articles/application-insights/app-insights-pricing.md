---
title: Fiyatlandırma ve veri birimi yönetmek için Azure Application Insights | Microsoft Docs
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
ms.openlocfilehash: 2c06c2220d3a3ed0a27b4f0febb4de95b2137ddc
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="manage-pricing-and-data-volume-in-application-insights"></a>Application Insights fiyatlandırma ve veri biriminde yönetme

Fiyatlandırmasını [Azure Application Insights] [ start] uygulama başına veri hacmi dayanır. Her bir Application Insights kaynağı ayrı bir hizmet ücretlendirilir ve Azure aboneliğiniz için fatura katkıda bulunur.

Application Insights sahip iki fiyatlandırma planı: Basic ve Enterprise. Temel fiyatlandırma planı varsayılan planı ' dir. Hiçbir ek ücret ödemeden tüm kurumsal plan özellikleri içerir. Temel plan faturaları alınan veri biriminde öncelikle. 

Bir düğüm başına ücret Kurumsal planına sahip ve her düğüm bir günlük veri indirimi alır. Kuruluşta planı, fiyatlandırma, dahil indirimi alınan veriler için sizden ücret kesilir. Operations Management Suite kullanırsanız, Kurumsal plan seçmeniz gerekir. 

Fiyatlandırma için Application Insights nasıl çalıştığı hakkında sorularınız varsa, bir soru nakledebilirsiniz bizim [Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=ApplicationInsights).

## <a name="pricing-plans"></a>Fiyatlandırma planı

Para birimi ve bölge geçerli fiyatlar için bkz: [Application Insights fiyatlandırma][pricing].

> [!NOTE]
> Nisan 2018, biz [sunulan](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/) Azure izlemek için yeni bir fiyatlandırma modeli. Bu model Hizmetleri izleme tam Portföy arasında basit bir "Kullandıkça Öde" modelinin devralır. Daha fazla bilgi edinmek [yeni fiyatlandırma modeli](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs), nasıl için [bu modeline taşıma etkisini değerlendirmenize](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#assessing-the-impact-of-the-new-pricing-model) , kullanım düzenlerini esas alarak ve [yeni modeline kabul etme](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#moving-to-the-new-pricing-model).

### <a name="basic-plan"></a>Temel plan

Temel plan yeni bir Application Insights kaynak oluşturulduğunda planı fiyatlandırma varsayılandır. Temel plan, bir Operations Management Suite aboneliğine sahip olanlar dışında tüm müşteriler için uygundur.

* Temel planda bazında veri hacmi sizden ücret kesilir. Veri birimi Application Insights tarafından alınan telemetri bayt sayısıdır. 
    
    Veri birimi Application Insights tarafından uygulamanızdan alınan sıkıştırılmamış JSON veri paketi boyutu olarak ölçülür.

    İçin [tablo veri analizi için içeri](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-import), veri birimi Application Insights'a gönderilen dosyalarının sıkıştırılmamış boyut olarak ölçülür.
* [Canlı ölçümleri akış](app-insights-live-stream.md) veri yok sayılan amacıyla fiyatlandırma için.
* [Sürekli verme](app-insights-export-telemetry.md) ve [Azure günlük analizi bağlayıcı](https://go.microsoft.com/fwlink/?LinkId=833039&amp;clcid=0x409) Nisan 2018 itibariyle temel plan ekstra ücret ödemeden kullanılabilir.

### <a name="enterprise-plan-and-operations-management-suite-subscription-entitlements"></a>Kurumsal plan ve Operations Management Suite aboneliği yetkilendirmeler

Operations Management Suite E1 ve E2 satın müşteriler ek bileşen hiçbir ek ücret ödemeden olarak uygulama Öngörüler Kurumsal alabilirsiniz [önceden duyurulmuş](https://blogs.technet.microsoft.com/msoms/2017/05/19/azure-application-insights-enterprise-as-part-of-operations-management-suite-subscription/). Özellikle, Operations Management Suite E1 ve E2 her birimi bir düğüme bir yetkilendirme uygulama Öngörüler Kurumsal planının içerir. 200 MB günde (ayrı), günlük analizi veri alımı hiçbir ek ücret ödemeden 90 günlük veri saklama ile alınan verilerin her Application Insights düğümü içerir. Plan daha makalenin sonraki bölümlerinde ayrıntılı olarak açıklanmıştır. 

Bu plan yalnızca bir Operations Management Suite aboneliği sahip müşteriler için geçerli olduğundan, bir Operations Management Suite aboneliğiniz müşteriler bu planı seçmek için bir seçenek görmezsiniz.

> [!NOTE]
> Bu yetkilendirme aldığından emin olmak için Application Insights kaynaklarınızı plan fiyatlandırması kuruluşta olması gerekir. Bu yetkilendirme yalnızca düğümleri olarak uygulanır. Uygulama Öngörüler kaynakları temel planda bir fayda fark yoktur. Bu yetkilendirme gösterilen tahmini maliyetleri de görünür olmayan **kullanım ve tahmini maliyet** bölmesi. Ayrıca, fiyatlandırma modeli Nisan 2018 izleme yeni Azure bir aboneliği taşımak, temel plan yalnızca plan kullanılabilir olur. Fiyatlandırma modeli izleme yeni Azure abonelik taşıma bir Operations Management Suite aboneliğiniz varsa önerilir değil.

Kurumsal planı hakkında daha fazla bilgi için bkz: [fiyatlandırma ayrıntıları Kurumsal](app-insights-pricing-enterprise-details.md).

### <a name="multi-step-web-tests"></a>Çok adımlı web testleri

[Çok adımlı web testleri](app-insights-monitor-web-app-availability.md#multi-step-web-tests) ek bir ücret doğurur. Çok adımlı web testleri bir dizi eylemi gerçekleştirmek web testleri ' dir.

İçin ayrı olan herhangi bir ücret alınmaz *ping testleri* tek bir sayfa. Çok adımlı sınamaları ve ping testlerinde telemetrisinden diğer telemetriyi uygulamanızdan aynı ücretlendirilir.

## <a name="review-pricing-plans-and-estimate-costs"></a>Fiyatlandırma planlarını gözden geçirin ve maliyetleri tahmin etme

Application Insights kullanılabilir fiyatlandırma planları anlamak kolaylaştırır ve hangi maliyetleriniz son kullanım düzenlerini esas alarak olasılığı düşüktür. Application Insights kaynağı için Azure Portalı'nda başlamak için Git **kullanım ve tahmini maliyetleri** bölmesi:

![Fiyatlandırma seçin](./media/app-insights-pricing/pricing-001.png)

A. Veri biriminiz ayı için gözden geçirin. Bu alınan korunur ve tüm verileri içerir (herhangi bir sonra [örnekleme](app-insights-sampling.md)) sunucusu ve istemci uygulamaları ve kullanılabilirlik testleri.  
B. Ayrı bir ücret yapılan [çok adımlı web testleri](app-insights-monitor-web-app-availability.md#multi-step-web-tests). (Bu, veri birimi bir ücret alınmaz dahil basit kullanılabilirlik testleri içermez.)  
C. Geçen ay için veri birimi eğilimleri görüntüleme.  
D. Veri alımı etkinleştirmek [örnekleme](app-insights-sampling.md).   
E. Günlük veri birimi ucun ayarlayın.  

Uygulama Öngörüler ücretler Azure faturanızı eklenir. Azure içinde fatura ayrıntılarını görebilirsiniz **faturalama** bölümde Azure portal'ın veya içinde [Azure fatura portalı](https://account.windowsazure.com/Subscriptions). 

![Soldaki menüde faturalama seçin](./media/app-insights-pricing/02-billing.png)

## <a name="data-rate"></a>Veri oranı
Verilerin hacmi üç şekilde sınırlıdır:

* **Örnekleme**: örnekleme uygulamalarınızdan sunucu ve istemci, en az bozulma ölçümleri ile gönderilen telemetri miktarını azaltmak için kullanabilirsiniz. Örnekleme gönderdiğiniz veri miktarını ayarlamak için kullanabileceğiniz birincil araçtır. Daha fazla bilgi edinmek [özellikleri örnekleme](app-insights-sampling.md). 
* **Günlük sınır**: Azure portalında Application Insights kaynağı oluşturduğunuzda, günlük sınır 100 GB/gün olarak ayarlanır. Visual Studio'da Application Insights kaynağı oluşturduğunuzda, varsayılan küçük (yalnızca 32.3 MB/gün). Günlük sınır varsayılan test kolaylaştırmak için ayarlanır. Kullanıcı uygulamayı üretim ortamına dağıtmadan önce günlük ucun oluşturacağı yöneliktir. 

    Bir yüksek trafik uygulama için daha yüksek bir maksimum istek sürece, üst sınır uygulanıyor 1000 GB/gün olur. 

    Günlük sınır ayarlarken dikkatli olun. Maksadınızı olmalıdır *hiç günlük ucun isabet*. Günlük sınır isabet durumunda geri kalanı gün için veri kaybına ve uygulamanızı izleyemez. Günlük cap değiştirmek için kullanmak **günlük birimi cap** seçeneği. Bu seçenek erişebilirsiniz **kullanım ve tahmini maliyetleri** bölmesinde (Bu açıklanan makalenin ilerleyen bölümlerinde daha ayrıntılı).
    Kısıtlama için Application Insights kullanılamadı alacak bazı abonelik türlerinde kaldırdık. Daha önce abonelik bir harcama sınırı varsa, günlük sınır iletişim harcama sınırını kaldırmak ve 32.3 MB/gün çıkarılmasına günlük ucun etkinleştirmek için yönergeler vardır.
* **Azaltma**: azaltma sınırları 32.000 olayları saniye başına veri hızı ortalaması 1 dakika içinde.

*Uygulamam azaltma oranı aşarsa ne olur?*

* Uygulamanızı dakikada uygunluk gönderir veri hacmi. Dakika içinde ölçülen ortalama saniyede oranı aşarsa, sunucunun bazı istekleri reddeder. SDK verileri arabelleğe alır ve bunu yeniden göndermeyi dener. Bu, birkaç dakika boyunca dalgalanma yayar. Uygulamanız birden çok azaltma hızı, verileri tutarlı bir şekilde gönderir, bazı verisi bırakılacak. (ASP.NET, Java ve JavaScript SDK'ları bu şekilde veri göndermeyi deneyin; diğer SDK yalnızca kısıtlanan açılan veri olabilir.) Azaltma ortaya çıkarsa, bir bildirim uyarı bu oluştu uyarır.

*Uygulamam gönderme ne kadar verinin nasıl biliyor musunuz?*

Ne kadar veri uygulamanızı gönderme görmek için aşağıdaki seçeneklerden birini kullanabilirsiniz:

* Git **kullanım ve tahmini maliyet** günlük veri birimi grafik görmek için bölmesi. 
* Ölçümleri Explorer'da yeni bir grafik ekleyin. Grafik ölçümü seçin **veri noktası birimi**. Aç **gruplandırma**ve ardından gruplandırma ölçütü **veri türü**.

## <a name="reduce-your-data-rate"></a>Veri hızını azaltın
Veri biriminiz azaltmak için yapabileceğiniz bazı noktalar şunlardır:

* Kullanım [örnekleme](app-insights-sampling.md). Bu teknoloji, ölçümlerinizi eğriltme olmadan veri hızını azaltır. Aramadaki ilgili öğeleri arasında gezinme özelliği kaybetmeyin. Sunucu uygulamaları örnekleme otomatik olarak çalışır.
* [Bildirilebilir Ajax çağrılarının sayısını sınırlamak](app-insights-javascript.md#detailed-configuration) her sayfa görünümü veya Ajax Raporlama Kapalı anahtarı.
* [Applicationınsights.config Düzenle](app-insights-configuration-with-applicationinsights-config.md) gerekmeyen koleksiyonu modülleri devre dışı bırakma. Örneğin, performans sayaçlarını veya bağımlılık veri inessential karar verebilirsiniz.
* Telemetrinize ayrı araçları anahtarlar bölün. 
* Önceden toplama ölçümleri. Uygulamanızda TrackMetric çağrıları yerleştirirseniz, ortalama, hesaplama ve ölçümleri toplu standart sapmasını kabul aşırı kullanarak trafiğini azaltabilir. Veya, kullanabileceğiniz bir [önceden toplama paketi](https://www.myget.org/gallery/applicationinsights-sdk-labs).

## <a name="manage-the-maximum-daily-data-volume"></a>Maksimum günlük veri hacmi yönetme

Günlük birim ucun toplanan verileri sınırlamak için kullanabilirsiniz. Ancak, cap karşılanırsa uygulamanızdan günün geri kalanı gönderilen tüm telemetri kaybı oluşur. Bu *değil önerilir* uygulamanız için günlük ucun isabet. Günlük sınır ulaştıktan sonra uygulamanızın performansını ve sistem durumu izleyemez.

Günlük birim ucun kullanmak yerine, [örnekleme](app-insights-sampling.md) ayarlamak istediğiniz düzeye veri birimi için. Ardından, uygulamanız beklenmedik bir şekilde telemetri kadar yüksek hacimli göndermeye başlar durumda günlük cap "son çare olarak" kullanın.

Günlük cap değiştirmek için **yapılandırma** Application Insights kaynağınıza bölümünü, **kullanım ve tahmini maliyetleri** bölmesinde, **günlük Cap**.

![Günlük telemetri birim ucu ayarlama](./media/app-insights-pricing/pricing-003.png)

## <a name="sampling"></a>Örnekleme
[Örnekleme](app-insights-sampling.md) telemetri gönderilme uygulamanıza, ilgili olaylar sırasında tanılama aramaları bulmasını korurken hızının düşürülmesi, bir yöntemdir. Ayrıca, doğru olay sayıları de korur.

Örnekleme giderlerini azaltmak ve aylık kotanızı içinde kalmak için etkili bir yoldur. Örnekleme algoritması telemetri ilgili öğeler şekilde korur, örneğin, arama kullandığınızda, belirli bir özel durumla ilişkili istek bulabilirsiniz. Ölçüm Gezgini istek hızları, özel durum hızları ve diğer sayılar için doğru değerleri görmeleri algoritma doğru sayıları de korur.

Örnekleme çeşitli biçimlerde vardır.

* [Uyarlamalı örnekleme](app-insights-sampling.md) için ASP.NET SDK'sı varsayılandır. Uyarlamalı örnekleme uygulamanızı gönderdiği telemetri birime otomatik olarak ayarlar. Böylece ağdaki telemetri trafiği azaltılır SDK web uygulamanızda otomatik olarak çalışır. 
* *Alım örnekleme* burada uygulamanızdan telemetrinin girer Application Insights hizmeti noktada çalışır alternatiftir. Alım örnekleme telemetriyi uygulamanızdan gönderilen hacmi etkilemez, ancak hizmet tarafından korunan birim azaltır. Tarayıcılar ve diğer SDK telemetri tarafından kullanılan kota azaltmak için alım örnekleme kullanabilirsiniz.

Alım örnekleme ayarlamak için Git **fiyatlandırma** bölmesi:

![Kota ve fiyatlandırma bölmesi, örnekleri kutucuğu seçin ve ardından örnekleme kesir seçin](./media/app-insights-pricing/pricing-004.png)

> [!WARNING]
> **Veri örnekleme** bölmesinde yalnızca alım örnekleme değerini denetler. Uygulamanıza Application Insights SDK'sı tarafından uygulanan örnekleme hızını yansıtmaz. Gelen telemetri SDK'ın örneklenen, alım örnekleme uygulanmaz.
>

Uygulanmış, olsun gerçek örnekleme hızını keşfetmek için bir [Analytics sorgu](app-insights-analytics.md). Sorgu şu şekildedir:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h)
    | render areachart

Her kaydı tutulur `itemCount` temsil ettiği özgün kayıt sayısını gösterir. 1 + önceki atılan kayıt sayısı için eşittir. 

## <a name="automation"></a>Otomasyon

Azure kaynak yönetimi kullanarak fiyat planı ayarlamak için bir komut dosyası yazabilirsiniz. [Nasıl olduğunu öğrenin](app-insights-powershell.md#price).

## <a name="limits-summary"></a>Sınırları özeti

[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Örnekleme](app-insights-sampling.md)

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: app-insights-overview.md
[pricing]: http://azure.microsoft.com/pricing/details/application-insights/
