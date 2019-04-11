---
title: Azure Application Insights uyarıları ayarlamak | Microsoft Docs
description: Yavaş yanıt süreleri, özel durumlar ve diğer performans veya kullanımı ile ilgili değişiklikleri web uygulamanıza ilgili bildirim alın.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.reviewer: lagayhar
ms.assetid: f8ebde72-f819-4ba5-afa2-31dbd49509a5
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/23/2019
ms.author: mbullwin
ms.openlocfilehash: 8026576478b16b753ba960155c383ffec62c61ce
ms.sourcegitcommit: 6e32f493eb32f93f71d425497752e84763070fad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2019
ms.locfileid: "59469806"
---
# <a name="set-alerts-in-application-insights"></a>Uygulama anlayışları'nda uyarılar ayarlayın
[Azure Application Insights] [ start] web uygulamanızdaki performansı ya da kullanım ölçümleri değişikliklere uyarabilir. 

Application Insights hakkında Canlı uygulamanızı izler bir [çeşitli platformlarda] [ platforms] performans sorunlarını tanılama ve kullanım biçimlerini anlamanıza yardımcı olacak.

Birden çok uyarı türü vardır:

* [**Ölçüm uyarıları** ](../../azure-monitor/platform/alerts-metric-overview.md) ne zaman bir ölçüm - yanıt süreleri, özel durum sayısı, CPU kullanımı veya sayfa görüntüleme gibi belirli bir süre boyunca eşik değerini aştığında size söyler.
* [**Günlük uyarıları** ](../../azure-monitor/platform/alerts-unified-log.md) uyarılar açıklamak için uyarı sinyali özel Kusto sorgu burada temelli kullanılır.
* [**Web testleri** ] [ availability] sitenizi yavaş internet üzerindeki kullanılamıyor veya yanıt olduğunda söyleyin. [Daha fazla bilgi edinin][availability].
* [**Proaktif tanılama** ](../../azure-monitor/app/proactive-diagnostics.md) olağan dışı performans desenler hakkında bilgilendirmek için otomatik olarak yapılandırılır.

## <a name="set-a-metric-alert"></a>Ölçüm uyarısı ayarlama
Uyarı kuralları sekmesini açın ve ardından Ekle düğmesini kullanın.

![Uyarı kuralları sekmesinde, ekleme uyarısı seçin. Uygulamanızı, ölçün, uyarı için bir ad girin ve bir ölçüm seçin için kaynak olarak ayarlayın.](./media/alerts/01-set-metric.png)

* Kaynak önce diğer özelliklerini ayarlayın. **"(Bileşenler)" kaynağı seçin** performans ya da kullanım ölçümlerine göre uyarıları ayarlamak istiyorsanız.
* Uyarı veren adı (uygulamanız yalnızca) kaynak grubu içinde benzersiz olmalıdır.
* Eşik değerini girmeniz istenir birimleri Not dikkat edin.
* "E-posta sahipleri..." kutuyu işaretlerseniz, uyarılar, bu kaynak grubu erişimi olan herkes için e-posta ile gönderilir. Bu kişi kümesi genişletmek için bunları Ekle [kaynak grubuna veya aboneliğe](../../azure-monitor/app/resources-roles-access-control.md) (kaynak değil).
* "Ek e-postaları" belirtirseniz, uyarılar, bu kişiler veya gruplara (olsun veya olmasın, "e-posta sahipleri..." onay kutusunun işareti) gönderilir. 
* Ayarlanmış bir [Web kancası adresi](../../azure-monitor/platform/alerts-webhooks.md) uyarılara yanıt veren web uygulaması ayarladıysanız. Uyarı etkinleştirildiğinde hem çözüldüğünde çağrılır. (Ancak şu anda sorgu parametreleri Web kancası özellikleri geçirilecek değil olduğunu unutmayın.)
* Devre dışı bırakabilir veya uyarıyı etkinleştir: üstündeki düğmeleri bakın.

*Uyarı Ekle düğmesinin göremiyorum.*

* Bir kuruluş hesabı kullanıyorsunuz? Sahibi veya katkıda bulunan bu uygulama kaynağına erişimi varsa, uyarılar ayarlayabilirsiniz. Erişim denetimi sekmesini göz atın. [Erişim denetimi hakkında bilgi edinin][roles].

> [!NOTE]
> Uyarılar dikey penceresinde,, zaten var. bir uyarı kümesi bakın: [Proaktif tanılama](../../azure-monitor/app/proactive-failure-diagnostics.md). Otomatik uyarı bir belirli ölçüm, istek hata oranı izler. Proaktif uyarı devre dışı bırakmak karar vermediğiniz sürece, istek hata oranı üzerinde kendi uyarı ayarlama gerekmez.
> 
> 

