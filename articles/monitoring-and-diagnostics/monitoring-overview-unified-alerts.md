---
title: Azure İzleyicisi'nde yeni uyarılar deneyimi keşfetme | Microsoft Docs
description: Yeni Basit ve ölçeklenebilir uyarıları yazma, görüntüleme ve daha kolay Uyarıları yönetme Azure yapar nasıl deneyimini anlama
author: manishsm-msft
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2018
ms.author: mamit
ms.custom: ''
ms.openlocfilehash: c3622b4699ef532f204231c76aa3436be3676763
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="the-new-alerts-experience-in-azure-monitor"></a>Azure İzleyicisi'nde yeni uyarılar deneyimi

## <a name="overview"></a>Genel Bakış

> [!NOTE]
> Bu makalede daha yeni uyarılar açıklanmaktadır. Eski Klasik Azure izleme uyarıları bölümünde açıklanmıştır [Klasik uyarılar genel bakış](monitoring-overview-alerts.md). 
>
>

Yeni deneyimi uyarılar var. Deneyimidir şimdi uyarılar (Klasik) sekmesi altında eski uyarılar. Yeni uyarılar deneyimi uyarıları (Klasik) deneyimi aşağıdaki faydaları vardır:

-   **Daha iyi bildirim sistemi**: tüm yeni Uyarıları kullanmak [Eylem grupları]( https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups), bildirimler ve birden çok uyarıları yeniden kullanılabilir eylemler grupları denir.  Klasik ölçüm uyarıları ve eski günlük analizi uyarıları Eylem grupları kullanmayın. 
- **Bir birleşik yazma deneyimi** - tüm oluşturma için ölçümleri, uyarı günlükleri ve etkinlik oturum Azure monitöre günlük analizi ve Application Insights içinde bir yerdir. 
- **Görünüm harekete Azure Portal'da günlük analizi uyarılarını** -Ayrıca bkz: günlük analizi uyarılar aboneliğinizde harekete artık şunları yapabilirsiniz. Daha önce bu ayrı bir portal yoktu. 
- **Fired uyarıları ve uyarı kuralları ayrılması** - uyarı kuralları (bir uyarıyı tetikleyen koşulu tanımı) ve tetiklenen uyarıları (uyarı kuralı tetikleme örneğini) Ayrıştırılan, işletimsel ve yapılandırma görünümleri ayrılmış şekilde.
- **Daha iyi iş akışı** - yeni uyarılar hakkında uyarı için doğru öğeleri bulmak daha basit hale getirir bir uyarı kuralı yapılandırma işlemi boyunca kullanıcı deneyimi kılavuzları yazma.
 
Yeni ölçüm uyarıları özel olarak aşağıdaki geliştirmeleri vardır:
-   **Geliştirilmiş gecikme**: yeni ölçüm uyarıları dakikada bir kadar sık çalıştırılabilir. Eski ölçüm uyarıları olan 5 dakikada bir sıklığında her zaman çalışır. Günlük uyarı çözümlenmedi bir uzun 1'den gecikme süresi nedeniyle günlüklerini alma kadar süreceğine dakikadır. 
-   **Çok boyutlu ölçümleri desteği**: ölçüm ilginç bir parçasını izlemenizi sağlayan boyutlu ölçülerine uyarabilir.
-   **Ölçüm koşullar hakkında daha fazla denetime**: daha zengin uyarı kurallarını tanımlayabilirsiniz. Yeni uyarılar ölçümleri maksimum, minimum, ortalama ve toplam değerler izlemeyi destekler.
-   **Birden çok ölçümlerini izleme birleştirilmiş**: tek bir kural ile birden çok ölçümleri (şu anda en fazla iki ölçümleri) izleyebilirsiniz. Her iki ölçümleri, belirtilen zaman aralığı için kendi ilgili eşiklerini ihlal varsa bir uyarı tetiklenir.
-   **Ölçümleri günlüklerinden** (sınırlı genel Önizleme): günlük analizi giderek veri için şimdi bazı günlük ayıklanır ve Azure İzleyici ölçümleri dönüştürülür ve ardından yalnızca diğer ölçümleri gibi uyarı. 



Aşağıdaki bölümlerde yeni deneyimi nasıl çalıştığını daha ayrıntılı olarak açıklanmaktadır.

## <a name="alert-rules-terminology"></a>Uyarı kuralları terminolojisi
Yeni uyarılar deneyimi aşağıdaki kavramlar geliştirme deneyimi farklı uyarı türleri arasında birleştirin sırasında uyarı kuralı ve puanlı uyarı nesneleri ayırmak için kullanır.

