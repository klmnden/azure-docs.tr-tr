---
title: "Akıllı algılama - Application ınsights'ta hatası anormallikleri | Microsoft Docs"
description: "Web uygulamanız için başarısız istekleri oranını olağan dışı değişiklikleri uyarır ve tanılama analizini sağlar. Herhangi bir yapılandırma gerekmez."
services: application-insights
documentationcenter: 
author: yorac
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: mbullwin
ms.openlocfilehash: ca484f4d11cf8ab18db2d0c6152f369a90311f10
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="smart-detection---failure-anomalies"></a>Akıllı algılama - hatası anormallikleri
[Application Insights](app-insights-overview.md) otomatik olarak web uygulamanızı başarısız istekleri oranını olağan dışı bir artışa karşılaşırsa yakın gerçek zamanlı olarak bildirir. Olağan dışı bir HTTP isteklerini veya başarısız olarak rapor bağımlılık çağrıları oranını artışa algılar. İstekler için başarısız olan istekler genellikle yanıt kodları 400 veya daha yüksek olan dosyalardır. Önceliklendirme ve sorunu tanılamak yardımcı olması için hataları ve ilgili telemetri özelliklerini analizini bildiriminde sağlanır. Daha fazla tanı için Application Insights portalına bağlantıları vardır. Normal hata oranı tahmin etmek için makine öğrenimi algoritmalarını kullanır gibi özelliğinin hiçbir Kurulum ya da yapılandırması gerekir.

