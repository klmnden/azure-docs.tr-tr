---
title: Akıllı algılama - hata anomalileri, Application ınsights | Microsoft Docs
description: Olağan dışı oranda başarısız istek web uygulamanıza değişiklikleri uyarır ve tanılama analizini sağlar. Herhangi bir yapılandırma gerekmez.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 12/18/2018
ms.reviewer: yossiy
ms.author: mbullwin
ms.openlocfilehash: cfa00504cd2a05985fde2af3357418eac8baceeb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61299104"
---
# <a name="smart-detection---failure-anomalies"></a>Akıllı algılama - hata Anomalileri
[Application Insights](../../azure-monitor/app/app-insights-overview.md) otomatik olarak web uygulamanızın olağandışı başarısız istek oranı artışı karşılaşırsa neredeyse gerçek zamanlı olarak bildirir. Bu, HTTP isteklerini veya başarısız olarak raporlanır bağımlılık çağrıları oranını olağan dışı bir artış algılar. Başarısız istekler, istekleri için yanıt kodları 400 veya daha yüksek olan genellikle biçimindedir. Önceliklendirmenize ve sorunu tanılamanıza yardımcı olmak için hataları ve ilgili telemetriyi özelliklerini analizini bildiriminde sağlanır. Daha ileri tanılama için Application Insights portalına bağlantıları vardır. Normal hata oranı tahmin etmek için makine öğrenimi algoritmaları kullanır gibi özellik Kurulum ya da yapılandırması gerekir.

