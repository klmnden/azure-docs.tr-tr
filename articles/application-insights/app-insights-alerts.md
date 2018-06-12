---
title: Azure Application Insights'ta uyarıları ayarlama | Microsoft Docs
description: Yavaş yanıt süreleri, özel durumlar ve diğer performans veya web uygulamanızda kullanım değişiklikler hakkında bilgi edinin.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: f8ebde72-f819-4ba5-afa2-31dbd49509a5
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: 87be1a48a6c3320187243e549a8fb8e5ecc9e006
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35293612"
---
# <a name="set-alerts-in-application-insights"></a>Application Insights'ta uyarıları ayarlama
[Azure Application Insights] [ start] performans veya kullanım ölçümü web uygulamanızda değişikliklere uyarabilir. 

Application Insights üzerinde Canlı uygulamanızı izler bir [çeşitli platformlar] [ platforms] performans sorunlarını tanılama ve kullanım biçimlerini anlamanıza yardımcı olacak.

Uyarılar üç tür vardır:

* **Ölçüm uyarıları** zaman - yanıt süreleri, özel durum sayısı, CPU kullanımı ya da sayfa görünümleri gibi belirli bir süre için bir eşik değeri bir ölçüm kestiği söyleyin. 
* [**Web testleri** ] [ availability] sitenizi yavaş internet üzerindeki kullanılamıyor veya yanıt veren olduğunda söyleyin. [Daha fazla bilgi edinin][availability].
* [**Öngörülü tanılama** ](app-insights-proactive-diagnostics.md) olağan dışı performans desenleri hakkında bilgilendirmek için otomatik olarak yapılandırılır.

Biz bu makalede ölçüm uyarıları odaklanılmaktadır.

## <a name="set-a-metric-alert"></a>Ölçüm uyarı ayarlama
Uyarı kuralları dikey penceresini açın ve ardından Ekle düğmesini kullanın. 

![Uyarı kuralları dikey penceresinde eklemek uyarı seçin. Uygulamanızı ölçmek, uyarı için bir ad veya bir ölçü seçmek için kaynak olarak ayarlayın.](./media/app-insights-alerts/01-set-metric.png)

* Diğer özellikler önce kaynağı ayarlayın. **"(Bileşenler)" kaynağı seçin** performans ya da kullanım ölçümleri uyarıları ayarlamak istiyorsanız.
* Uyarı vermek adı (yalnızca, uygulama) kaynak grubunda benzersiz olmalıdır.
* Eşik değerini girmeniz istenir birimleri Not dikkat edin.
* "E-posta sahipleri..." kutusunu işaretlerseniz, uyarılar bu kaynak grubu erişimi olan herkes tarafından e-posta gönderilir. Bu kişiler kümesini genişletmek için bunları Ekle [kaynak grubuna veya aboneliğe](app-insights-resources-roles-access-control.md) (kaynağı değil).
* "Ek e-postaları" belirtirseniz, uyarılar, bu kişiler veya gruplar (olsun veya olmasın, "... sahipleri e-posta" onay kutusunun işareti) gönderilir. 
* Ayarlanmış bir [Web kancası adresi](../monitoring-and-diagnostics/insights-webhooks-alerts.md) uyarılara yanıt bir web uygulaması ayarladıysanız. Uyarı etkinleştirildiğinde, hem bu çözümlendiğinde denir. (Ancak şu anda sorgu parametreleri Web kancası özellikleri olarak geçirilecek değil olduğunu unutmayın.)
* Devre dışı bırakabilir veya uyarıyı etkinleştirin: düğmelerini dikey pencerenin üstündeki bakın.

*Uyarı Ekle düğmesi bakın yok.* 

* Bir kurumsal hesap kullanıyor musunuz? Sahibi veya katkıda bu uygulama kaynağa erişim varsa uyarılar ayarlayabilirsiniz. Erişim denetimi dikey göz atın. [Erişim denetimi hakkında bilgi edinin][roles].

> [!NOTE]
> Uyarıları dikey penceresinde, zaten var olduğu bir uyarı kümesi bkz: [öngörülü tanılama](app-insights-proactive-failure-diagnostics.md). Otomatik uyarı bir belirli ölçüm, isteği hata oranı izler. Öngörülü uyarıyı devre dışı bırakmaya karar sürece, isteği hata oranı üzerinde kendi uyarı ayarlamanız gerekmez. 
> 
> 

## <a name="see-your-alerts"></a>Uyarılarınızı bakın
Bir e-posta uyarı değişiklikleri durum olduğunda, etkin ve etkin arasında alın. 

Her uyarı geçerli durumu uyarı kuralları dikey penceresinde görüntülenir.

Aşağı açılan uyarılar son etkinliğin bir özeti verilmiştir:

![Aşağı açılan uyarıları](./media/app-insights-alerts/010-alert-drop.png)

Durum değişiklikleri geçmişini etkinlik günlüğünde şöyledir:

![Genel Bakış dikey penceresinde, ayarlar, Denetim günlükleri'ı tıklatın.](./media/app-insights-alerts/09-alerts.png)

## <a name="how-alerts-work"></a>Uyarılar nasıl çalışır?
* Üç durumlu bir uyarı vardır: "Hiçbir zaman etkinleştirilmiş", "Etkinleştirildi" ve "Çözümlendi." Son değerlendirildiğinde etkinleştirilmiş anlamına gelir, belirtilen koşulun true.
* Bir uyarı durumu değiştiğinde bildirim oluşturulur. (Uyarı oluşturduğunuzda Uyarı koşulu zaten true ise, koşulu yanlış gider kadar bir bildirim alamayabilirsiniz.)
* Her bir bildirim e-posta kutusunu işaretli veya e-posta adresleri sağlanan bir e-posta oluşturur. Bildirimleri aşağı açılan listesinde de bakabilirsiniz.
* Bir uyarı, bir ölçüm geldiğinde, ancak aksi, her defasında değerlendirilir.
* Değerlendirme ölçüm önceki süresi içinde toplar ve yeni durumu belirlemek üzere eşik karşılaştırır.
* Aralığı belirten seçtiğiniz dönem ölçümleri toplanır. Bu uyarı ne sıklıkta değerlendirileceğini etkilemez: varış ölçümleri sıklığını bağlıdır.
* Hiçbir veri bir süre için belirli bir ölçüm gelirse boşluğu uyarı değerlendirmesi ve ölçüm Gezgini grafiklerde farklı etkilere sahiptir. Hiçbir veri grafiğin örnekleme aralığı daha uzun süre görülür, ölçüm Explorer'da grafik 0 değerini gösterir. Ancak aynı Ölçüm üzerinde dayalı bir uyarı değil yeniden değerlendirimiş, ve uyarının durumu değişmeden kalır. 
  
    Veri sonunda ulaştığında grafik sıfır değerine geri atlar. Uyarı değerlendirir belirtilen dönem için kullanılabilir verileri temel alan. Yeni veri noktası tek bir dönem içinde kullanılabilir ise, toplama yalnızca üzerinde veri noktası temel alır.
* Uzun süre ayarlasanız bile bir uyarı uyarı ve sağlam durumları arasında sık Titreşim. Ölçüm değeri eşik gezinen bu durum meydana gelebilir. Eşik yok hysteresis yoktur: sağlıklı geçişi aynı değerde uyarı geçiş olur.

## <a name="what-are-good-alerts-to-set"></a>Ayarlamak için iyi uyarılar nelerdir?
Uygulamanızda bağlıdır. İle başlamak çok fazla ölçümleri ayarlanmadı en iyisidir. Uygulamanızı çalışırken nasıl normal şekilde davranır için bir fikir almak için ölçüm grafiklerinizi arayan biraz zaman ayırın. Bu yöntem, kendi performansını artırmak için yollar bulmanıza yardımcı olur. Ardından zaman ölçümleri normal bölgesi dışından Git bildirmek için uyarıları ayarlayın. 

Popüler uyarıları şunları içerir:

* [Tarayıcı ölçümleri][client], özellikle tarayıcı **sayfa yükleme süreleri**, web uygulamaları için iyidir. Sayfanız birçok komut dosyaları varsa, göz önünde bulundurmanız gerekenler **tarayıcı özel durumları**. Bu ölçümleri ve Uyarıları alabilmek için ayarlanmış olan [web sayfası izleme][client].
* **Sunucu yanıt süresi** sunucu tarafı web uygulamaları için. Uyarılarını ayarlama yanı sıra bu orantısız ile yüksek istek hızları değişiyorsa görmek için bu ölçüm takip: değişim uygulamanızı kaynakları tükendi çalıştığını gösterebilir. 
* **Sunucu özel durumları** - bunları görmek için bazı yapmak zorunda [ek kurulum](app-insights-asp-net-exceptions.md).

Unutmayın [öngörülü hata oranı tanılama](app-insights-proactive-failure-diagnostics.md) otomatik olarak hata kodlarıyla isteklerine uygulamanızı yanıt hızını izleme. 

## <a name="automation"></a>Otomasyon
* [Uyarıları Ayarlama otomatikleştirmek için PowerShell kullanma](app-insights-powershell-alerts.md)
* [Web kancası uyarılara yanıt verme otomatikleştirmek için kullanın](../monitoring-and-diagnostics/insights-webhooks-alerts.md)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="see-also"></a>Ayrıca bkz.
* [Kullanılabilirlik web testleri](app-insights-monitor-web-app-availability.md)
* [Uyarıları Ayarlama otomatikleştirme](app-insights-powershell-alerts.md)
* [Öngörülü tanılama](app-insights-proactive-diagnostics.md) 

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[platforms]: app-insights-platforms.md
[roles]: app-insights-resources-roles-access-control.md
[start]: app-insights-overview.md