Bu özellik, bulutta veya kendi sunucularda barındırılan, Java ve ASP.NET web uygulamaları için kullanılabilir. Çağıran çalışan rolü varsa, örneğin, istek veya bağımlılık telemetri - oluşturduğu herhangi bir uygulama için de çalıştığını [TrackRequest()](app-insights-api-custom-events-metrics.md#trackrequest) veya [TrackDependency()](app-insights-api-custom-events-metrics.md#trackdependency).

Ayarladıktan sonra [projeniz için Application Insights](app-insights-overview.md), ve uygulamanızı belirli bir minimum miktar telemetri oluşturur sağlanan akıllı algılama hatası anormallikleri açılana ve uyarıları göndermek için önce uygulamanızın normal davranışını öğrenmek için 24 saat sürer.

Burada, örnek uyarıyı verilmiştir.

![Küme çözümleme hatası geçici gösteren örnek akıllı algılaması Uyarısı](./media/app-insights-proactive-failure-diagnostics/013.png)

> [!NOTE]
> Varsayılan olarak, bu örnek daha kısa bir biçim posta alın. Ancak [geçiş bu ayrıntılı biçimine](#configure-alerts).
>
>

Size bildirir dikkat edin:

* Normal uygulama davranışına göre hata oranı.
* Kaç kullanıcı – ne kadar endişelenmenize gerek bilmesi etkilenir.
* Hatalarla ilgili özellik deseni. Bu örnekte, belirli yanıt kodu, istek adı (işlem) ve uygulamanın sürüm yoktur. Bu hemen, kodunuzda arayan nerede başlatılacağını anlatır. Diğer olasılıklar belirli bir tarayıcı veya istemci işletim sistemi olabilir.
* Özel durum, günlük izlemelerini ve bağımlılık hatası (veritabanları veya diğer dış bileşenlere), karakterize hatalarıyla ilgili olarak görünür.
* Application Insights telemetrisi ilgili aramaları doğrudan bağlantılar.

## <a name="benefits-of-smart-detection"></a>Akıllı algılama yararları
Sıradan [ölçüm uyarıları](app-insights-alerts.md) bir sorun olabilir söyleyin. Ancak akıllı algılama, aksi takdirde kendiniz yapmak olurdu analiz çok gerçekleştirmek için tanılama iş başlatır. Düzgün paketlenmiş sonuçları hızla sorun köküne almak için Yardım alın.

## <a name="how-it-works"></a>Nasıl çalışır?
Akıllı algılama izler uygulamanızdan ve özellikle alınan telemetri başarısızlık oranları. Bu kural istekleri için sayar `Successful request` özelliği false olduğunda ve kendisi için çağrı bağımlılık sayısı `Successful call` özellik değer false. Varsayılan olarak, istekleri için `Successful request == (resultCode < 400)` (için özel kod yazmıştır sürece [filtre](app-insights-api-filtering-sampling.md#filtering) ya da kendi oluşturma [TrackRequest](app-insights-api-custom-events-metrics.md#trackrequest) çağrıları). 

Uygulamanızın performansı tipik bir düzen davranışının sahiptir. Bazı istekleri veya bağımlılık çağrıları diğerlerinden hatalarına karşı daha eğilimlidir olacaktır; ve genel hata oranı yük arttıkça gidebilir. Akıllı algılama machine learning bu anormallikleri bulmak kullanır.

Telemetri web uygulamanızdan Application Insights geldiğinde akıllı algılama şu anki davranışı son birkaç gün içinde görülen modelleri ile karşılaştırır. Hata oranı olağan dışı bir artışa by comparison with önceki performans gözlenir, analiz tetiklenir.

Analiz tetiklendiğinde hizmeti bir küme analizi hataları niteleyen değerlerin bir desen belirlemeye başarısız istek üzerinde gerçekleştirir. Yukarıdaki örnekte, analiz, çoğu hata belirli Sonuç kodu, istek adı, sunucu URL'si ana bilgisayar ve rol örneği hakkında olduğunu buldu. Bunun aksine, çözümleme için istemci işletim sistemi özelliği birden çok değer dağıtılır ve bu nedenle listelenmez buldu.

Hizmetiniz bu telemetri aramaları izlenmiş olan, bir özel durum ve bu isteklerle ilişkili tüm izleme günlüklerini örneği ile birlikte, tanımlanan kümedeki isteklerle ilişkili bağımlılık hatası analyser arar.

Ortaya çıkan çözümleme değil yapılandırmadığınız sürece, uyarı olarak gönderilir.

Gibi [el ile ayarladığınız uyarıları](app-insights-alerts.md), uyarının durumunu inceleyin ve Application Insights kaynağınıza uyarıları dikey penceresinde yapılandırın. Ancak diğer uyarılar, ayarlayın veya akıllı algılama yapılandırmak gerekmez. İsterseniz, devre dışı bırakın ya da kendi hedef e-posta adreslerini değiştirin.

## <a name="configure-alerts"></a>Uyarıları yapılandırma
Akıllı algılama devre dışı bırakmak, e-posta alıcılarını değiştirmek, bir Web kancası oluşturma veya ayrıntılı uyarı iletileri kabul.

Uyarılar sayfasında açın. El ile ayarlayın ve, şu anda uyarı durumunda olup olmadığını görebilirsiniz herhangi bir uyarı birlikte hatası anormallikleri eklenir.

![Genel bakış sayfasında, uyarıları kutucuğa tıklayın. Veya herhangi bir ölçümleri sayfasında Uyarılar düğmesini tıklatın.](./media/app-insights-proactive-failure-diagnostics/021.png)

Uyarıyı yapılandırmak için tıklatın.

![Yapılandırma](./media/app-insights-proactive-failure-diagnostics/032.png)

Akıllı algılama devre dışı bırakabilir, ancak bunu silemezsiniz dikkat edin (veya başka bir tane oluşturun).

#### <a name="detailed-alerts"></a>Ayrıntılı uyarıları
"Daha ayrıntılı tanılama Al" ı seçerseniz e-posta daha fazla tanı bilgilerini içerir. Bazı durumlarda yalnızca e-postadaki verilerden sorunu tanılamak mümkün olur.

Özel durum ve izleme iletilerini içerdiği için ayrıntılı uyarı hassas bilgiler içerebilir hafif bir risk yoktur. Kodunuzu bu iletilerine hassas bilgiler verebilecek ancak, bu yalnızca gerçekleşir.

## <a name="triaging-and-diagnosing-an-alert"></a>Önceliklendirme ve bir uyarı tanılama
Başarısız istek oranı olağan dışı bir artışa algılandığını bildiren bir uyarı gösterir. Uygulamanızı veya kendi ortam ile ilgili bazı sorunlar olduğunu olasıdır.

İsteklerin ve etkilenen kullanıcıların sayısını sınıflandırılacak nasıl sorun acildir karar verebilirsiniz. Yukarıdaki örnekte, %22.5 hata oranı normal %1 hızıyla karşılaştıran, hatalı bir şey olacağına olduğunu gösterir. Diğer taraftan, yalnızca 11 kullanıcılar bundan etkilendi. Uygulamanızı olsaydı, ne kadar ciddi olduğu değerlendirmek gerçekleştirebilir.

Çoğu durumda, istek adı, özel durum, sağlanan bağımlılık hatası ve izleme verileri çubuğundan hızlı bir şekilde sorunu tanılamak mümkün olacaktır.

Diğer bazı ipuçları vardır. Örneğin, bu örnekte bağımlılık hatası oranı (%89.3) özel durum oranı aynıdır. Bu özel durumun doğrudan bağımlılık hatası - kodunuzda aramaya başlamak burada NET bir fikir veren gelen ortaya önerir.

Daha fazla araştırmak için her bölümdeki bağlantılar sizi için düz yönlendirir bir [arama sayfası](app-insights-diagnostic-search.md) ilgili istekleri, özel durum, bağımlılık veya izlemeleri filtrelenir. Veya açabilirsiniz [Azure portal](https://portal.azure.com), uygulamanız için Application Insights kaynağı gidin ve hataları dikey penceresini açın.

Bu örnekte 'bağımlılık hataları Ayrıntıları Görüntüle' bağlantısını tıklatarak Application Insights arama dikey pencere açılır. Kök nedeni örneği olan SQL deyimini gösterir: null değerlere zorunlu alanlarda sağlanan ve kaydetme sırasında doğrulamayı geçemedi işlemi.

![Tanılama arama](./media/app-insights-proactive-failure-diagnostics/051.png)

## <a name="review-recent-alerts"></a>Son uyarıları gözden geçirin

Tıklatın **akıllı algılama** en son uyarı almak için:

![Uyarılar özeti](./media/app-insights-proactive-failure-diagnostics/070.png)


## <a name="whats-the-difference-"></a>Fark nedir...
Akıllı algılama hatası anormallikleri tamamlar diğer benzer ancak ayrı özelliklerini Application Insights.

* [Ölçüm uyarıları](app-insights-alerts.md) tarafından ayarlanır ve çok çeşitli ölçümleri CPU doluluğu, istek hızları, sayfa yükleme sürelerinin ve benzerleri gibi izleyebilirsiniz. Daha fazla kaynak eklemeniz gerekir, örneğin, uyarmak için kullanabilirsiniz. Bunun aksine, web uygulamanızın başarısız sonra gerçek zamanlı şekilde yakın, web uygulamanızın normal davranışın önemli ölçüde karşılaştırıldığında hızı artar isteği bildirmek için tasarlanmış küçük bir aralık kritik ölçümleri (şu anda yalnızca başarısız istek oranı), akıllı algılama hatası anormallikleri kapsar.

    Akıllı algılama eşiğini yanıt geçerli koşullar olarak otomatik olarak ayarlar.

    Akıllı algılama, tanılama iş başlatır.
* [Akıllı performans anormallikleri](app-insights-proactive-performance-diagnostics.md) de Intelligence ölçümlerinizi olağan dışı kalıpları bulmak için kullandığı makine ve sizin tarafınızdan yapılandırma gereklidir. Ancak hata anormallikleri akıllı algılama, performans anormallikleri akıllı algılama amacı belirli bir tarayıcı türüne kesimleri hatalı hizmet, kullanım topluluğu, - örneğin, belirli sayfaları bulmak için. Hiçbir sonuç bulunursa, büyük olasılıkla bir uyarı daha az Acil ve analiz günlük gerçekleştirilir. Bunun aksine, çözümleme hatası anormallikleri için sürekli olarak gelen telemetriyi gerçekleştirilir ve sunucu başarısızlık oranları beklenenden daha büyük olduğunda dakika içinde bildirilir.

## <a name="if-you-receive-a-smart-detection-alert"></a>Bir akıllı algılaması uyarısı alırsanız
*Neden bu uyarı aldınız?*

* Başarısız istek oranı önündeki nokta normal taban çizgisine göre olağan dışı bir artışa algıladık. İlişkili telemetri ve hataları çözümleme sonrasında, içine görünmelidir bir sorun olduğunu düşünün.

*Bildirim kesinlikle bir sorun sahibim anlama geliyor?*

* Uygulama kesintisi ya da azalma uyarı deneyin, ancak yalnızca, tam olarak semantiğini ve uygulama veya kullanıcılar üzerindeki etkisini anlamak.

*Bu nedenle, yazarlar verilerimi göz önünde bulundurmanız?*

* Hayır. Hizmet tamamen otomatik olarak yapılır. Yalnızca bildirimleri alır. Verileriniz [özel](app-insights-data-retention-privacy.md).

*Bu uyarıya abone var mı?*

* Hayır. Her uygulama gönderdiği telemetri isteği akıllı algılama uyarı kuralı sahiptir.

*Aboneliği veya miyim my iş arkadaşlarınızı yerine gönderilen bildirimleri almak?*

* Evet, içinde uyarı kuralları akıllı algılama kuralı yapılandırmak için tıklatın. Uyarıyı devre dışı bırakmak veya alıcılar için uyarı değiştirin.

*I e-posta kesildi. Portalda bildirimleri nereden bulabilirim?*

* Etkinlik günlükleri. Azure, uygulamanız için Application Insights kaynağı açın, ardından etkinlik günlükleri seçin.

*Bilinen sorunlar hakkında uyarı bazılarıdır ve bunları almak istemiyorsanız.*

* Bizim biriktirme listesi üzerinde uyarı gizleme sunuyoruz.

## <a name="next-steps"></a>Sonraki adımlar
Bu tanılama araçları, uygulamanızdan alınan telemetri incelemek yardımcı olur:

* [Ölçüm Gezgini](app-insights-metrics-explorer.md)
* [Arama Gezgini](app-insights-diagnostic-search.md)
* [Analizi - güçlü sorgu dili](app-insights-analytics-tour.md)

Akıllı algılamaların tamamen otomatik olarak yapılır. Ancak, belki de daha fazla bazı uyarıları ayarlamak ister misiniz?

* [El ile yapılandırılmış ölçüm uyarıları](app-insights-alerts.md)
* [Kullanılabilirlik web testleri](app-insights-monitor-web-app-availability.md)
