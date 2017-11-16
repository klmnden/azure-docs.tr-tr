---
title: "Azure zaman serisi Öngörüler nedir? | Microsoft Belgeleri"
description: "Giriş Azure zaman serisi Öngörüler, zaman serisi veri analizi ve IOT çözümleri için yeni bir hizmet."
services: time-series-insights
ms.service: time-series-insights
author: op-ravi
ms.author: omravi
manager: jhubbard
editor: MarkMcGeeAtAquent
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: article
ms.date: 11/15/2017
ms.openlocfilehash: 95cb26ada6f8ea39bc1a437a755f80ee7ddb7698
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="what-is-azure-time-series-insights"></a>Azure zaman serisi Öngörüler nedir?

Zaman serisi Öngörüler depolamak, Görselleştirme ve büyük miktarlarda IOT cihazları tarafından oluşturulan gibi zaman serisi veri sorgulama için yerleşik olarak bulunur.  Depolamak, yönetmek, sorgu veya Bulut zaman serisi verileri görselleştirmek istiyorsanız, zaman serisinin Öngörüler sağ sizin için olasıdır.  

Uygulamanın, dahili tüketim veya kullanmak, dış müşterileri için oluşturuluyorsa zaman serisi Öngörüler dizin oluşturma, depolama ve zaman serisi veri toplamak için bir arka ucu olarak kullanılabilir.  En üstte bir özel görsel ve kullanıcı deneyimi oluşturabilirsiniz.  Zaman serisi Öngörüler bu senaryoyu etkinleştirmek için REST sorgu API'lerini kullanıma sunar.  

Zaman serisi verilerinizi olduğu konusunda emin değilseniz, işte bilmeniz gerekir.  Zaman serisi veri bir varlık veya işlem zaman içinde nasıl değiştiğini temsil eder.  Bir zaman damgası vardır ve zaman eksen en anlamlı olan benzersizdir.  Zaman serisi veri genellikle zaman sırayla ulaştığında ve genellikle veritabanınız için bir güncelleştirme olarak değil, bir ekleme olarak kabul edilir.  Zaman serisi Öngörüler yakalar ve bir satır olarak her yeni bir olay depolar çünkü değişiklik geriye dönük arayın ve gelecekteki değişiklik tahmin etmek için etkinleştirme zamanla ölçülür.  Büyük birimleri depolanması, dizin oluşturma, sorgulama, çözümleme ve görselleştirme zaman serisi veri zor olabilir.  

## <a name="primary-scenarios"></a>Birincil senaryolar

- Zaman serisi veri ölçeklenebilir bir şekilde depolama.  
  - Özünde, zaman serisinin Öngörüler aklınızda zaman serisi verilerle tasarlanmış bir veritabanı vardır.  Ölçeklenebilir ve tam olarak yönetilen olduğundan, depolama ve olayları yönetme iş zaman serisi Öngörüler işler.

- Gerçek zamanlı veri keşfi.  
  - Zaman serisi Öngörüler tüm verileri bir ortama akış visualizes bir Gezgini sağlar.  Kısa süre içinde bir olay kaynağı bağladıktan sonra olay verileri, keşfedilen ve zaman serisi Öngörüler içinde sorgulanan görüntülenebilir.  Verileri, bir cihaz verileri beklenen şekilde yayma ve IOT varlık sistem durumu, verimliliği ve genel etkinliğini izleme olup olmadığını doğrulamak için kullanışlıdır.  

- Kök neden analizi ve anomali algılama.
  - Zaman serisi Öngörüler desenleri ve yürütün ve çok adımlı kök neden analizi kaydetmek için perspektif görünümler gibi araçlar vardır.  Ayrıca, Azure akış analizi gibi hizmetler uyarı ile birlikte zaman serisi Öngörüler çalışır, uyarılar ve algılanan anormallikleri görüntülenebilir için zaman serisi Öngörüler explorer'ın gerçek zamanlı yakınında.  