## <a name="see-your-alerts"></a>Uyarılarınızı görmek
Etkin ve etkin arasındaki değişiklikleri uyarı durumu, e-posta alın. 

Her bir uyarının geçerli durumu uyarı kuralları sekmesinde gösterilir.

Aşağı açılan Uyarılardaki son etkinliğin bir özeti verilmiştir:

![Aşağı açılan uyarıları](./media/alerts/010-alert-drop.png)

Durum değişikliklerinin geçmişini etkinlik günlüğünde şöyledir:

![Genel Bakış sekmesinde ayarları, Denetim günlükleri](./media/alerts/09-alerts.png)

## <a name="how-alerts-work"></a>Uyarılar nasıl çalışır?
* Bir uyarı üç durumu vardır: "Etkinleştirmemiştir", "Etkinleştirildi" ve "Çözüldü." Son Değerlendirme zaman etkinleştirilmiş anlamına gelir, belirtilen koşulun true.
* Bir uyarı durumu değiştiğinde bildirim oluşturulur. (Bir uyarı oluşturulduğunda Uyarı koşulu zaten true, koşul false ölçeklendirilinceye kadar bir uyarı almayabilir.)
* E-posta kutusu işaretli ya da e-posta adreslerini sağlanan her bir bildirim e-posta oluşturur. Bildirimler açılır listenin de bakabilirsiniz.
* Bir uyarı, bir ölçüm ulaşan, ancak aksi her zaman değerlendirilir.
* Değerlendirme, önceki dönem boyunca ölçüm toplar ve ardından yeni durumu belirlemek için eşikle karşılaştırır.
* Aralık belirten seçtiğiniz dönem ölçümleri toplanır. Uyarı sıklıkta değerlendirileceğini etkilemez: varış ölçümleri sıklığı temel bağlıdır.
* Hiçbir veri süre için belirli bir ölçüm alınırsa, boşluk uyarı değerlendirmesi ve ölçüm Gezgini'nde grafiklerini farklı etkileri vardır. Uzun süre grafiğin örnekleme aralığı, veri görüldüğünde ölçüm Gezgini'nde grafik 0 değerini gösterir. Ancak aynı ölçüme göre bir uyarı değil yeniden değerlendirimiş, ve uyarının durumu değişmeden kalır. 
  
    Veri sonunda ulaştığında, grafik sıfır değerine geri atlar. Uyarı değerlendirir, belirtilen süre için kullanılabilir verileri temel alan. Yeni veri noktası süresinde tek çoğaltmaysa, toplama yalnızca veri noktası temel alır.
* Uzun süre ayarlasanız bile bir uyarı uyarı ve iyi durumda durumlar arasında sık Titreşim. Ölçüm değeri bir eşiği getirirse bu durum oluşabilir. Eşik yok hysteresis yoktur: geçiş sağlıklı olarak aynı değerde uyarı geçiş olur.

## <a name="what-are-good-alerts-to-set"></a>Ayarlanacak iyi uyarılar nedir?
Bu durum uygulamanıza bağlıdır. İle başlamak çok sayıda ölçüm ayarlanmadı en iyisidir. Uygulamanız çalışırken nasıl normal şekilde davranır için bir genel görünüm almak için ölçüm grafiklerine baktığımızda biraz zaman ayırın. Bu uygulama veritabanının performansını geliştirmeye yönelik yollar bulmanıza yardımcı olur. Ardından zaman ölçümlerini normal bölgenin dışında Git bildirmek için uyarılar ayarlayın. 

Popüler uyarılar şunlardır:

* [Tarayıcı ölçümleri][client], özellikle tarayıcı **sayfa yükleme sürelerinin**, web uygulamaları için uygundur. Birçok betik, sayfa varsa göz önünde bulundurmanız gerekenler **tarayıcı özel durumları**. Bu ölçümleri ve uyarıları almak için ayarlanmış olması [web sayfası izleme][client].
* **Sunucu yanıt süresi** sunucu tarafı web uygulamaları için. Uyarıları Ayarlama yanı sıra bu ölçüm, orantısız ile yüksek istek hızları değişiyorsa görmek için takip: değişim uygulamanızı kaynaklar yetersiz çalıştığını gösterebilir. 
* **Sunucu özel durumları** - bunları görmek için bazı yapmanız gereken [ek kurulum](../../azure-monitor/app/asp-net-exceptions.md).

