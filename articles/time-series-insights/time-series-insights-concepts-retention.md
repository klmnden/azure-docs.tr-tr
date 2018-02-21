---
title: "Azure zaman serisi Öngörüler ortamınızda veri bekletme anlama | Microsoft Docs"
description: "Bu makalede, Azure zaman serisi Öngörüler ortamınızda veri bekletme denetleyen iki ayarları açıklanır."
services: time-series-insights
ms.service: time-series-insights
author: anshan
ms.author: anshan
manager: kfile
editor: MicrosoftDocs/tsidocs
ms.reviewer: jasonh, kfile, anshan
ms.workload: big-data
ms.topic: article
ms.date: 02/09/2018
ms.openlocfilehash: 46e0c4fa25c7d8a56763b80bf7de97c775c7ee99
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2018
---
# <a name="understand-data-retention-in-time-series-insights"></a>Zaman serisi Öngörüler cinsinden veri tutma anlama
Bu makalede veri bekletme zaman serisi Öngörüler (TSI) ortamınızdaki etkileyen iki ayarları açıklanır.

Her TSI ortam denetimleri ayarının **veri saklama süresini**. Değer 1 ile 400 gün yayar. Veriler ortam depolama kapasitesi veya bekletme süreye göre (1-400) silinir, hangisi önce gelirse.

Her TSI ortamı ek ayarının **depolama sınırını aştığından davranış**. Maksimum kapasite bir ortamın ulaşıldığında bu ayarı; giriş ve temizleme davranışını denetler. Aralarından seçim yapabileceğiniz iki davranışları vardır:
- **Eski veri temizleme** (varsayılan)  
- **Duraklatma giriş**

> [!NOTE]
> Yeni bir ortam oluştururken, varsayılan olarak, bekletme için yapılandırılmış **temizleme eski verileri**. Bu ayar değiştirilebilir, Azure portal kullanarak oluşturulma zamanı sonra gerektiği gibi **yapılandırma** TSI ortamının sayfası.

Bekletme davranışları geçiş hakkında daha fazla bilgi için gözden [zaman serisi öngörü saklamayı yapılandırmaya](time-series-insights-how-to-configure-retention.md).

Veri saklama davranışı karşılaştırın:

## <a name="purge-old-data"></a>Eski verileri temizleme
- Bu davranış, TSI ortamları ve aynı davranışı TSI ortamlara sergilenen genel önizleme içinde başlatılan beri sergiler için varsayılan davranıştır.  
- Kullanıcıların her zaman görmek istediğinizde bu davranış tercih edilir kendi *en son verileri* TSI ortamlarında. 
- Bu davranış *temizler* verilerinin ortamı (bekletme süresini, boyut veya count hangisi önce gelirse) sınırları ulaştı. Bekletme varsayılan olarak 30 gün olarak ayarlanır. 
- Eski alınan verilerin ilk (FIFO yaklaşım) temizlenir.

### <a name="example-1"></a>Örnek 1:
Bekletme davranışı örnek ortamıyla göz önünde bulundurun **giriş devam etmek ve eski verileri temizleme**: Bu örnekte, **veri saklama süresini** 400 gün olarak ayarlanır. **Kapasite** 30 GB toplam kapasite içeren S1 birimine ayarlanır.   Her gün için 500 MB gelen verileri ortalama öğelerden varsayalım. Bu ortam yalnızca 60 gün maksimum kapasite 60 günde ulaşıldıktan sonra gelen veri oranını verilen verileri koruyabilirsiniz. Olarak gelen veriler toplanır: 500 MB her gün x 60 gün = 30 GB. 

Bu örnekte, 61st gün ortamı en yeni verileri gösterir, ancak 60 gün sayısından önceki eski verileri temizler. Temizleme, akış yeni veriler için yer yapar ve böylece yeni verileri incelenecek devam edebilir. 

Kullanıcı verilerini korumak isterse ek birimlerini ekleyerek ortamı boyutunu artırabilir veya daha az veri gönderebilir.  

### <a name="example-2"></a>Örnek 2:
Bir ortamda yapılandırmak da bekletme davranışı göz önünde bulundurun **giriş devam etmek ve eski verileri temizlemek**. Bu örnekte, **veri saklama süresini** 180 günlük daha düşük bir değere ayarlanır. **Kapasite** 30 GB toplam kapasite içeren S1 birimine ayarlanır. Tam 180 gün için verileri depolamak için günlük giriş günde 0.166 GB (166 MB) aşamaz.  

