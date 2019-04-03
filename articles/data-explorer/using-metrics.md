---
title: Azure Veri Gezgini performansını, sistem durumu ve kullanım ölçümleri ile izleme
description: Kümenin performans, durumunu ve kullanımını izlemek için Azure Veri Gezgini ölçümleri'ni kullanmayı öğrenin.
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: gabil
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/01/2019
ms.openlocfilehash: 5252ca8898439b63a8819f6abfd634de0786932b
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58851935"
---
# <a name="monitor-azure-data-explorer-performance-health-and-usage-with-metrics"></a>Azure Veri Gezgini performansını, sistem durumu ve kullanım ölçümleri ile izleme

Azure Veri Gezgini uygulamalar, web siteleri, IoT cihazları ve daha fazlasından akışı yapılan büyük miktarda veri üzerinde gerçek zamanlı analiz yapmaya yönelik hızlı ve tam olarak yönetilen bir veri analizi hizmetidir. Azure veri gezginini kullanmak için ilk küme oluşturma ve bu kümede bir veya daha fazla veritabanı oluşturun. Ardından karşı sorgular çalıştırabileceği şekilde onlara bir veritabanına (yükle) veri alın. Azure Veri Gezgini ölçümleri durumunu ve performansını küme kaynakları için ana göstergelerin sağlar. Bu makalede Azure Veri Gezgini küme durumunu ve tek başına ölçümler olarak kendi senaryonuza performansını izlemek için ayrıntılı ölçümleri kullanın. Ölçümler için temel olarak kullanabilirsiniz işletimsel [Azure panoları](/azure/azure-portal/azure-portal-dashboards) ve [Azure uyarıları](/azure/azure-monitor/platform/alerts-metric-overview).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa, oluşturun bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/).

* Oluşturma bir [kümesi ile veritabanı](create-cluster-database-portal.md).

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="using-metrics"></a>Ölçümleri kullanarak

Azure Veri Gezgini kümenizi seçin **ölçümleri** ölçümleri bölmesini açın ve kümenizde analize başla.

![Ölçümleri Seç](media/using-metrics/select-metrics.png)

Ölçümleri Bölmesi'nde:

![Ölçümleri bölmesi](media/using-metrics/metrics-pane.png)

1. Bir ölçüm grafiği oluşturmak için Seç **ölçüm** adını ve ilgili **toplama** ölçüm aşağıda ayrıntılı olarak. **Kaynak** ve **ölçüm Namespace** seçiciler, Azure Veri Gezgini kümenize önceden seçilmiş.

    **Ölçüm** | **Birim** | **Toplama** | **Ölçüm tanımı**
    |---|---|---|---|
    | Önbellek kullanımı | Yüzde | AVG, Max, Min | Gerekli önbellek boyutu (göre tanımlanmış önbellek İlkesi) ve toplam önbellek boyutu (toplam boyutunu verilen kullanıcı etkinliklerine SSD) kümesi arasındaki oran. Bir ortalama önbellek kullanımı % 80 veya daha küçük bir küme için sürdürülebilir bir durumdur. Ortalama önbellek kullanımı % 80'in ise, kümenin olmalıdır [yukarı ölçeklendirilemez](manage-cluster-scale-up.md) fiyatlandırma katmanı için bir depolama için iyileştirilmiş veya [ölçeği](manage-cluster-scale-out.md) daha fazla örnek için. Alternatif olarak, önbellek İlkesi (daha az gün önbellekte) uyarlayın. Önbellek kullanımı % 100 üzerinde ise, önbelleğe alma ilkesine göre önbelleğe veri boyutu büyüktür, önbellek kümesinde toplam boyutu. |
    | CPU | Yüzde | AVG, Max, Min | Toplam CPU kullanılan ve tüm kümede kullanılabilir CPU arasındaki oran. Bir ortalama CPU % 80 veya daha küçük bir küme için sürdürülebilir. En fazla CPU veriyi işlemek için ek işlem kaynak yok anlamına gelir % 100 değeri. Bir küme iyi değil, engellenen belirli CPU'ları olup olmadığını belirlemek için CPU en büyük değerini kontrol edin. |
    | (Event Hubs için) işlenen olaylar | Sayı | Max, Min, TOPLA | Toplam olay sayısı olay hub'ları tarafından gönderilen ve küme tarafından alındı. Reddedilen olaylar ve küme altyapısı tarafından kabul edilen olaylar olayların ayrılır. |
    | Alma gecikmesi | Saniye | AVG, Max, Min | Sorgu için hazır olana kadar veri kümesinde alındığı zamandan alınan veri gecikme süresi. Alma gecikmesi saniye cinsinden ölçülür. Alma gecikme süresini alımı senaryoya bağlıdır. |
    | Alma sonucu | Sayı | Sayı | Başarısız ve başarılı alma işlemlerinin toplam sayısı. Kullanım **bölme uygulamak** başarı demet oluşturma ve sonuçları başarısız.|
    | Alımı kullanımı | Yüzde | AVG, Max, Min | Veri alımı için kullanılan gerçek kaynakları ve kapasitesini ilke alımı yapmak için ayrılan toplam kaynaklar arasındaki oran. Varsayılan kapasite en fazla 512 eşzamanlı alımı işlemlerini veya %75 alım yatırım küme kaynaklarının ilkesidir. Ortalama alımı kullanımı % 80 veya daha küçük bir küme için sürdürülebilir bir durumdur. En fazla alımı kullanımı alma sırası neden olabilir ve tüm küme alma özelliği kullanılır yani % 100 değeri. |
    | Alma birim (MB cinsinden) | Sayı | Max, Min, TOPLA | Veri kümesine (MB cinsinden) alınan toplam boyutu. Alınan veri sıkıştırma önce MB sayısının birimleridir. |
    | Canlı | Sayı | Ortalama | Kümenin yanıtlama izler. Tamamen esnek bir küme 1 değerini döndürür ve 0 engellenen veya bağlantısı kesilmiş bir küme döndürür. |
    | Sorgu süresi | Saniye | Sayısı, ortalama, Min, Max, TOPLA | Sorgu sonuçları alınana kadar toplam süresi. |
    | | | |

    Ek bilgi ilgili [desteklenen Azure Veri Gezgini küme ölçümleri](/azure/azure-monitor/platform/metrics-supported#microsoftkustoclusters)

2. Seçin **ölçüm Ekle** birden çok ölçümleri aynı grafikte çizilen görmek için düğme.
3. Seçin **+ yeni grafik** birden fazla grafiği tek bir görünümde görmek için düğme.
4. Saat Seçici zaman aralığını değiştirmek için kullanın (varsayılan: Son 24 saat).
5. Kullanım [ **Filtre Ekle** ve **uygulamak bölme** ](/azure/azure-monitor/platform/metrics-getting-started#apply-dimension-filters-and-splitting) boyutlara sahip ölçümler için.
6. Seçin **panoya Sabitle** yeniden görüntüleyebilmek grafik yapılandırmanızı panoya eklemek için.
7. Ayarlama **yeni uyarı kuralı** ölçütleri kullanarak ölçümlerinizi görselleştirmek için. Yeni uyarı verme kuralı, hedef kaynak, ölçüm, bölme ve grafik filtresi boyutlardan içerir. Bu ayarları değiştirmek [uyarı kuralı oluşturma bölmesinde](/azure/azure-monitor/platform/metrics-charts#create-alert-rules).

Kullanma hakkında ek bilgi [ölçüm Gezgini](/azure/azure-monitor/platform/metrics-getting-started).


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Azure veri Gezgini'nde verileri Sorgulama](web-query-data.md)
