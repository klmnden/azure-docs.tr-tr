---
title: Azure zaman serisi Öngörüler ortamınızın ölçeğini planlama | Microsoft Docs
description: Bu makalede izleme ve depolama kapasitesi, veri saklama, giriş kapasitesi dahil olmak üzere bir Azure zaman serisi Öngörüler ortam planlarken en iyi uygulamaları izleyerek açıklar.
services: time-series-insights
ms.service: time-series-insights
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: article
ms.date: 11/15/2017
ms.openlocfilehash: 991db58db1bb07f338c0f80aa4db69ddb868dcab
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="plan-your-azure-time-series-insights-environment"></a>Azure zaman serisi Öngörüler ortamınızı planlama

Bu makalede, Azure zaman serisi Öngörüler ortamınızı beklenen giriş hızınızı ve veri bekletme gereksinimlerinize göre plan açıklar.

## <a name="best-practices"></a>En iyi uygulamalar

Verilerinizi depolamak gereken uzun nasıl dakikaya göre itme beklediğiniz ne kadar veri biliyorsanız zaman serisi Insights ile çalışmaya başlamak için en iyisidir.  

Her iki zaman serisi Öngörüler SKU'ları için kapasite ve saklama hakkında daha fazla bilgi için bkz: [zaman serisi Öngörüler fiyatlandırma](https://azure.microsoft.com/pricing/details/time-series-insights/).

En iyi planı aşağıdaki öznitelikleri ortamı uzun vadeli başarı için göz önünde bulundurun: 
- Depolama kapasitesi
- Veri saklama süresi
- Giriş kapasitesi 
- Olaylarınızı şekillendirme
- Yerinde başvuru verilere sahip olma

## <a name="understand-storage-capacity"></a>Depolama kapasitesi anlama
Varsayılan olarak, sağladığınız depolama alanı miktarına göre verileri (birimler kez depolama miktarını birim başına) ve Giriş zaman serisi Öngörüler korur.

## <a name="understand-data-retention"></a>Veri saklamayı anlama
Zaman serisi Öngörüler ortamı yapılandırabilirsiniz **veri saklama süresini** ayarı, 400 gün bekletme etkinleştirme.  Zaman serisi Öngörüler sahip iki mod, ortamınızın sağlamak için en iyi duruma getirir, varsa en güncel verilerin (üzerinde varsayılan olarak) ve diğeri de sınırları getirildiğinden, giriş varsa burada duraklatıldı bekletme sağlamak için en iyi duruma getirir genel depolama kapasitesi ortam ulaşılır.  Bekletme ve ortam yapılandırma sayfasında Azure Portalı'ndaki iki modlar arasında geçiş ayarlayabilirsiniz.

En fazla 400 gün veri bekletme zaman serisi Öngörüler ortamınızda yapılandırabilirsiniz.

## <a name="configure-data-retention"></a>Veri saklamayı yapılandırma