Bu özellik, bulutta ya da kendi sunucularınızda bulunan Java ve ASP.NET web uygulamaları için çalışır. Çağıran bir çalışan rolü varsa, örneğin, istek veya bağımlılık telemetri - oluşturan herhangi bir uygulama için de çalışır [TrackRequest()](../../azure-monitor/app/api-custom-events-metrics.md#trackrequest) veya [TrackDependency()](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency).

Ayarladıktan sonra [projeniz için Application Insights](../../azure-monitor/app/app-insights-overview.md), ve uygulamanızı belirli bir en düşük miktarda telemetri oluşturur sağlanan hata anomalileri, akıllı algılama silinmeden önce uygulamanızın normal davranışını öğrenmek için 24 saat sürer. açık ve Uyarıları gönderebilirsiniz.

Örnek uyarı aşağıda verilmiştir.

![Küme çözümleme hatası etrafında gösteren örnek akıllı algılama Uyarısı](./media/proactive-failure-diagnostics/013.png)

> [!NOTE]
> Varsayılan olarak, bu örnekten daha kısa biçimi posta alın. Ancak [ayrıntılı bu biçime geçiş](#configure-alerts).
>
>

Size bildirir dikkat edin:

* Normal uygulama davranışına göre hata oranı.
* Kaç kullanıcının - ne kadar endişelenmenize gerek bilmesi etkilenir.
* Hatalarla ilgili özellik deseni. Bu örnekte, belirli bir yanıt kodu, istek adı (işlem) ve uygulama sürümü yok. Bu hemen, kodunuzda arama nerede başlatılacağını anlatır. Diğer olasılıklar, belirli bir tarayıcı veya istemci işletim sistemi olabilir.
* Özel durum, günlük izlemeleri ve bağımlılık hatası (veritabanları veya diğer dış bileşenlere) karakterize hataları ile ilişkilendirilecek görünen.
* İlgili aramalar Application ınsights telemetrisi üzerinde doğrudan bağlantılar.

## <a name="benefits-of-smart-detection"></a>Akıllı algılama avantajları
Sıradan [ölçüm uyarıları](../../azure-monitor/app/alerts.md) size bir sorun olabilir. Ancak, akıllı algılama, aksi takdirde kendiniz yapmak zorunda olabilirsiniz analiz çok fazla gerçekleştirmek için tanılama iş başlatır. Düzgünce paketlenmiş, sonuçları, sorunun kök hızlı bir şekilde almak için Yardım alın.

## <a name="how-it-works"></a>Nasıl çalışır?
Akıllı algılama belirli ve uygulamanızdan alınan telemetri izler hata oranları. Bu kural istekleri için sayar `Successful request` özelliği false olduğunda ve kendisi için çağrılarının bağımlılık sayısı `Successful call` özellik değer false. Varsayılan olarak, istekleri `Successful request == (resultCode < 400)` (özel kod yazdığınız sürece [filtre](../../azure-monitor/app/api-filtering-sampling.md#filtering) ya da kendi oluşturma [TrackRequest](../../azure-monitor/app/api-custom-events-metrics.md#trackrequest) çağırır). 

Uygulamanızın performansını davranış tipik bir düzen vardır. Bazı istekler veya bağımlılık çağrıları diğerlerine göre hataya daha hatalara açık olacaktır. ve genel hata oranı yük arttıkça gidebilir. Akıllı algılama bu anormallikleri bulmak için makine öğrenimini kullanıyor.

Telemetri web uygulamanızdan Application Insights'a geldiğinde, akıllı algılama şu anki davranışı son birkaç gün içinde görülen desenleri ile karşılaştırır. Hata oranı olağandışı by comparison with önceki performans gözlemlendiğinde bir analiz tetiklenir.

Analiz tetiklendiğinde hizmet hataları niteleyen değerleri desenini belirlemek için başarısız istek üzerinde Küme çözümleme gerçekleştirir. Yukarıdaki örnekte, analiz, çoğu hata belirli Sonuç kodu, istek adı, sunucu URL konağı ve rol örneği hakkında olduğunu algıladı. Bunun aksine, analiz, istemci işletim sistemi özelliği birden çok değer dağıtılır ve bu nedenle listelenmez algıladı.

Hizmetinizin telemetri çağrıları ile işaretlenmiş, bir özel durum ve bu isteklerle ilişkili tüm izleme günlüklerini örneği ile birlikte, belirlemiş durumdadır kümedeki isteklerle ilişkili bağımlılık hatası Çözümleyicisi arar.

Sonuçta elde edilen analiz için yapılandırmış olduğunuz sürece, uyarı gönderilir.

Gibi [el ile ayarladığınız uyarıları](../../azure-monitor/app/alerts.md), uyarının durumunu incelemek ve Application Insights kaynağınıza uyarılar dikey penceresinden yapılandırın. Ancak diğer uyarılar farklı olarak ayarlamak veya akıllı algılama yapılandırmak gerekmez. İsterseniz, devre dışı bırakın ya da kendi hedef e-posta adreslerini değiştirin.

### <a name="alert-logic-details"></a>Uyarı mantığı ayrıntıları

Uyarılar için size tam uygulama ayrıntılarını paylaşamaz öğrenme algoritmasına makinemizi özel tarafından tetiklenir. Bu kullanıcıyla Bununla birlikte, bazen temel mantığını nasıl çalıştığı hakkında daha fazla bilgi edinmek için ihtiyacınız olduğunu biliyoruz. Uyarıyı tetikleyen belirlemek için değerlendirilen temel unsurlar şunlardır: 

* İstekleri/bağımlılıkları 20 dakika çalışırken zaman penceresi içinde hata yüzdesi analizi.
* Son 40 dakika ve son yedi gün ve X kez aşan önemli sapmaları için standart sapma isteyen son 20 dakikası oranına hatası yüzdesi karşılaştırması.
* Bir Uyarlamalı sınırı için en düşük hatası yüzdesi kullanarak gerçekleştirmediğine, istekleri/bağımlılıkları, uygulamanın hacmine dayalı.

## <a name="configure-alerts"></a>Uyarı yapılandırma
Akıllı algılama devre dışı bırakın, e-posta alıcılarını değiştirmek, bir Web kancası oluşturma veya ayrıntılı uyarı iletileri kabul et.

Uyarılar sayfasını açın. El ile ayarlamanız ve, o anda uyarı durumunda olduğunu gördüğünüz tüm uyarıların yanı sıra hata Anomalileri dahildir.

![Genel bakış sayfasında, uyarıları kutucuğuna tıklayın. Veya tüm Metrikler sayfasında, uyarıları düğmesine tıklayın.](./media/proactive-failure-diagnostics/021.png)

Bunu yapılandırmak için uyarıya tıklayın.

![Yapılandırma](./media/proactive-failure-diagnostics/032.png)

Akıllı algılama devre dışı bırakabilirsiniz, ancak bunu silemezsiniz dikkat edin (veya başka bir tane oluşturun).

#### <a name="detailed-alerts"></a>Ayrıntılı uyarıları
"Daha ayrıntılı tanılama Al" seçeneğini belirlerseniz e-posta daha fazla tanılama bilgisi içerir. Bazen yalnızca e-posta verilerden sorunu tanılamak mümkün olacaktır.

Özel durum ve izleme iletilerini içerdiğinden daha ayrıntılı uyarı hassas bilgiler içerebilir bir hafif riski yoktur. Kodunuzu hassas bilgileri bu iletilere izin verebilir ancak bu yalnızca gerçekleşir.

## <a name="triaging-and-diagnosing-an-alert"></a>Önceliklendirme ve tanılama uyarı
Bir uyarı, olağandışı başarısız istek oranında olağandışı artışı algılandığını gösterir. Uygulamanızı veya kendi ortamında bir sorun olduğundan emin olma olasılığı yüksektir.

Etkilenen kullanıcı sayısı veya isteklerin sınıflandırılacak nasıl sorun acildir karar verebilirsiniz. Yukarıdaki örnekte, %22.5 hata oranı % 1'bir normal oran ile karşılaştırır, bir sorun olup bittiğini olduğunu gösterir. Öte yandan, yalnızca 11 kullanıcı etkilenmiştir. Uygulamanızı olsaydı, ne kadar ciddi olup olmadığını değerlendirmek mümkün olacaktır.

Çoğu durumda, istek adı, özel durum, sağlanan bağımlılık hatası ve izleme verilerini çubuğundan hızlı bir şekilde sorunu tanılamak mümkün olacaktır.

Diğer bazı ipuçları vardır. Örneğin, bu örnekte bağımlılık hatası oranı (%89.3) özel durum oranı aynıdır. Bu özel durumun doğrudan bağımlılık hatası - kodunuzda bakmaya başlamak burada NET bir fikir veren gelen ortaya önerir.

Daha fazla araştırmak için her bölümdeki bağlantılar, için düz sürer bir [arama sayfası](../../azure-monitor/app/diagnostic-search.md) ilgili istekleri, özel durum, bağımlılık veya izlemeleri filtrelendi. Veya açabileceğiniz [Azure portalında](https://portal.azure.com), uygulamanız için Application Insights kaynağına gidin ve hataları dikey penceresini açın.

Bu örnekte, 'bağımlılık hatalarının Ayrıntıları Görüntüle' bağlantıya tıklamak, Application Insights arama dikey penceresi açılır. Bu örnek olarak kök neden olan SQL deyimi gösterilmektedir: Null değerlere zorunlu alanları sağlandı ve kaydetme sırasında doğrulamayı geçemedi işlemi.

![Tanılama arama](./media/proactive-failure-diagnostics/051.png)

## <a name="review-recent-alerts"></a>Son uyarıları gözden geçirin

Tıklayın **akıllı algılama** en son uyarı almak için:

![Uyarılar özeti](./media/proactive-failure-diagnostics/070.png)


## <a name="whats-the-difference-"></a>Fark nedir...
Hata anomalileri, akıllı algılama tamamlar diğer benzer ancak farklı Application Insights özellikleri.

* [Ölçüm uyarıları](../../azure-monitor/app/alerts.md) sizin tarafınızdan ayarlanır ve çok çeşitli CPU doluluğu, istek hızları, sayfa yükleme süreleri ve benzeri gibi ölçümleri izleyebilirsiniz. Daha fazla kaynak eklemeniz gerekir, örneğin, uyarmak için kullanabilirsiniz. Bunun aksine, neredeyse gerçek zamanlı şekilde sonra web uygulamanızın başarısız oldu, isteği web uygulamasının için önemli ölçüde karşılaştırıldığında hızı artar bildirmek için tasarlanmış küçük bir aralık kritik ölçüm (şu anda yalnızca başarısız istek oranında olağandışı), hata anomalileri, akıllı algılama kapsar Normal davranış.

    Akıllı algılama eşiğini yaygınlaşan koşullarına yanıt otomatik olarak ayarlar.

    Akıllı algılama, tanılama iş başlatır.
* [Akıllı algılama performans anomalilerini](proactive-performance-diagnostics.md) ayrıca kullanan zeka ölçümlerinizi olağan dışı desenlerini bulmak için makine ve sizin tarafınızdan yapılandırma gerekmiyor. Ancak hata anomalileri, akıllı algılama, akıllı algılama performans anomalilerini amacı bölümleri hatalı hizmet, kullanım topluluğu, - örneğin, belirli bir sayfa tarafından belirli bir tarayıcı türünü bulmak için. Analiz günlük gerçekleştirilir ve herhangi bir sonuç bulunmazsa, uyarı daha az Acil olması daha olasıdır. Aksine, hata anomalileri için'analiz, gelen telemetriyi sürekli olarak gerçekleştirilir ve sunucu hata oranları beklenenden daha büyük ise dakika içinde bildirilir.

## <a name="if-you-receive-a-smart-detection-alert"></a>Bir akıllı algılama uyarısı alırsanız
*Bu uyarı neden aldım?*

* Normal önündeki nokta taban çizgisine göre başarısız istek oranı olağandışı tespit ettik. İlişkili telemetri öğelerini ve hataları çözümleme sonrasında, içine görünmelidir bir sorun olduğunu düşünüyoruz.

*Bildirim kesinlikle bir sorun yaşamıyorum anlama geliyor?*

* Uygulama kesintisi veya performans düşüşü uyarı deneyin ancak yalnızca tam olarak semantiği ve uygulama ya da kullanıcılar üzerindeki etkiyi anladığınızdan.

*Bu nedenle, guys my verilere bakmak?*

* Hayır. Hizmeti tamamen otomatik olarak yapılır. Yalnızca bildirimleri alın. Verileriniz [özel](../../azure-monitor/app/data-retention-privacy.md).

*Bu uyarıya abone gerekiyor mu?*

* Hayır. Her uygulamanın gönderdiği telemetri isteği akıllı algılama uyarı kuralı vardır.

*Aboneliği veya miyim arkadaşlarım için bunun yerine gönderilen bildirimleri alın?*

* Evet, içinde uyarı kuralları yapılandırmak için akıllı algılama kuralına tıklayın. Uyarıyı devre dışı bırakın veya alıcılar için uyarı değiştirin.

*Ben, e-posta kaybolur. Portalda bildirimleri nerede bulabilirim?*

* Etkinlik günlükleri. Azure'da uygulamanız için Application Insights kaynağını açın ve ardından etkinlik günlükleri'ni seçin.

*Bazı uyarılar hakkında bilinen sorunlar var ve bunları almak istemiyorum.*

* Uyarı gizleme bizim biriktirme listesi üzerinde sahibiz.

## <a name="next-steps"></a>Sonraki adımlar
Bu tanılama araçları, uygulamanızdan alınan telemetri incelemenize yardımcı:

* [Ölçüm Gezgini](../../azure-monitor/app/metrics-explorer.md)
* [Arama Gezgini](../../azure-monitor/app/diagnostic-search.md)
* [Analytics - güçlü sorgu dili](../../azure-monitor/log-query/get-started-portal.md)

Akıllı algılama tamamen otomatik olarak yapılır. Ancak belki de daha fazla bazı uyarıları ayarlamak ister misiniz?

* [El ile yapılandırılan ölçüm uyarıları](../../azure-monitor/app/alerts.md)
* [Kullanılabilirlik web testleri](../../azure-monitor/app/monitor-web-app-availability.md)