- Zaman serisi verilerinin çok asset/site karşılaştırma için farklı konumlardan akış genel görünümü.
  - Birden çok olay kaynakları bir zaman serisi Öngörüler ortama bağlanabilir.  Bu, birden çok, farklı konumlardan akış veri birlikte yakın gerçek zamanlı görüntülenebilir olduğunu anlamına gelir.  Kullanıcılar iş kılavuzları ile veri paylaşımı ve sorunları gidermenize yardımcı olması için uzmanlara uygulayabilirsiniz etki alanı uzmanlar ile daha iyi işbirliği etkinleştirmek için en iyi yöntemleri uygulayın ve learnings paylaşmak için bu görünürlüğü yararlanabilir.

- Zaman serisi Öngörüler en üstünde bir müşteri uygulaması oluşturma. 
  - Zaman serisi Öngörüler zaman serisi veri kullanan uygulamalar oluşturmanıza olanak etkinleştirme REST sorgu API'lerini kullanıma sunar.

## <a name="capabilities"></a>Özellikler

- **Hızlı bir şekilde kullanmaya başlama:** Azure zaman serisi Öngörüler hiçbir açık veri hazırlık gerekir. Azure IOT hub'ı veya olay hub'ı olayları milyonlarca dakika cinsinden bağlayın. Bağlantı kurulduktan sonra görselleştirin ve IOT çözümlerinizi hızlı bir şekilde doğrulamak için algılayıcı verileri ile etkileşim. Kod yazmadan verilerinizle etkileşim kurabilirsiniz.
Bilgi edinmek için yeni dil yoktur; Zaman serisi Öngörüler noktası İleri düzey kullanıcılar için bir parçalı, serbest metin sorgu yüzey sağlar ve keşif'i tıklatın.
- **Yakın gerçek zamanlı Öngörüler:** zaman serisi Öngörüler milyonlarca algılayıcı olayı, günde bir dakika gecikmeyle alma. Zaman serisi Öngörüler, algılayıcı verilerini Öngörüler yardımcı olarak eğilimleri kazanmanıza yardımcı olur ve anormallikleri, kök neden çözümlemeler yürütün ve maliyetli kapalı kalma sürelerinden kaçının. Gerçek zamanlı verilerle geçmiş verileri arasında çapraz bağıntıya olanak tanıdığından, Zaman Serisi Görüşleri verilerindeki gizli eğilimleri ortaya çıkarmanıza yardımcı olur.
- **Özel çözümler derleme:** mevcut uygulamalarınızı Azure zaman serisi Öngörüler katıştırmak verisine veya zaman serisi Öngörüler REST API'leri ile yeni özel çözümler oluşturabilir. Diğer öngörülerinizi keşfetmek paylaşabilirsiniz kişiselleştirilmiş görünümler oluşturun.
- **Ölçeklenebilirlik:** zaman serisi Öngörüler ölçekte IOT desteklemek üzere tasarlanmıştır. Varsayılan saklama ile günde 100 milyon için milyon olay 31 gün span 1 giriş dağıtabilirsiniz. Görselleştirme ve canlı verileri çözümlemek akış yakın zamanlı olarak geçmiş verileri geliştirir. İlerleyen, giriş ve saklama hızları Kurumsal ölçek uyum sağlayacak şekilde artacaktır.

## <a name="getting-started"></a>Başlarken
Başlarken 5 dakikadan daha kısa sürer. 

1.  Alınacak başlatıldı, Azure portalında bir zaman serisi Öngörüler ortamı sağlamak. 
2.  Bir Azure IOT hub'ı veya olay hub'ı gibi bir olay kaynağına bağlanın.  
3.  (Bu ek bir hizmet değil) başvuru verileri karşıya yükleyin.
4.  Zaman serisi Öngörüler Gezgini ile dakika cinsinden verilerinizi bakın.

## <a name="time-series-insights-explorer"></a>Zaman serisi Öngörüler Gezgini
Bu diyagramda zaman serisi örneği gösterilmektedir Öngörüler Veri Gezgini üzerinden görüntülenebilir:! [Zaman serisi Öngörüler Gezgini] (media/time-series-insights-explorer/explorer4.png)


## <a name="next-steps"></a>Sonraki adımlar
 - [Bir tanıtım ortamında zaman serisi Öngörüler explorer'ı kullanarak keşfedin](./time-series-quickstart.md)
 - [Zaman serisi Öngörüler ortamınızdaki planlama](time-series-insights-environment-planning.md)

