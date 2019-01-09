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
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: fe63813220d526536a31efb48b6ec29b6ae383e8
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2019
ms.locfileid: "54121150"
---
# <a name="set-alerts-in-application-insights"></a>Uygulama anlayışları'nda uyarılar ayarlayın
[Azure Application Insights] [ start] web uygulamanızdaki performansı ya da kullanım ölçümleri değişikliklere uyarabilir. 

Application Insights hakkında Canlı uygulamanızı izler bir [çeşitli platformlarda] [ platforms] performans sorunlarını tanılama ve kullanım biçimlerini anlamanıza yardımcı olacak.

Uyarılar üç tür vardır:

* **Ölçüm uyarıları** ne zaman bir ölçüm - yanıt süreleri, özel durum sayısı, CPU kullanımı veya sayfa görüntüleme gibi belirli bir süre boyunca eşik değerini aştığında size söyler. 
* [**Web testleri** ] [ availability] sitenizi yavaş internet üzerindeki kullanılamıyor veya yanıt olduğunda söyleyin. [Daha fazla bilgi edinin][availability].
* [**Proaktif tanılama** ](../../azure-monitor/app/proactive-diagnostics.md) olağan dışı performans desenler hakkında bilgilendirmek için otomatik olarak yapılandırılır.

Bu makalede ölçüm uyarıları odaklanıyoruz.

## <a name="set-a-metric-alert"></a>Ölçüm uyarısı ayarlama
Uyarı kuralları dikey penceresini açın ve ardından Ekle düğmesini kullanın. 

![Uyarı kuralları dikey penceresinde, uyarı Ekle'i seçin. Uygulamanızı, ölçün, uyarı için bir ad girin ve bir ölçüm seçin için kaynak olarak ayarlayın.](./media/alerts/01-set-metric.png)

* Kaynak önce diğer özelliklerini ayarlayın. **"(Bileşenler)" kaynağı seçin** performans ya da kullanım ölçümlerine göre uyarıları ayarlamak istiyorsanız.
* Uyarı veren adı (uygulamanız yalnızca) kaynak grubu içinde benzersiz olmalıdır.
* Eşik değerini girmeniz istenir birimleri Not dikkat edin.
* "E-posta sahipleri..." kutuyu işaretlerseniz, uyarılar, bu kaynak grubu erişimi olan herkes için e-posta ile gönderilir. Bu kişi kümesi genişletmek için bunları Ekle [kaynak grubuna veya aboneliğe](../../azure-monitor/app/resources-roles-access-control.md) (kaynak değil).
* "Ek e-postaları" belirtirseniz, uyarılar, bu kişiler veya gruplara (olsun veya olmasın, "e-posta sahipleri..." onay kutusunun işareti) gönderilir. 
* Ayarlanmış bir [Web kancası adresi](../../azure-monitor/platform/alerts-webhooks.md) uyarılara yanıt veren web uygulaması ayarladıysanız. Uyarı etkinleştirildiğinde hem çözüldüğünde çağrılır. (Ancak şu anda sorgu parametreleri Web kancası özellikleri geçirilecek değil olduğunu unutmayın.)
* Devre dışı bırakabilir veya uyarıyı etkinleştir: dikey penceresinin üstündeki düğmeleri bakın.

*Uyarı Ekle düğmesinin göremiyorum.* 

* Bir kuruluş hesabı kullanıyorsunuz? Sahibi veya katkıda bulunan bu uygulama kaynağına erişimi varsa, uyarılar ayarlayabilirsiniz. Erişim denetimi dikey penceresinde göz atın. [Erişim denetimi hakkında bilgi edinin][roles].

> [!NOTE]
> Uyarılar dikey penceresinde,, zaten var. bir uyarı kümesi bakın: [Proaktif tanılama](../../azure-monitor/app/proactive-failure-diagnostics.md). Otomatik uyarı bir belirli ölçüm, istek hata oranı izler. Proaktif uyarı devre dışı bırakmak karar vermediğiniz sürece, istek hata oranı üzerinde kendi uyarı ayarlama gerekmez. 
> 
> 

## <a name="see-your-alerts"></a>Uyarılarınızı görmek
Etkin ve etkin arasındaki değişiklikleri uyarı durumu, e-posta alın. 

Her bir uyarının geçerli durumu uyarı kuralları dikey pencerede gösterilir.

Aşağı açılan Uyarılardaki son etkinliğin bir özeti verilmiştir:

![Aşağı açılan uyarıları](./media/alerts/010-alert-drop.png)

Durum değişikliklerinin geçmişini etkinlik günlüğünde şöyledir:

![Genel Bakış dikey penceresinde, ayarları, Denetim günlükleri](./media/alerts/09-alerts.png)

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

## <a name="automation"></a>Otomasyon
* [Uyarıları Ayarlama otomatikleştirmek için PowerShell kullanma](../../azure-monitor/app/powershell-alerts.md)
* [Uyarılara yanıt verme otomatikleştirmek için Web kancalarını kullanma](../../azure-monitor/platform/alerts-webhooks.md)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="see-also"></a>Ayrıca bkz.
* [Kullanılabilirlik web testleri](../../azure-monitor/app/monitor-web-app-availability.md)
* [Uyarıları Ayarlama otomatikleştirin](../../azure-monitor/app/powershell-alerts.md)
* [Proaktif tanılama](../../azure-monitor/app/proactive-diagnostics.md) 

<!--Link references-->

[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[client]: ../../azure-monitor/app/javascript.md
[platforms]: ../../azure-monitor/app/platforms.md
[roles]: ../../azure-monitor/app/resources-roles-access-control.md
[start]: ../../azure-monitor/app/app-insights-overview.md