Bu ortam günlük giriş oranı günde 0.166 GB aştığında, bazı veriler temizlenmeden bu yana veri 180 gün için depolanamaz. Bu aynı ortamda meşgul bir zaman çerçevesinde göz önünde bulundurun. Gün başına ortalama bir 0.189 GB ortam giriş hızı artırabilir varsayalım. O meşgul zaman dilimi içinde 158 günlük verileri korunur (30GB/0.189 = bekletme 158.73 gün). Bu süre istenen veri bekletme zaman çerçevesi'dan küçük.

## <a name="pause-ingress"></a>Duraklatma giriş
- Bu davranış, kendi saklama dönemi önce boyutu ve sayısı sınırları ulaştıysanız veriler temizlenmez emin olmak için tasarlanmıştır.  
- Bu davranış kullanıcıların veri tutma süresine ihlal nedeniyle temizlenmeden önce ortamlarına kapasitesini artırmak ek süre sağlar
- Bu davranış, veri kaybına karşı korunmasına yardımcı olur, ancak giriş olay kaynağı Bekletme süresinin dışına duraklatıldığında, en son veri kaybı için bir fırsat oluşturur.
- Ancak, bir ortam maksimum kapasite ulaşıldığında, ortam veri giriş ek eylemler gerçekleşir kadar duraklar: 
   - Ortam maksimum kapasitesini artırın. Daha fazla bilgi için bkz: [zaman serisi Öngörüler ortamınızı ölçeklendirme](time-series-insights-how-to-scale-your-environment.md) daha fazla ölçek birimi eklemek için.
   - Veri saklama süresi ulaştı ve bu nedenle ortamı kapasitesinin üst sınırına aşağıdan getiren veri temizlenir.

### <a name="example-3"></a>Örnek 3:
Bir ortam için yapılandırılmış tutma davranışı göz önünde bulundurun **giriş duraklatmak**. Bu örnekte, **veri saklama süresi** 60 gün için yapılandırılmıştır. **Kapasite** S1 3 birimleri için ayarlanır. Bu ortamda, her gün 2 GB veri olduğunu varsayalım. Maksimum kapasite ulaşıldığında bu ortamda, giriş duraklatıldı. O anda giriş sürdürür kadar veya 'giriş devam' (yeni veriler için yer açmak için eski verileri temizlemek) etkin olduğu kadar aynı veri ortamı gösterilmektedir. 

Ne zaman giriş sürdürür:
- Olay kaynağı tarafından alındığı sırada veri akışları
- Bekletme ilkeleri, olay kaynağı aşmadığınız sürece olayları kendi zaman damgası göre dizinlenir. Olay kaynağı Bekletme yapılandırması hakkında daha fazla bilgi için [olay hub'ları SSS](../event-hubs/event-hubs-faq.md)

> [!IMPORTANT]
> Giriş duraklatıldıktan önlemeye yardımcı olmak için bildirim sağlamak için uyarıları ayarlamanız gerekir. Varsayılan saklama için Azure olay kaynakları 1 gün olduğundan veri kaybı mümkündür. Giriş duraklatıldı sonra ek eylem sürece bu nedenle, büyük olasılıkla en son veri kaybı. Kapasiteyi artırmak veya geçiş için davranış **temizleme eski verileri** olası veri kaybını önlemek için.

Etkilenen Event Hubs ayarlamayı göz önünde bulundurun **ileti bekletme** Duraklat Giriş zaman serisi öngörü oluştuğunda veri kaybını en aza indirmek için özellik.

![Olay hub'ı ileti bekletme.](media/time-series-insights-contepts-retention/event-hub-retention.png)

Özellik olay kaynağı (timeStampPropertyName) yapılandırdıysanız TSI x ekseni olarak olay hub'ı adresindeki varış zaman damgası varsayılan olarak ayarlanır. TimeStampPropertyName farklı bir şey olması için yapılandırılmışsa, olaylar ayrıştırılır, ortam veri paketindeki yapılandırılmış timeStampPropertyName arar. 

Ek kapasite uyum veya bekletme, uzunluğunu artırmak için bkz. ortamınızı ölçeği gerekiyorsa [zaman serisi Öngörüler ortamınızı ölçeklendirme](time-series-insights-how-to-scale-your-environment.md) daha fazla bilgi için.  

## <a name="next-steps"></a>Sonraki adımlar
Değiştirme bekletme davranışı hakkında daha fazla bilgi için gözden [zaman serisi öngörü saklamayı yapılandırmaya](time-series-insights-how-to-configure-retention.md).
