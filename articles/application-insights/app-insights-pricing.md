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
ms.openlocfilehash: 42c54509cedf48e9ebabb5b50865ed56e54bee05
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="manage-pricing-and-data-volume-in-application-insights"></a>Application Insights fiyatlandırma ve veri biriminde yönetme

Fiyatlandırmasını [Azure Application Insights] [ start] uygulama başına veri hacmi dayanır. Her bir Application Insights kaynağı ayrı bir hizmet ücretlendirilir ve Azure aboneliğiniz için fatura katkıda bulunur.

İki fiyatlandırma planı vardır. Varsayılan plan tüm alınan veri biriminde öncelikle hiçbir ek maliyet ve faturalar Kurumsal plan aynı özellikler içeren temel adı verilir. Operations Management Suite kullanıyorsanız, bir düğüm başına sahip plan günlük veri kesintileri birlikte gider ve ardından dahil indirimi alınan veriler için ücretinin kuruluş için tercih.

Fiyatlandırma için Application Insights nasıl çalıştığı hakkında sorularınız varsa, bir soru gönderin çekinmeyin bizim [Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=ApplicationInsights).

## <a name="the-price-plans"></a>Fiyat planları

Bkz: [Application Insights fiyatlandırma sayfası] [ pricing] geçerli fiyatlar para birimi ve bölge için.

### <a name="basic-plan"></a>Temel plan

Yeni bir Application Insights kaynak oluşturulduğunda ve bir Operations Management Suite aboneliği olanlar dışında tüm müşteriler için uygundur temel plan varsayılandır.