1. İçinde [Azure portal](https://portal.azure.com), zaman serisinin Öngörüler ortamınızı seçin.

2. Üzerinde **zaman serisi Öngörüler ortamı sayfa**altında **ayarları** başlığını seçin **yapılandırma**. 

3. İçinde **veri saklama süresi (gün)** kutusunda, 400 1 bir değer girin.

   ![Bekletme yapılandırma](media/environment-mitigate-latency/configure-retention.png)

## <a name="understand-ingress-capacity"></a>Giriş kapasite anlama

Planlama odaklanmak için diğer dakika başına ayırma bir türevi değil giriş kapasite alanıdır. 

Bir azaltma açısından ingressed veri paketi paket boyutu 32 KB'ın 32 olayları olarak kabul edilir, her 1 KB boyuta sahip. İzin verilen en fazla olay boyutu 32 KB'tır; veri paketlerinin 32 KB'den büyük kesilir.

Aşağıdaki tabloda her SKU giriş kapasiteyi özetlenmektedir:

|SKU  |Birim başına aylık olay sayısı  |Olay başına ay, birim boyutu  |Birim başına dakika başına olay sayısı  | Birim başına dakika başına boyutu   |
|---------|---------|---------|---------|---------|
|S1     |   30 milyon     |  30 GB     |  700    |  700 KB   |
|S2     |   300 milyon    |   300 GB   | 7,000   | 7000 KB  |

S1 veya S2 SKU kapasitesi tek bir ortamda 10 birimlerine artırabilir. Bir S2 S1 ortamın veya bir S1 S2 ortamın geçiremezsiniz. 

Giriş kapasitesi için öncelikle bir ay başına temelinde gerektiren toplam giriş belirlemeniz gerekir. Daha sonra bu burada azaltma ve gecikme süresi bir rol oynar olduğu gibi dakika başına ihtiyaçlarını nelerdir, belirleyin.

Son 24 saatten az, veri giriş bir depo varsa, zaman serisinin Öngörüler "nım" yukarıda listelenen oranları x 2 giriş oranını görüntüleyebilirsiniz. 

Örneğin, bir tek S1 SKU ve giriş verileri dakika ve depo başına 700 olayların bir hızda 1 saatten kısa bir süre bir hızda 1400 olayları veya daha az için varsa, olurdu ortamınıza belirgin hiçbir gecikme süresi. Ancak, bir saatten fazla dakika başına 1400 olayları aşarsa, gecikme görselleştirilmiş ve ortamınızdaki sorgu için kullanılabilir olan verileri için büyük olasılıkla deneyimi. 

Önceden ne kadar veri itme beklediğiniz bilemeyebilirsiniz. Bu durumda, veri telemetri için bulabilirsiniz [Azure IOT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-metrics) ve [Azure Event Hubs](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/05/25/using-the-azure-rest-apis-to-retrieve-event-hub-metrics/) , Azure portalında. Bu telemetri ortamınızı sağlama belirlemenize yardımcı olabilir. Kullanım **ölçümleri** kendi telemetri görüntülemek ilgili olay kaynağı için Azure portalında sayfası. Olay kaynağı ölçümlerinizi anlarsanız daha etkili bir şekilde planlamak ve zaman serisi Öngörüler ortamınızı sağlamak.

### <a name="calculate-ingress-requirements"></a>Giriş gereksinimlerini hesaplamak

- Giriş kapasitenizi, ortalama dakika başına oranıdır ve ortamınıza eşdeğer kapasitenizi 1 saatten az için x 2, beklenen giriş işlemek için büyük olduğunu onaylayın.

- Giriş ani gerçekleşirse, son 1 saat, daha uzun süre depo oranı, ortalama kullanın ve depo oranı işlemek için kapasiteye sahip bir ortam sağlamak.
 
### <a name="mitigate-throttling-and-latency"></a>Azaltma ve gecikme süresini azaltmak

Gecikme süresi azaltma ve engelleme hakkında daha fazla bilgi için bkz: [azaltmak gecikme süresi ve azaltma](time-series-insights-environment-mitigate-latency.md). 

## <a name="shaping-your-events"></a>Olaylarınızı şekillendirme
TSI için olayları göndermek yolu sağlama ortamı boyutunu desteklediğinden emin olmak önemlidir (buna karşılık, kaç tane olayları TSI okur ortama boyutunu ve her olay boyutu eşleyebilirsiniz).  Benzer şekilde, dilim ve verilerinizi sorgulanırken filtre ölçütü isteyebilirsiniz öznitelikler hakkında düşünmek önemlidir.  Bu aklınızda bölümünü şekillendirme JSON gözden geçirme önerdiğimiz bizim *Gönder olayları* belgeleri [belgeleri] (https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-send-events).  Bu sayfanın en doğru olur.  

## <a name="ensuring-you-have-reference-data-in-place"></a>Yerinde başvuru verilere sahip olma
Bir başvuru veri kümesi, olay kaynağı olayların büyütmek öğelerin koleksiyonudur. Zaman serisi Öngörüler giriş altyapısı başvuru Veri kümenizi her olay Olay kaynağınızdan karşılık gelen veri satırı ile birleştirir. Bu genişletilmiş olay daha sonra sorgu için kullanılabilir. Bu katılımı başvuru veri kümesinde tanımlanan birincil anahtar sütunları temel alır.

Başvuru verileri firmalarda geriye dönük katılmamış unutmayın. Bu, yalnızca geçerli ve gelecekteki giriş verileri eşleşen ve yapılandırılmış ve karşıya sonra başvuru tarih kümesine birleşik anlamına gelir.  TSI için çok fazla geçmiş veri göndermek ve verme karşıya yükleme veya başvuru verileri TSI içinde ilk olarak, oluşturmak planlıyorsanız zorunda kalabilirsiniz daha sonra yeniden çalışmanıza (ipucu, eğlenceli değil).  

Oluşturma, karşıya yükleme ve başvuru verilerinizi TSI yönetme hakkında daha fazla bilgi edinmek için head bizim *başvuru verileri* belgeleri [belgeleri] (https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-add-reference-data-set).


## <a name="next-steps"></a>Sonraki adımlar
- [Bir olay hub'ı olay kaynağı ekleme](time-series-insights-how-to-add-an-event-source-eventhub.md)
- [Bir IOT hub'ı olay kaynağı ekleme](time-series-insights-how-to-add-an-event-source-iothub.md)
