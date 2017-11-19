---
title: "Fiyatlandırma ve veri birimi yönetmek için Azure Application Insights | Microsoft Docs"
description: "Telemetri birimleri yönetin ve Application Insights maliyetlerini izleyin."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: ebd0d843-4780-4ff3-bc68-932aa44185f6
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: mbullwin
ms.openlocfilehash: ecb6dd0343c36a0f1571b416817aad5e7a52fccb
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="manage-pricing-and-data-volume-in-application-insights"></a>Application Insights fiyatlandırma ve veri biriminde yönetme


Fiyatlandırmasını [Azure Application Insights] [ start] uygulama başına veri hacmi dayanır. Düşük kullanım geliştirme sırasında veya küçük bir uygulama için 1 GB aylık indirimi telemetri verilerinin olduğundan boş olması olasıdır.

Her bir Application Insights kaynağı ayrı bir hizmet ücretlendirilir ve Azure aboneliğiniz için fatura katkıda bulunur.

İki fiyatlandırma planı vardır. Varsayılan plan Basic adı verilir. Günlük bir ücretsiz olarak sahip, ancak bazı ek özellikler gibi sağlayan kurumsal plan için seçebilirsiniz [sürekli verme](app-insights-export-telemetry.md).

Fiyatlandırma için Application Insights nasıl çalıştığı hakkında sorularınız varsa, bir soru gönderin çekinmeyin bizim [Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=ApplicationInsights). 

## <a name="the-price-plans"></a>Fiyat planları

Bkz: [Application Insights fiyatlandırma sayfası] [ pricing] geçerli fiyatlar para birimi değerlerinizi için.

### <a name="basic-plan"></a>Temel plan

Yeni bir Application Insights kaynak oluşturulduğunda ve çoğu müşteri için yeterli temel plan varsayılandır.

* Temel planda veri hacmine göre ücretlendirilen: Application Insights tarafından alınan telemetri bayt sayısı. Veri birimi Application Insights tarafından uygulamanızdan alınan sıkıştırılmamış JSON veri paketi boyutu olarak ölçülür.
İçin [tablo veri analizi içeri](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-import), veri hacmi Application Insights'a gönderilen dosyaların sıkıştırılmamış boyut olarak ölçülür.  
* Yalnızca denemeler geliştirme veya ödeme yapmanız tahmin edilemez şekilde her uygulama için ilk, 1 GB ücretsizdir.
* [Canlı ölçümleri akış](app-insights-live-stream.md) veri yok sayılan amacıyla fiyatlandırma için.
* [Sürekli verme](app-insights-export-telemetry.md) temel planda bir ek GB başına ücret için kullanılabilir.

### <a name="enterprise-plan"></a>Kurumsal plan

* Kurumsal planda uygulamanıza Application Insights tüm özelliklerini kullanabilirsiniz. [Sürekli verme](app-insights-export-telemetry.md) ve 