* Temel planda veri hacmine göre ücretlendirilen: Application Insights tarafından alınan telemetri bayt sayısı. Veri birimi Application Insights tarafından uygulamanızdan alınan sıkıştırılmamış JSON veri paketi boyutu olarak ölçülür.
İçin [tablo veri analizi içeri](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-import), veri hacmi Application Insights'a gönderilen dosyaların sıkıştırılmamış boyut olarak ölçülür.
* [Canlı ölçümleri akış](app-insights-live-stream.md) veri yok sayılan amacıyla fiyatlandırma için.
* [Sürekli verme](app-insights-export-telemetry.md) ve [günlük analizi bağlayıcı](https://go.microsoft.com/fwlink/?LinkId=833039&amp;clcid=0x409) Nisan 2018 itibariyle temel plan, ek bir ücret yer alır.

### <a name="enterprise-plan-and-operations-management-suite-subscription-entitlements"></a>Kurumsal plan ve Operations Management Suite aboneliği yetkilendirmeler

Microsoft Operations Management Suite E1 ve E2 satın müşteriler ek bileşen hiçbir ek ücret ödemeden olarak uygulama Öngörüler Kurumsal alabilir [önceden duyurulmuş](https://blogs.technet.microsoft.com/msoms/2017/05/19/azure-application-insights-enterprise-as-part-of-operations-management-suite-subscription/). Özellikle, Operations Management Suite E1 ve E2 her birimi 1 düğüme bir yetkilendirme Application Insights Kurumsal planının içerir. Aşağıda her Application Insights düğüm daha ayrıntılı olarak açıklandığı gibi en fazla 200 MB günde (ayrı), günlük analizi veri alımı hiçbir ek ücret ödemeden 90 günlük veri saklama ile alınan verileri içerir. Bu yalnızca bir Operations Management Suite aboneliği sahip müşteriler için geçerli olduğuna göre bir abonelik olmadan müşteriler bu plana seçmek için bir seçenek görmez.

> [!NOTE]
> Bu yetkilendirme aldığından emin olmak için Application Insights kaynaklarınızı kuruluştaki planı fiyatlandırma olmalıdır. Temel plan Application Insights kaynaklarında bir fayda fark olmayan şekilde bu yetkilendirme yalnızca düğümleri olarak uygulanır. Bu yetkilendirme gösterilen tahmini maliyetleri görünür olmaz Not *kullanım ve tahmini maliyet* sayfası. Ayrıca, bir abonelik Nisan 2018 fiyatlandırma modelini izleme yeni Azure taşırsanız, bir Operations Management Suite aboneliği varsa önerilmez şekilde temel plan yalnızca plan kullanılabilir olacaktır.

Kurumsal planı hakkında daha fazla ayrıntı şu adresi ziyaret edin [Kurumsal fiyatlandırma ayrıntıları sayfası.](app-insights-pricing-enterprise-details.md)

### <a name="multi-step-web-tests"></a>Çok adımlı web testleri

İçin ek bir ücret yoktur [çok adımlı web testleri](app-insights-monitor-web-app-availability.md#multi-step-web-tests). Bu, bir dizi eylemi gerçekleştirmek için web testleri ifade eder.

Tek bir sayfayla 'ping sınaması' için ayrı bir ücret ödemeden yoktur. Diğer telemetriyi uygulamanızdan birlikte ping testleri ve çok adımlı testleri telemetrisinden ücretlendirilir.

## <a name="review-pricing-plans-and-estimate-costs"></a>Fiyatlandırma planlarını gözden geçirin ve maliyetleri tahmin etme

Application Insights kullanılabilir fiyatlandırma planları ve hangi maliyetleri büyük olasılıkla son kullanım düzenlerini esas tabanlı anlamak kolaylaştırır. Başlangıç açarak **kullanım ve tahmini maliyetleri** Azure portalında Application Insights kaynağı sayfasında:

![Fiyatlandırma seçin.](./media/app-insights-pricing/pricing-001.png)

**a.** Veri biriminiz ayı için gözden geçirin. Bu alındı ve korunan tüm verileri içerir (herhangi bir sonra [örnekleme](app-insights-sampling.md) sunucu ve istemci uygulamalarını ve kullanılabilirlik testleri.

**b.** Ayrı bir ücret yapılan [çok adımlı web testleri](app-insights-monitor-web-app-availability.md#multi-step-web-tests). (Bu, veri birimi bir ücret alınmaz dahil basit kullanılabilirlik testleri içermez.)

**c.** Geçen ay için veri birimi eğilimleri görüntüleme.

**d.** Veri alımı etkinleştirmek [örnekleme.](app-insights-sampling.md) 

**e.** Günlük veri birimi ucun yapılandırın.

Uygulama Öngörüler ücretler Azure faturanızı eklenir. Azure Faturalama bölümünde Azure portalının ya da buna fatura ayrıntılarını görebilirsiniz [Azure Billing Portal](https://account.windowsazure.com/Subscriptions). 

![Yan menüde faturalama seçin.](./media/app-insights-pricing/02-billing.png)

## <a name="data-rate"></a>Veri oranı
Verileri gönderme birim sınırlıdır üç yolu vardır:

* **Örnekleme:** bu düzenek kullanılabilir sunucu ve istemci uygulamalarınızı en az bozulma ölçümleri ile gönderilen telemetri miktarını azaltın. Bu, veri miktarını ayarlamak için sahip birincil araçtır. Daha fazla bilgi edinmek [özellikleri örnekleme](app-insights-sampling.md). 
* **Günlük sınır:** Azure portalından bu Application Insights kaynağı oluşturma ayarlandığında 100 GB/gün. Visual Studio'da Application Insights kaynağı oluştururken, varsayılan yalnızca sınama kolaylaştırmak için tasarlanmıştır (yalnızca 32.3 MB/gün) küçük. Bu durumda uygulamayı üretim ortamına dağıtmadan önce kullanıcının günlük ucun oluşturacağı yöneliktir. Bir yüksek trafik uygulama için daha yüksek bir maksimum istediniz sürece, üst sınır uygulanıyor 1000 GB/gün olur. Maksadınızı olması gerektiği gibi günlük ucun ayarlarken dikkatli **hiç günlük ucun isabet**, çünkü daha sonra geri kalanı gün için veri kaybı ve uygulamanızı izlemek üzere oluşturulamıyor. Değiştirmek için kullanım ve tahmini maliyetleri sayfa (aşağıya bakın) bağlı günlük birimi uç seçeneği kullanın. Kısıtlama için Application Insights kullanılamadı alacak bazı abonelik türlerinde kaldırdık. Abonelik bir harcama sınırı varsa, daha önce günlük sınır iletişim yönergeleri kaldırın ve 32.3 MB/gün çıkarılmasına günlük ucun etkinleştirme sahip olur.
* **Azaltma:** bu ikinci, ortalama üzerinde 1 dakika başına 32.000 olayları veri oranı sınırlar.

*Uygulamam azaltma oranı aşarsa ne olur?*

* Uygulamanızı dakikada uygunluk gönderir veri hacmi. Dakika içinde ölçülen ortalama saniyede oranı aşarsa, sunucunun bazı istekleri reddeder. SDK verileri arabelleğe alır ve ardından yeniden göndermek birkaç dakika boyunca bir dalgalanma yaymak çalışır. Uygulamanızı sürekli veri azaltma hızı yukarıda gönderirse, bazı verisi bırakılacak. (ASP.NET, Java ve JavaScript SDK'ları bu yolla yeniden göndermeyi deneyin; diğer SDK yalnızca kısıtlanan açılan veri olabilir.) Azaltma meydana gelirse, bu gerçekleştirilmedi uyarı bir bildirim görürsünüz.

*Uygulamam gönderme ne kadar verinin nasıl biliyor musunuz?*

* Açık **kullanım ve tahmini maliyet** günlük veri birimi grafik görmek için sayfayı. 
* Ölçümleri Gezgini'nde, yeni bir grafik ekleyin veya seçin **veri noktası birimi** kendi ölçüm olarak. Gruplandırılması geçin ve group by **veri türü**.

## <a name="to-reduce-your-data-rate"></a>Veri hızını azaltmak için
Veri biriminiz azaltmak için yapabileceğiniz bazı noktalar şunlardır:

* Kullanım [örnekleme](app-insights-sampling.md). Bu teknoloji, ölçümlerinizi eğriltme ve arama ilgili öğeleri arasında gezinme özelliği kesintiye olmadan veri oranı azaltır. Sunucu uygulamaları otomatik olarak çalışır.
* [Bildirilebilir Ajax çağrılarının sayısını sınırlamak](app-insights-javascript.md#detailed-configuration) her sayfa görünümü veya Ajax Raporlama Kapalı anahtarı.
* Koleksiyon modülleri tarafından gerekmez kapalı geçiş [Applicationınsights.config düzenleme](app-insights-configuration-with-applicationinsights-config.md). Örneğin, performans sayaçlarını veya bağımlılık veri inessential karar verebilirsiniz.
* İzleme anahtarı ayırmak için telemetrinizi bölün. 
* Önceden toplama ölçümleri. Uygulamanızda TrackMetric çağrıları yerleştirdiyseniz, ortalama, hesaplama ve ölçümleri toplu standart sapmasını kabul aşırı kullanarak trafiğini azaltabilir. Veya, kullanabileceğiniz bir [önceden toplama paketi](https://www.myget.org/gallery/applicationinsights-sdk-labs).

## <a name="managing-the-maximum-daily-data-volume"></a>Maksimum günlük veri hacmi yönetme

Toplanan verileri sınırlamak için günlük birimi ucun kullanabilirsiniz, ancak ucun karşılanırsa uygulamanızdan günün geri kalanı gönderilen tüm telemetri kaybıyla sonuçlanır. Bu **değil önerilir** onu isabet sonra uygulamanızın performansını ve sistem durumu izleyemiyor olduğundan günlük ucun isabet uygulamanıza sağlamak için.

Bunun yerine, kullanın [örnekleme](app-insights-sampling.md) düzeyine veri hacmi ayarlamak için gibi ve durumda, uygulamanızın telemetri kadar yüksek hacimli beklenmedik bir şekilde göndermeye başlar günlük ucun "son çare olarak" kullanın.

Application Insights kaynağınıza Yapılandır bölümünde günlük cap değiştirmek için **kullanım ve tahmini maliyetleri** sayfasında, **günlük Cap**.

![Günlük telemetri birim ucu ayarlama](./media/app-insights-pricing/pricing-003.png)

## <a name="sampling"></a>Örnekleme
[Örnekleme](app-insights-sampling.md) telemetri gönderilme uygulamanız için hala bulmasını tanılama aramaları sırasında ilgili olaylar korurken hızının düşürülmesi, yöntemidir ve hala tutma doğru olay sayar.

Örnekleme giderlerini azaltmak ve aylık kotanızı içinde kalmak için etkili bir yoldur. Arama kullandığınızda, örneğin, belirli bir özel durumla ilişkili istek bulabilmesi için örnekleme algoritması telemetri, ilgili öğeler korur. Ölçüm Gezgini istek hızları, özel durum hızları ve diğer sayılar için doğru değerleri görebilmesi için algoritma doğru sayıları de korur.

Örnekleme çeşitli biçimlerde vardır.

* [Uyarlamalı örnekleme](app-insights-sampling.md) uygulamanızı gönderdiği telemetri birime otomatik olarak ayarlar ASP.NET SDK için varsayılandır. Böylece ağdaki telemetri trafiği azaltılır SDK web uygulamanızda otomatik olarak çalışır. 
* *Alım örnekleme* burada uygulamanızdan telemetrinin girer Application Insights hizmeti noktada çalışır alternatiftir. Telemetriyi uygulamanızdan gönderilen hacmi etkilemez, ancak hizmet tarafından korunan birim azaltır. Tarayıcılar ve diğer SDK telemetri tarafından kullanılan kota azaltmak için kullanabilirsiniz.

Alım örnekleme ayarlamak için fiyatlandırma iletişim kutusunda denetimi ayarlayın:

![Kota ve fiyatlandırma iletişim örnekleri kutucuğa tıklayın ve örnekleme kesir seçin.](./media/app-insights-pricing/pricing-004.png)

> [!WARNING]
> Veri örnekleme iletişim yalnızca alım örnekleme değerini denetler. Uygulamanıza Application Insights SDK'sı tarafından uygulanan örnekleme hızını yansıtmaz. Gelen telemetri SDK örneklenen alım örnekleme uygulanmaz.
>

Uygulanmış burada olsun gerçek örnekleme hızını keşfetmek için bir [Analytics sorgu](app-insights-analytics.md) bu gibi:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h)
    | render areachart

Her kaydı tutulur `itemCount` temsil eder, özgün kayıt sayısını gösteren 1 + önceki atılan kayıt sayısı eşit. 

## <a name="automation"></a>Otomasyon

Azure kaynak Yönetimi'ni kullanarak fiyat planı ayarlamak için bir komut dosyası yazabilirsiniz. [Nasıl olduğunu öğrenin](app-insights-powershell.md#price).

## <a name="limits-summary"></a>Sınırları özeti

[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Örnekleme](app-insights-sampling.md)

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: app-insights-overview.md
[pricing]: http://azure.microsoft.com/pricing/details/application-insights/