- **Hedef kaynak** -bir hedef herhangi bir Azure kaynağı olabilir. Hedef kaynak uyarı için kullanılabilir sinyalleri ve kapsamını tanımlar. Örnek hedefler: bir sanal makine, bir depolama hesabı, bir sanal makine ölçek kümesi, bir günlük analizi çalışma alanı veya Application Insights kaynağı.

- **Ölçüt** - ölçütü sinyal birleşimi ve mantığı hedef kaynak üzerindeki uygulanır. Örnekler: Yüzdesi % CPU > 70, sunucu yanıt süresi > 4 ms, sonuç sayısı günlük sorgu > 100 vs. 

- **Sinyal** - hedef kaynak tarafından gösterilen işaret eder ve çeşitli türlerde olabilir. **Ölçüm**, **etkinlik günlüğü**, **Application Insights**, ve **günlük** desteklenen sinyal türleridir.

- **Mantığı** -sinyal içinde olup olmadığını denetlemek için kullanıcı tanımlı mantık beklenen aralık/değerleri.  
 
- **Eylem** - belirli bir eylemi uyarı başlatıldığında gerçekleştirilecek. Örneğin, bir e-posta adresine e-postayla gönderme veya bir Web kancası URL'si çağırma. Bir uyarı oluşturulduğunda birden çok eylem ortaya çıkabilir. Bu uyarılar eylem gruplarını destekler.  
 
- **Uyarı kuralı** -uyarı tetikleyecek koşulu. Uyarı kuralı uyarmak için ölçüt ve hedef yakalar. Uyarı kuralı, bir etkin veya devre dışı durumda olabilir.
 
    > [!NOTE]
    > Bu, burada uyarı kural ve Mazotlu uyarı temsil eder ve bu nedenle uyarı, etkin veya devre dışı durumlardan birinde olabilir uyarıları (Klasik) deneyiminde farklıdır.
    >

## <a name="single-place-to-view-and-manage-alerts"></a>Uyarıları görüntülemek ve yönetmek için tek yer
Uyarıları deneyimi tüm Azure uyarıları görüntülemek ve yönetmek için tek yer hedefidir. Aşağıdaki alt bölümleri tek tek her yeni deneyimi ekranın işlevleri açıklanmaktadır.

### <a name="alerts-overview-page"></a>Uyarılar genel bakış sayfası
**İzleyici - uyarıları** genel bakış sayfasında toplanan tüm Mazotlu uyarıların özetini gösterir ve toplam yapılandırılmış/etkin uyarı kuralları. Ayrıca, tüm Mazotlu uyarıların bir listesi gösterilir. Abonelikler ve filtre parametrelerini değiştirme toplamalar güncelleştirir ve liste uyarıları tetiklenir.

> [!NOTE]
> Uyarıları'nda gösterilen Mazotlu uyarıları desteklenen ölçüm ve etkinlik günlüğü uyarıları sınırlıdır; Azure İzleyiciye Genel Bakış eski Azure uyarıları de dahil Mazotlu uyarıları sayısını gösterir

 ![Uyarılar genel bakış](./media/monitoring-overview-unified-alerts/alerts-preview-overview2.png) 

### <a name="alert-rules-management"></a>Uyarı kuralları Yönetimi
**İzleyici - uyarıları > kuralları** tüm uyarı kuralları, Azure Aboneliklerini yönetmek için tek bir sayfadır. (Etkin veya devre dışı) tüm uyarı kurallarını listeler ve hedef kaynak, kaynak grupları, kural adı veya durum göre sıralanabilir. Uyarı kuralları da devre dışı ve etkin veya bu sayfadan düzenlenemez.  

 ![Uyarı kuralları](./media/monitoring-overview-unified-alerts/alerts-preview-rules.png)


## <a name="one-alert-authoring-experience-across-all-monitoring-sources"></a>Bir uyarı yazma deneyimi tüm izleme kaynakları
Yeni uyarılar deneyiminde uyarıları izleme hizmeti bağımsız olarak tutarlı bir şekilde yazılabilir veya türü sinyal. Tüm uyarıları tetiklenir ve ilgili ayrıntıları tek sayfa olarak kullanılabilir.  
 
Bir uyarı yazma olan üç adım görev burada kullanıcı uyarı için bir hedef ilk Çekmeleri sağ sinyal seçerek ve ardından sinyal uyarı kuralının bir parçası olarak uygulanması için mantığı belirterek arkasından. Bu Basitleştirilmiş yazma işlemi artık izleme kaynağı veya bir Azure kaynağı seçmeden önce desteklenen sinyalleri öğrenmek kullanıcının gerektirir. Ortak geliştirme deneyimi otomatik olarak seçilen hedef kaynak üzerindeki temel kullanılabilir sinyalleri listesini filtreler ve uyarı mantığı oluşturulmasını size yol gösterir