[Analytics bağlayıcı oturum](https://go.microsoft.com/fwlink/?LinkId=833039&amp;clcid=0x409) Kurumsal planda ek bir ücret kullanılabilir.
* Telemetri tüm uygulamaların Kurumsal planda gönderiyor düğüm başına ücret ödersiniz. 
 * A *düğüm* bir fiziksel veya sanal sunucu makinesi ya da uygulamanızı barındıran bir hizmet olarak Platform rol örneği.
 * Geliştirme makineler, istemci tarayıcıları ve mobil cihazları düğümleri olarak sayılmaz.
 * Uygulamanızı bir web hizmeti ve arka uç çalışan gibi telemetri göndermesine çeşitli bileşenleri varsa bunlar ayrı olarak sayılır.
 * [Canlı ölçümleri akış](app-insights-live-stream.md) veri yok sayılan yaratılır abonelik üzerinden fiyatlandırma için uygulama başına düğüm başına ücretlerinizi. 12 telemetri göndermesini beş düğümleriniz varsa uygulamalar ücret olduğundan için beş düğüm.
* Aylık ücret tırnak içine rağmen yalnızca içinde ve bir düğüm telemetri bir uygulamadan gönderir her saat için ücret ödersiniz. Tırnak işaretli aylık ücret saatlik ücret olduğu / 744 (saat cinsinden 31 gün ay sayısı).
* 200 MB günde bir veri birimi ayırma (saatlik ayrıntı) algılanan her düğüm için verilir. Kullanılmayan veri ayırma bir günden sonraki devredilir değil.
 * Kurumsal fiyatlandırma seçeneği belirlerseniz, her abonelik bir günlük indirimi veri aboneliğin Application Insights kaynaklarına telemetri göndermesini düğüm sayısını temel alır. Bu nedenle tüm günlük verileri gönderme 5 düğümleriniz varsa, bu Abonelikteki tüm Application Insights kaynaklarına uygulanan 1 GB havuza alınmış bir indirimi gerekir. Dahil edilen veri tüm düğümleri arasında paylaşıldığından belirli düğümler diğer düğümlere daha fazla veri gönderiyorsanız önemli değildir. Belirli bir tarihteki Application Insights kaynakları Bu abonelik için günlük veri ayırma dahil daha fazla veri almaya devam ederseniz, GB başına fazla kullanım veri ücretleri uygulanır. 
 * Günlük verileri indirimi saat (UTC kullanarak) gün cinsinden sayı olarak hesaplanır her düğüm tarafından 24 kez 200 MB bölünmüş telemetri gönderiyor. 15 gün içinde 24 saatlik sırasında telemetri göndermesini 4 düğüm varsa, o gün için dahil edilen veri olacak şekilde ((4 x 15) / 24) 200 MB = 500 MB x. Düğümleri o gün 1 GB veri gönderirseniz veri fazlalık için fiyat GB başına 2.30 ABD Doları, 1,15 ABD doları için ücret olacaktır.
 * Kurumsal planın günlük indirimi temel seçeneği seçtiğiniz uygulamalarla paylaşılmayan not alın ve kullanılmayan indirimi gelen günlük devredilir değil. 
* Ayrı düğüm sayısını belirlemek bazı örnekler şunlardır:
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
  * Uygulamanız, roleInstance özel bir değere ayarlamak için SDK'sı kullanıyorsa, varsayılan olarak aynı değeri düğüm sayısını belirlemek üzere kullanılacak olduğunu unutmayın. 
  * İstemci makineler veya mobil cihazlardan çalışan bir uygulama ile yeni bir SDK sürümünü kullanıyorsanız, düğüm sayısı (çok sayıda istemci makineleri veya mobil cihazlar) çok büyük bir sayı döndürebilir mümkündür. 

### <a name="multi-step-web-tests"></a>Çok adımlı web testleri

İçin ek bir ücret yoktur [çok adımlı web testleri](app-insights-monitor-web-app-availability.md#multi-step-web-tests). Bu, bir dizi eylemi gerçekleştirmek için web testleri ifade eder. 

Tek bir sayfayla 'ping sınaması' için ayrı bir ücret ödemeden yoktur. Diğer telemetriyi uygulamanızdan birlikte ping testleri ve çok adımlı testleri telemetrisinden ücretlendirilir.
 
## <a name="operations-management-suite-subscription-entitlement"></a>Operations Management Suite aboneliği yetkilendirme

Olarak [son duyurdu](https://blogs.technet.microsoft.com/msoms/2017/05/19/azure-application-insights-enterprise-as-part-of-operations-management-suite-subscription/), Microsoft Operations Management Suite E1 ve E2 satın müşteriler hiçbir ek ücret ödemeden ek bileşen olarak uygulama Öngörüler Kurumsal alabilir. Özellikle, Operations Management Suite E1 ve E2 her birimi 1 düğüme bir yetkilendirme Application Insights Kurumsal planının içerir. Yukarıda belirtildiği gibi 200 MB günde (ayrı), günlük analizi veri alımı hiçbir ek ücret ödemeden 90 günlük veri saklama ile alınan verilerin her Application Insights düğümü içerir. 

> [!NOTE]
> Bu yetkilendirme aldığından emin olmak için Application Insights kaynaklarınızı kuruluştaki planı fiyatlandırma olmalıdır. Temel plan Application Insights kaynaklarında bir fayda fark olmayan şekilde bu yetkilendirme yalnızca düğümleri olarak uygulanır. Bu yetkilendirme gösterilen özellikleri + fiyatlandırma dikey tahmini maliyetleri görünür olmaz unutmayın. 
>
 
## <a name="review-pricing-plans-and-estimate-costs"></a>Fiyatlandırma planlarını gözden geçirin ve maliyetleri tahmin etme

Applicaition Öngörüler kullanılabilir fiyatlandırma planları ve hangi maliyetleri büyük olasılıkla olması tabanlı üzerinde son kullanım desenlerini anlamak daha kolay hale getirir. Başlangıç açarak **özellikleri + fiyatlandırma** dikey penceresinde Azure portalında Application Insights kaynağı:

![Fiyatlandırma seçin.](./media/app-insights-pricing/01-pricing.png)

**a.** Veri biriminiz ayı için gözden geçirin. Bu alındı ve korunan tüm verileri içerir (herhangi bir sonra [örnekleme](app-insights-sampling.md) sunucu ve istemci uygulamalarını ve kullanılabilirlik testleri.

**b.** Ayrı bir ücret yapılan [çok adımlı web testleri](app-insights-monitor-web-app-availability.md#multi-step-web-tests). (Bu, veri birimi bir ücret alınmaz dahil basit kullanılabilirlik testleri içermez.)

**c.** Kurumsal planı etkinleştirin.

**d.** Üzerinden veri birimi için geçen ay görüntüleme, günlük sınır ayarlayın veya alım örnekleme ayarlamak için yönetim seçenekleri veri için tıklayın.

Uygulama Öngörüler ücretler Azure faturanızı eklenir. Azure Faturalama bölümünde Azure portalının ya da buna fatura ayrıntılarını görebilirsiniz [Azure Billing Portal](https://account.windowsazure.com/Subscriptions). 

![Yan menüde faturalama seçin.](./media/app-insights-pricing/02-billing.png)

## <a name="data-rate"></a>Veri oranı
Verileri gönderme birim sınırlıdır üç yolu vardır:

* **Örnekleme:** bu düzenek kullanılabilir sunucu ve istemci uygulamalarınızı en az bozulma ölçümleri ile gönderilen telemetri miktarını azaltın. Bu, veri miktarını ayarlamak için sahip birincil araçtır. Daha fazla bilgi edinmek [özellikleri örnekleme](app-insights-sampling.md). 
* **Günlük sınır:** Azure portalından bu Application Insights kaynağı oluşturma ayarlandığında 100 GB/gün. Visual Studio'da Application Insights kaynağı oluştururken, varsayılan faciliate yalnızca sınama amaçlı küçük (yalnızca 32.3 MB/gün). Bu durumda uygulamayı üretim ortamına dağıtmadan önce kullanıcının günlük ucun oluşturacağı yöneliktir. Bir yüksek trafik uygulama için daha yüksek bir maksimum istediniz sürece, üst sınır uygulanıyor 1000 GB/gün olur. Maksadınızı olması gerektiği gibi günlük ucun ayarlarken dikkatli **hiç günlük ucun isabet**, çünkü daha sonra geri kalanı gün için veri kaybı ve uygulamanızı izlemek üzere oluşturulamıyor. Değiştirmek için veri birimi yönetim dikey (aşağıya bakın) bağlı günlük birimi cap dikey penceresini kullanın. Bazı abonelik türleri için Application Insights kullanılan kredi gerektiğini unutmayın. Abonelik bir harcama sınırı varsa, günlük sınır dikey penceresinde kaldırmak ve 32.3 MB/gün çıkarılmasına günlük ucun etkinleştirme yönergeleri sahip olur.  
* **Azaltma:** bu veri oranı 32 başına olay ikinci olarak, 1 dakika içinde ölçülen ortalama k için sınırlar. 


*Uygulamam azaltma oranı aşarsa ne olur?*

* Uygulamanızı dakikada uygunluk gönderir veri hacmi. Dakika içinde ölçülen ortalama saniyede oranı aşarsa, sunucunun bazı istekleri reddeder. SDK verileri arabelleğe alır ve ardından yeniden göndermek birkaç dakika boyunca bir dalgalanma yaymak çalışır. Uygulamanızı sürekli veri azaltma hızı yukarıda gönderirse, bazı verisi bırakılacak. (ASP.NET, Java ve JavaScript SDK'ları bu yolla yeniden göndermeyi deneyin; diğer SDK yalnızca kısıtlanan açılan veri olabilir.) Azaltma meydana gelirse, bu gerçekleştirilmedi uyarı bir bildirim görürsünüz.

*Uygulamam gönderme ne kadar verinin nasıl biliyor musunuz?*

* Açık **veri birimi Yönetimi** günlük veri birimi grafik görmek için dikey. 
* Ölçümleri Gezgini'nde, yeni bir grafik ekleyin veya seçin **veri noktası birimi** kendi ölçüm olarak. Gruplandırılması geçin ve group by **veri türü**.

## <a name="to-reduce-your-data-rate"></a>Veri hızını azaltmak için
Veri biriminiz azaltmak için yapabileceğiniz bazı noktalar şunlardır:

* Kullanım [örnekleme](app-insights-sampling.md). Bu teknoloji, ölçümlerinizi eğriltme ve arama ilgili öğeleri arasında gezinme özelliği kesintiye olmadan veri oranı azaltır. Sunucu uygulamaları otomatik olarak çalışır.
* [Bildirilebilir Ajax çağrılarının sayısını sınırlamak](app-insights-javascript.md#detailed-configuration) her sayfa görünümü veya Ajax Raporlama Kapalı anahtarı.
* Koleksiyon modülleri tarafından gerekmez kapalı geçiş [Applicationınsights.config düzenleme](app-insights-configuration-with-applicationinsights-config.md). Örneğin, performans sayaçlarını veya bağımlılık veri inessential karar verebilirsiniz.
* İzleme anahtarı ayırmak için telemetrinizi bölün. 
* Önceden toplama ölçümleri. Uygulamanızda TrackMetric çağrıları yerleştirdiyseniz, ortalama, hesaplama ve ölçümleri toplu standart sapmasını kabul aşırı kullanarak trafiğini azaltabilir. Veya, kullanabileceğiniz bir [önceden toplama paketi](https://www.myget.org/gallery/applicationinsights-sdk-labs).

## <a name="managing-the-maximum-daily-data-volume"></a>Maksimum günlük veri hacmi yönetme

Toplanan verileri sınırlamak için günlük birimi ucun kullanabilirsiniz, ancak ucun karşılanırsa uygulamanızdan günün geri kalanı gönderilen tüm telemetery kaybıyla sonuçlanır. Bu **değil önerilir** onu isabet sonra uygulamanızın performansını ve sistem durumu izleyemiyor olduğundan günlük ucun isabet uygulamanıza sağlamak için. 

Bunun yerine, kullanın [örnekleme](app-insights-sampling.md) düzeyine veri hacmi ayarlamak için gibi ve durumda, uygulamanızın telemetery kadar yüksek hacimli beklenmedik bir şekilde göndermeye başlar günlük ucun "son çare olarak" kullanın. 

Uygulama Insihgts kaynağınız Yapılandır bölümünde günlük cap değiştirmek için tıklatın **veri birimi Yönetimi** sonra **günlük Cap**.

![Günlük telemetri birim ucu ayarlama](./media/app-insights-pricing/daily-cap.png) 

## <a name="sampling"></a>Örnekleme
[Örnekleme](app-insights-sampling.md) telemetri gönderilme uygulamanız için hala bulmasını tanılama aramaları sırasında ilgili olaylar korurken hızının düşürülmesi, yöntemidir ve hala tutma doğru olay sayar. 

Örnekleme giderlerini azaltmak ve aylık kotanızı içinde kalmak için etkili bir yoldur. Arama kullandığınızda, örneğin, belirli bir özel durumla ilişkili istek bulabilmesi için örnekleme algoritması telemetri, ilgili öğeler korur. Ölçüm Gezgini istek hızları, özel durum hızları ve diğer sayılar için doğru değerleri görebilmesi için algoritma doğru sayıları de korur.

Örnekleme çeşitli biçimlerde vardır.

* [Uyarlamalı örnekleme](app-insights-sampling.md) uygulamanızı gönderdiği telemetri birime otomatik olarak ayarlar ASP.NET SDK için varsayılandır. Böylece ağdaki telemetri trafiği azaltılır SDK web uygulamanızda otomatik olarak çalışır. 
* *Alım örnekleme* burada uygulamanızdan telemetrinin girer Application Insights hizmeti noktada çalışır alternatiftir. Telemetriyi uygulamanızdan gönderilen hacmi etkilemez, ancak hizmet tarafından korunan birim azaltır. Tarayıcılar ve diğer SDK telemetri tarafından kullanılan kota azaltmak için kullanabilirsiniz.

Alım örnekleme ayarlamak için fiyatlandırma dikey penceresinde denetimi ayarlayın:

![Kota ve fiyatlandırma dikey örnekleri kutucuğa tıklayın ve örnekleme kesir seçin.](./media/app-insights-pricing/04.png)

> [!WARNING]
> Veri örnekleme dikey penceresi yalnızca alım örnekleme değerini denetler. Uygulamanıza Application Insights SDK'sı tarafından uygulanan örnekleme hızını yansıtmaz. Gelen telemetri SDK örneklenen alım örnekleme uygulanmaz.
> 

Uygulanmış burada olsun gerçek örnekleme hızını keşfetmek için bir [Analytics sorgu](app-insights-analytics.md) bu gibi:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h) 
    | render areachart 

Her kaydı tutulur `itemCount` temsil eder, özgün kayıt sayısını gösteren 1 + önceki atılan kayıt sayısı eşit. 


## <a name="automation"></a>Otomasyon

Azure kaynak Yönetimi'ni kullanarak fiyat planı ayarlamak için bir komut dosyası yazabilirsiniz. [Bilgi nasıl](app-insights-powershell.md#price).

## <a name="limits-summary"></a>Sınırları özeti
[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Örnekleme](app-insights-sampling.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: app-insights-overview.md
[pricing]: http://azure.microsoft.com/pricing/details/application-insights/