Gerektiğini unutmayın [öngörülü hata oranı tanılama](../../azure-monitor/app/proactive-failure-diagnostics.md) otomatik olarak, uygulamanızın yanıt vereceğini hata kodlarıyla isteklerine oranı izleyin.

## <a name="how-to-set-an-exception-alert-using-custom-log-search"></a>Özel günlük arama özelliğini kullanarak bir özel durum uyarısı ayarlama

Bu bölümde, bir sorgu tabanlı özel durum uyarısı ayarlamak nasıl alacağız. Bu örnekte, son 24 saat içindeki başarısız oranı % 10'dan yüksek olduğunda bir uyarı istiyoruz. diyelim.

1. Azure portalında Application Insight kaynağınıza gidin.
2. Sol tarafta, altında tıklatın yapılandırın **uyarı**.

    ![Soldaki altında yapılandırma uyarıya tıklayın](./media/alerts/1appinsightalert.png)

3. Uyarı sekmenin üstünde seçin **yeni uyarı kuralı**.

     ![Uyarı sekmenin üst kısmında yeni uyarı kuralı'a tıklayın.](./media/alerts/2createalert.png)

4. Kaynağınızın otomatik seçili olmalıdır. Bir koşul ayarlamak için tıklayın **koşul Ekle**.

    ![Koşul Ekle seçeneğine tıklayın](./media/alerts/3addcondition.png)

5. Yapılandırma sinyal mantığını sekmesinde seçin **özel günlük araması**

    ![Özel günlük Ara](./media/alerts/4customlogsearch.png)

6. Özel günlük arama sekmede sorgunuzu "Arama sorgusu" kutusuna girin. Bu örnekte, kullanacağız Kusto sorgusu aşağıda.
    ```kusto
    let percentthreshold = 10;
    let period = 24h;
    requests
    | where timestamp >ago(period)
    | summarize requestsCount = sum(itemCount)
    | project requestsCount, exceptionsCount = toscalar(exceptions | where timestamp >ago(period) | summarize sum(itemCount))
    | extend exceptionsRate = toreal(exceptionsCount)/toreal(requestsCount) * 100
    | where exceptionsRate > percentthreshold

    ```

    ![Sorgu arama sorgu kutuya yazın.](./media/alerts/5searchquery.png)
    
    > [!NOTE]
    > Ayrıca, diğer sorgu tabanlı uyarı türleri için bu adımları uygulayabilirsiniz. Bu Kusto sorgu dili hakkında daha fazla bilgi [Kusto Başlarken doc](https://docs.microsoft.com/azure/kusto/concepts/) veya bu [SQL Kusto için kural sayfası](https://docs.microsoft.com/azure/kusto/query/sqlcheatsheet)

7. "Altında Alert logic", sonuçları veya ölçüm ölçüsü sayısına göre olup olmadığını seçin. Ardından koşulu (büyüktür, eşittir, daha fazla) ve bir eşik seçin. Bu değerleri değiştirirken, Koşul Önizleme cümle değişiklikler fark edebilirsiniz. Bu örnekte "equal" kullanıyoruz.

    ![Uyarı mantığı altında göre sağlanan seçenekleri ve koşul seçin, ardından bir eşik girin](./media/alerts/6alertlogic.png)

8. "Temel Evaluated" altında sıklığı ve süresi ayarlayın. Dönem burada biz için put değeriyle eşleşmelidir yukarıdaki sorguda dönem. Ardından **Bitti**.

    ![Dönem ve sıklığı altındaki ayarlayın ve sonra bitti'ye tıklayın](./media/alerts/7evaluate.png)

9. Artık ile tahmini aylık maliyet oluşturduğumuz koşul görüyoruz. Aşağıda altında ["Eylem grupları"](../platform/action-groups.md) yeni bir grup oluşturun veya varolan bir tanesini seçin. İsterseniz, eylemlerini özelleştirebilirsiniz.

    ![Seç'e tıklayın veya eylem grubu altında düğmeler oluşturma](./media/alerts/8actiongroup.png)

10. Son uyarı ayrıntılarınızı ekleyin (uyarı kuralı adı, açıklaması, önem derecesi). İşiniz bittiğinde tıklayın **uyarı kuralı oluştur** altındaki.

    ![Uyarı ayrıntısı altında uyarı kuralı adınızı yazın, bir açıklama yazın ve bir önem derecesi seçin](./media/alerts/9alertdetails.png)

## <a name="how-to-unsubscribe-from-classic-alert-e-mail-notifications"></a>Klasik uyarı e-posta bildirim aboneliği nasıl

Bu bölümde uygulandığı **Klasik kullanılabilirlik uyarıları**, **Application Insights ölçüm uyarıları**ve **Klasik hata anomalileri uyarılar**.

Aşağıdakilerden biri geçerliyse bu Klasik uyarılar için e-posta bildirimleri alıyorsunuz:

* E-posta adresinizi, bildirim e-posta alıcıları alanında uyarı kuralı ayarlar listelenir.

* Abonelik için belirli rolleri barındıran kullanıcılara e-posta bildirimleri gönderme seçeneği etkinleştirilir ve ilgili rol, belirli bir Azure aboneliği için basılı tutun.

![Uyarı bildirimi ekran görüntüsü](./media/alerts/alert-notification.png)

Güvenlik ve gizlilik genellikle öneririz, açıkça bildirim alıcılarını Klasik uyarılarınızı için belirttiğiniz daha iyi denetlemek için **bildirim e-posta alıcılarını** alan. Belirli roller bulunduran tüm kullanıcılara bildirin seçeneğini geriye dönük uyumluluk için sağlanır.

Bir belirli uyarı kuralı tarafından oluşturulan e-posta bildirimi aboneliğinizi iptal etmek, e-posta adresinizi kaldırmak **bildirim e-posta alıcılarını** alan.

Belirli roller tüm üyeleri otomatik olarak bilgilendirme seçeneğini devre dışı bırakın ve bunun yerine bu uyarı kuralı için bildirimler bildirim e-posta almak için gereken tüm kullanıcı e-postalar listesi e-posta adresinizi açıkça listelenmemişse öneririz Alıcılar alan.

## <a name="who-receives-the-classic-alert-notifications"></a>Kimin (Klasik) Uyarı bildirimlerini alır?

Bu bölümde, yalnızca klasik uyarılar için geçerlidir ve yalnızca istenen alıcılarınız bildirimlerini aldığından emin olmak için Uyarı bildirimlerini iyileştirmenize yardımcı olur. Arasındaki fark hakkında daha fazla anlamak için [Klasik uyarılar](../platform/alerts-classic.overview.md) ve yeni uyarılar deneyimini, başvurmak [uyarılar genel bakış makalesi](../platform/alerts-overview.md). Yeni uyarılar deneyimini uyarı bildiriminde denetlemek için kullanın [Eylem grupları](../platform/action-groups.md).

* Klasik bir uyarı bildirimlerini belirli alıcılara kullanılmasını öneririz.

* (Kullanılabilirlik ölçümlerini dahil), herhangi bir Application Insights ölçümler ile ilgili uyarılar için **toplu/grup** abonelik sahibi, katkıda bulunan veya okuyucu rollerine sahip kullanıcılar için onay kutusu seçeneği etkinleştirilirse, gönderir. Aslında, _tüm_ abonelik Application Insights kaynağına erişimi olan kullanıcılar kapsamındaki ve ilgili bildirimler alacaksınız.

> [!NOTE]
> Şu anda kullanıyorsanız **toplu/grup** onay kutusu seçeneğini ve devre dışı bırakmak, bu değişikliği geri almak mümkün olmayacaktır.

Yeni uyarı deneyimi/neredeyse gerçek zamanlı uyarılar, rollerine bağlı olarak kullanıcılara bildirmek gerekiyorsa kullanın. İle [Eylem grupları](../platform/action-groups.md), (tek bir seçenek olarak birlikte birleştirilmiş değil) sahip/katkıda bulunan/okuyucu rolüne sahip kullanıcılara e-posta bildirimleri yapılandırabilirsiniz.

## <a name="automation"></a>Otomasyon
* [Uyarıları Ayarlama otomatikleştirmek için PowerShell kullanma](../../azure-monitor/app/powershell-alerts.md)
* [Uyarılara yanıt verme otomatikleştirmek için Web kancalarını kullanma](../../azure-monitor/platform/alerts-webhooks.md)

## <a name="see-also"></a>Ayrıca bkz.
* [Kullanılabilirlik web testleri](../../azure-monitor/app/monitor-web-app-availability.md)
* [Uyarıları Ayarlama otomatikleştirin](../../azure-monitor/app/powershell-alerts.md)
* [Öngörülü tanılama](../../azure-monitor/app/proactive-diagnostics.md) 

<!--Link references-->

[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[client]: ../../azure-monitor/app/javascript.md
[platforms]: ../../azure-monitor/app/platforms.md
[roles]: ../../azure-monitor/app/resources-roles-access-control.md
[start]: ../../azure-monitor/app/app-insights-overview.md