Uyarı türleri aşağıdaki oluşturma hakkında daha fazla bilgi edinebilirsiniz [burada](monitor-alerts-unified-usage.md).
- Ölçüm Uyarıları
- Günlük uyarıları (günlük analizi)
- Günlük uyarıları (etkinlik günlükleri)
- Günlük uyarıları (Application Insights)

 
## <a name="alerts-supported-in-new-experience"></a>Yeni deneyimi desteklenen uyarıları
Uyarıları hizmetlerini izleme birkaç Azure kullanılabilir. Hakkında bilgi edinmek ve bu hizmetleri kullanıldığı durumlar için [bu makaleye bakın](./monitoring-overview.md). İşte uyarı türlerini dökümünü kullanılabilir Azure ve yeni uyarılar deneyimi tarafından şu anda desteklenmiyor. 


| **Sinyal türü** | **Kaynağı izle** | **Açıklama** | 
|-------------|----------------|-------------|
| Ölçüm | Azure İzleyicisi | Olarak da bilinir [yakın gerçek zamanlı ölçüm uyarıları](monitoring-near-real-time-metric-alerts.md), ölçüm koşulları 1 dakika olarak sık değerlendiriliyor destekler ve birden çok ölçüm ve çok boyutlu ölçüm kuralları için izin. Desteklenen kaynak türleri listesi kullanılabilir [burada](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported). |
| Ölçüm | Azure İzleyicisi | [Eski classic ölçüm uyarıları](monitoring-overview-alerts.md) yeni uyarılar deneyimi desteklenmez. Bunları Azure portalında uyarılar (Klasik) altında bulabilirsiniz. Klasik uyarıları henüz yeni uyarılar taşınmamış olan bazı ölçümleri türleri desteklemez. Tam listesi için bkz: [ölçümleri desteklenen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-supported-metrics)
| Günlükler  | Log Analytics | Bildirimleri almak veya bir günlük arama sorgusu ölçüm ve/veya olay veriler üzerinde belirli ölçütleri karşıladığında otomatik eylemler çalıştırın. Eski günlük analizi uyarılar hala kullanılabilir, ancak [yeni deneyim halinde kopyalanmasını](monitoring-alerts-extend.md). Ayrıca, bir [, Önizleme *günlük analizi günlüklerini ölçümleri* ](monitoring-alerts-extend-tool.md) kullanılabilir. Önizleme günlükleri bazı türleri almak ve bunları burada, ardından yeni bir uyarı deneyim kullanarak üzerlerinde uyarabilir ölçümlere dönüştürmek sağlar. Önizleme yanı sıra yerel Azure İzleyici ölçümleri almak istediğiniz Azure olmayan günlükleri varsa faydalıdır. |
| Etkinlik Günlüğü | Etkinlik günlükleri (Genel) | Seçili hedef (kaynak/kaynak grubu/abonelik) gerçekleştirilen tüm oluşturma, güncelleştirme ve silme eylemlerini kayıtları içerir. |
| Etkinlik Günlüğü  | Hizmet Durumu | Yeni uyarılar deneyimi desteklenmiyor. Bkz: [etkinlik günlüğü uyarı hizmeti bildirimlerinin oluşturma](monitoring-activity-log-alerts-on-service-notifications.md).  |
| Günlükler  | Application Insights | Uygulamanızın performansı ayrıntılarla günlükleri içerir. Analytics sorgu kullanarak, geçen-uygulama verilerine dayalı eylemler için koşullar tanımlayabilirsiniz. |
| Ölçüm | Application Insights | Yeni uyarılar deneyimi desteklenmiyor. Bkz: [ölçüm uyarıları](../application-insights/app-insights-alerts.md) |
| Web kullanılabilirlik testleri | Application Insights | Uyarıları deneyimi desteklenmiyor.  Bkz: [Web testi uyarıları](../application-insights/app-insights-monitor-web-app-availability.md). Application Insights'a veri göndermek için izleme eklenmiş tüm Web sitesi için kullanılabilir. Kullanılabilirlik ve yanıt hızını bir Web sitesinin beklentilerini altında olduğunda bir bildirim alırsınız. |




## <a name="next-steps"></a>Sonraki adımlar
- [Yeni uyarılar deneyimi oluşturun, görüntüleyin ve Uyarıları yönetmek için nasıl kullanılacağını öğrenin](monitor-alerts-unified-usage.md)
- [Uyarıları deneyiminde günlük uyarılar hakkında bilgi edinin](monitor-alerts-unified-log.md)
- [Uyarıları deneyiminde ölçüm uyarılar hakkında bilgi edinin](monitoring-near-real-time-metric-alerts.md)
- [Etkinlik günlüğü uyarıları uyarılar deneyiminde hakkında bilgi edinin](monitoring-activity-log-alerts-new-experience.md)
