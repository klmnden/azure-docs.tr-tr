---
title: Azure Time Series Insights ortamınızda veri saklamayı anlama | Microsoft Docs
description: Bu makalede, Azure zaman serisi görüşleri ortamınıza veri tutma denetleyen iki ayarları açıklanır.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 02/09/2018
ms.custom: seodec18
ms.openlocfilehash: b230ac48cf2ca14c9ed988f869b5abba3e347215
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63761564"
---
# <a name="understand-data-retention-in-time-series-insights"></a>Zaman serisi görüşleri'nde veri saklamayı anlama

Bu makalede, zaman serisi öngörüleri (TSI) ortamınızda veri bekletme etkileyen iki ayarları açıklanır.

## <a name="video"></a>Video

### <a name="in-this-video-we-cover-time-series-insights-data-retention-and-how-to-plan-for-itbr"></a>Bu videoda, Time Series Insights veri saklama ve bunun için nasıl kapsar.</br>

> [!VIDEO https://www.youtube.com/embed/03x6zKDQ6DU]

Her TSI ortamın denetleyen bir ayarı vardır **veri saklama zamanı**. Değer 1 ile 400 gün yayılır. Veriler ortamı depolama kapasitesi veya bekletme süresine (1-400) göre silinir, hangisinin önce geldiğine.

Her TSI ortam ek ayarının **depolama sınırı aştı davranışı**. Bu ayar, bir ortamın maksimum kapasite üst sınırına ulaşıldığında giriş ve temizleme davranışını denetler. Aralarından seçim yapabileceğiniz iki davranışları vardır:
- **Eski veri temizleme** (varsayılan)  
- **Duraklatma giriş**

> [!NOTE]
> Yeni ortam oluşturulurken varsayılan olarak, bekletme için yapılandırılmış **eski veri temizleme**. Bu ayar, Azure portalını kullanarak oluşturma zamanı sonra gerektiği şekilde açılıp kapatılabilir **yapılandırma** TSI ortamın sayfası.

Bekletme davranışları geçiş hakkında daha fazla bilgi için gözden [bekletme zaman serisi Öngörülerinde yapılandırma](time-series-insights-how-to-configure-retention.md).

Veri saklama davranışını karşılaştırın:

## <a name="purge-old-data"></a>Eski verileri temizleme

- Bu davranış TSI ortamları ve aynı davranışı TSI ortamları olduğundan, genel Önizleme için başlatılan sergilenen sergiler varsayılan davranışıdır.  
- Kullanıcıların her zaman görmek istediğinizde bu davranışı tercih edilir, *en son verileri* TSI ortamlarında. 
- Bu davranış *temizler* veri ortamı (elde tutma süresi, boyut veya count, hangisi önce gerçekleşirse) sınırlara ulaştı. Bekletme, varsayılan olarak 30 gün olarak ayarlanır. 
- En eski alınan verilerin ilk (FIFO yaklaşım) temizlenir.

### <a name="example-one"></a>Bir örnek

Bekletme davranışı bulunduğu ortamı göz önünde bulundurun **giriş devam etmek ve eski verilerini Temizle**: Bu örnekte, **veri saklama zamanı** 400 gün olarak ayarlanır. **Kapasite** 30 GB toplam kapasite içeren S1 birimi için ayarlanır.   Ortalama olarak her gün gelen veri 500 MB olarak birikir varsayalım. Bu ortama, yalnızca 60 gün gelen veri hızı maksimum kapasite 60 gün boyunca ulaşıldıktan sonra verilen verileri koruyabilirsiniz. Gelen veri olarak toplanır: 500 MB her gün x 60 gün = 30 GB.

Bu örnekte, 61st gün ortamı çevrimiçiyken gösterir, ancak 60 günden eski eski verileri temizler. Temizleme, akış yeni verilere yer yapar ve böylece yeni veri incelenecek devam edebilir. 

Kullanıcı verileri daha uzun süre tutmak isteyen, ek birimleri ekleyerek ortamı boyutunu artırabilir veya daha az veri gönderebilirsiniz.  

### <a name="example-two"></a>İki örnek

Bir ortam da yapılandırılmış bekletme davranışı göz önünde bulundurun **giriş devam etmek ve eski veri temizleme**. Bu örnekte, **veri saklama zamanı** 180 gün daha düşük bir değere ayarlanır. **Kapasite** 30 GB toplam kapasite içeren S1 birimi için ayarlanır. Tam 180 gün boyunca verileri saklamak için günlük giriş günde 0.166 GB (166 MB) aşamaz.  

Bu ortamın günlük giriş oranı günde 0.166 GB aştığında, bazı veriler temizlendi olduğundan veri 180 gün boyunca depolanamaz. Bu ortamda meşgul bir zaman çerçevesinde göz önünde bulundurun. Gün başına ortalama bir 0.189 GB ortamın giriş oranı artırabilir varsayılır. O meşgul zaman çerçevesi içinde yaklaşık 158 günden verileri korunur (30GB/0.189 = 158,73 gün saklama). Bu süre istenen veri bekletme zaman çerçevesi'dan küçük.

## <a name="pause-ingress"></a>Duraklatma girişi

- Bu davranış, boyutu ve sayısı sınırları, tutma süresi öncesinde ulaştıysanız veri temizlenmez emin olmak için tasarlanmıştır.  
- Bu davranış kullanıcılar veri bekletme süresi ihlal nedeniyle temizlenir. önce ortamlarında kapasitesini artırmak için ek süre sağlar.
- Bu davranış veri kaybına karşı korunmasına yardımcı olur, ancak tutma süresi, olay kaynağı olan giriş duraklatılmışsa, en son veri kaybı için bir fırsat oluşturur.
- Ancak, bir ortamın kapasite üst sınırı aşıldığında, ortam veri girişi duraklatır kadar ek eylemler gerçekleşir: 
   - Ortamın kapasite üst sınırı artırırsınız. Daha fazla bilgi için [Time Series Insights ortamınızı ölçeklendirme](time-series-insights-how-to-scale-your-environment.md) daha fazla ölçek birimi eklemek için.
   - Veri saklama süresi ulaştı ve bu nedenle, kapasite üst sınırı aşağıdaki ortam getirme veri temizlenir.

### <a name="example-three"></a>Örnek üç

Bir ortam için yapılandırılmış bekletme davranışı göz önünde bulundurun **giriş duraklatma**. Bu örnekte, **veri saklama süresi** 60 gün için yapılandırılmıştır. **Kapasite** 3 S1 birimi için ayarlanır. Bu ortamda, her gün 2 GB'lık veri girişi sahip olduğunu varsayın. Bu ortamda, kapasite üst sınırı aşıldığında giriş duraklatıldı. O anda giriş sürdürür kadar veya 'giriş devam' (yeni veriler için yer açmak için eski verileri temizlemek) etkin olduğu kadar aynı veri ortam gösterilmektedir. 

Ne zaman giriş devam eder:

- Olay kaynağına göre alındığı sırayla bir veri akışı
- Olay kaynağınızı bekletme ilkeleri aşmadığınız sürece olaylara bağlı olarak, zaman dizine eklenir. Olay kaynağı saklama yapılandırması, daha fazla bilgi için [Event Hubs SSS](../event-hubs/event-hubs-faq.md)

> [!IMPORTANT]
> Giriş duraklatıldıktan önlemeye yardımcı olmak için bildirim sağlamak için uyarılar ayarlamanız gerekir. Varsayılan saklama süresi 1 gün için Azure olay kaynakları olduğundan veri kaybı mümkündür. Giriş duraklatıldı sonra başka bir işlem açıklanmamasına sürece bu nedenle, büyük olasılıkla en son veri kaybı. Kapasitesini artırmanız veya geçiş için davranış **eski veri temizleme** olası veri kaybını önlemek için.

Etkilenen Event Hubs, ayarlamayı göz önünde bulundurun **ileti bekletme** duraklatma Giriş zaman serisi öngörüleri ortaya çıktığında, veri kaybını en aza indirmek için özellik.

![Olay hub'ı ileti bekletme.](media/time-series-insights-contepts-retention/event-hub-retention.png)

Olay kaynağı (timeStampPropertyName) üzerinde hiçbir özellik yapılandırıldıysa, TSI x ekseni olarak sıem'e olay hub'ı, zaman damgası varsayılan kullanır. TimeStampPropertyName farklı bir şey olacak şekilde yapılandırılmışsa, olaylar ayrıştırılır, ortam veri paketindeki yapılandırılmış timeStampPropertyName arar. 

Ek kapasite uyum veya bekletme, uzunluğunu artırmak için ortamınızın ölçeğini gerekiyorsa [Time Series Insights ortamınızı ölçeklendirme](time-series-insights-how-to-scale-your-environment.md) daha fazla bilgi için.  

## <a name="next-steps"></a>Sonraki adımlar

- Değiştirme bekletme davranışı hakkında daha fazla bilgi için gözden [bekletme zaman serisi Öngörülerinde yapılandırma](time-series-insights-how-to-configure-retention.md).
