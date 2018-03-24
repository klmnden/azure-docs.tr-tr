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
ms.date: 02/02/2018
ms.author: mamit
ms.custom: ''
ms.openlocfilehash: bc9d788367ab14751f9f9158ac88149dc420368a
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="explore-the-alerts-experience-in-azure-monitor"></a>Azure İzleyicisi'nde uyarıları deneyimi keşfedin

## <a name="overview"></a>Genel Bakış
 Yeni Azure deneyimi olan uyarılar arayın ve güncelleştirilmiş işlevler. Bu yeni deneyim kullanılabilir **uyarıları** sekmesinde Azure İzleyici'nin altında. Bazı uyarılar (Klasik) deneyimine göre yeni uyarılar deneyimi kullanmanın avantajları şunlardır:

 - **Fired uyarıları ve uyarı kuralları ayrılması** - yeni uyarılar deneyimi kuralları (bir uyarıyı tetikleyen koşulu tanımı) uyarı ve tetiklenen uyarıları (uyarı kuralı tetikleme örneğini) Ayrıştırılan, böylece işletimsel ve Yapılandırma görünümleri ayrılır.
 - **A ölçümü, etkinlik günlüğü ve günlük uyarılar için rapor yazma deneyimi birleşik** - yeni uyarılar hakkında uyarı için doğru öğeleri bulmak daha basit hale getirir bir uyarı kuralı yapılandırma işlemi boyunca kullanıcı deneyimi kılavuzları yazma.
 - **Görünüm harekete Azure Portal'da günlük analizi uyarılarını** -uyarıları deneyimi yapabilecekleriniz Ayrıca bkz: günlük analizi uyarılar aboneliğinizde harekete artık.  
 

Aşağıdaki bölümlerde yeni deneyimi nasıl çalıştığını daha ayrıntılı olarak açıklanmaktadır.

## <a name="taxonomy"></a>Sınıflandırma
Uyarıları deneyimi kavramlar geliştirme deneyimi farklı uyarı türleri arasında birleştirin sırasında uyarı kuralı ve puanlı uyarı nesneleri ayırmak için kullanır.

- **Hedef kaynak** -bir hedef herhangi bir Azure kaynağı olabilir. Hedef kaynak uyarı için kullanılabilir sinyalleri ve kapsamını tanımlar. Örnek hedefler: bir sanal makine, bir depolama hesabı, bir sanal makine ölçek kümesi, bir günlük analizi çalışma alanı veya Application Insights kaynağı.

- **Ölçüt** - ölçütü sinyal birleşimi ve mantığı hedef kaynak üzerindeki uygulanır. Örnekler: Yüzdesi % CPU > 70, sunucu yanıt süresi > 4 ms, sonuç sayısı günlük sorgu > 100 vs. 

- **Sinyal** - hedef kaynak tarafından gösterilen işaret eder ve çeşitli türlerde olabilir. Bu önizleme destekleyen **ölçüm**, **etkinlik günlüğü**, **Application Insights** ve **günlük** sinyal türleri olarak.

- **Mantığı** -sinyal içinde olup olmadığını denetlemek için kullanıcı tanımlı mantık beklenen aralık/değerleri.  
 
- **Eylem** -bir alıcı (örneğin, bir adresi e-postayla gönderme veya bir Web kancası URL'si nakil) bir bildirim gönderilmesini belirli bir çağrı. Bildirimleri genellikle birden çok eylem tetikleyebilir. Bu Önizleme'de, destek eylem gruplarının desteklenen uyarı türleri.  
 
- **Uyarı kuralı** -uyarı tetikleyecek bir koşul tanımı. Bu Önizleme'de, uyarmak için ölçüt ve hedef uyarı kuralı yakalar. Uyarı kuralı, bir etkin veya devre dışı durumda olabilir.
 
    > [!NOTE]
    > Bu, burada uyarı kural ve Mazotlu uyarı temsil eder ve bu nedenle uyarı, etkin veya devre dışı durumlardan birinde olabilir uyarıları (Klasik) deneyiminde farklıdır.
    >

## <a name="single-place-to-view-and-manage-alerts"></a>Uyarıları görüntülemek ve yönetmek için tek yer
Uyarıları (Önizleme) deneyimi tüm Azure uyarıları görüntülemek ve yönetmek için tek yer hedefidir. Aşağıdaki alt bölümleri tek tek her yeni deneyimi ekranın işlevleri açıklanmaktadır.

### <a name="alerts-preview-overview-page"></a>Uyarıları (Önizleme) genel bakış sayfası
**İzleyici - uyarıları (Önizleme)** genel bakış sayfasında toplanan tüm Mazotlu uyarıların özetini gösterir ve toplam yapılandırılmış/etkin uyarı kuralları. Ayrıca, tüm Mazotlu uyarıların bir listesi gösterilir. Abonelikler ve filtre parametrelerini değiştirme toplamalar güncelleştirir ve liste uyarıları tetiklenir.

> [!NOTE]
> Uyarıları'nda gösterilen Mazotlu uyarıları desteklenen ölçüm ve activitylog uyarıları sınırlıdır; Azure İzleyiciye Genel Bakış eski Azure uyarıları de dahil yangın uyarıları sayısını gösterir

 ![alerts-overview](./media/monitoring-overview-unified/alerts-preview-overview.png) 

### <a name="alert-rules-management"></a>Uyarı kuralları Yönetimi
**İzleyici - uyarıları (Önizleme) > kuralları** tüm uyarı kuralları, Azure Aboneliklerini yönetmek için tek bir sayfadır. (Etkin veya devre dışı) tüm uyarı kurallarını listeler ve hedef kaynak, kaynak grupları, kural adı veya durum göre sıralanabilir. Uyarı kuralları da devre dışı ve etkin veya bu sayfadan düzenlenemez.  

 ![Uyarı kuralları](./media/monitoring-overview-unified/alerts-preview-rules.png)


## <a name="one-alert-authoring-experience-across-all-monitoring-sources"></a>Bir uyarı yazma deneyimi tüm izleme kaynakları
Yeni uyarılar deneyiminde uyarıları izleme hizmeti bağımsız olarak tutarlı bir şekilde yazılabilir veya türü sinyal. Tüm uyarıları tetiklenir ve ilgili ayrıntıları tek sayfa olarak kullanılabilir.  
 
Bir uyarı yazma olan üç adım görev burada kullanıcı uyarı için bir hedef ilk Çekmeleri sağ sinyal seçerek ve ardından sinyal uyarı kuralının bir parçası olarak uygulanması için mantığı belirterek arkasından. Bu Basitleştirilmiş yazma işlemi artık izleme kaynağı veya bir Azure kaynağı seçmeden önce desteklenen sinyalleri öğrenmek kullanıcının gerektirir. Ortak geliştirme deneyimi otomatik olarak seçilen hedef kaynak üzerindeki temel kullanılabilir sinyalleri listesini filtreler ve uyarı mantığı oluşturulmasını size yol gösterir

Uyarı türleri aşağıdaki oluşturma hakkında daha fazla bilgi edinebilirsiniz [burada](monitor-alerts-unified-usage.md).
- (Yakın gerçek zamanlı ölçüm uyarıları geçerli deneyimi denir) ölçüm uyarıları
- Günlük uyarıları (günlük analizi)
- Günlük uyarıları (etkinlik günlükleri)
- Günlük uyarıları (Application Insights)

 

## <a name="alert-types-supported-in-this-preview"></a>Bu Önizleme'de desteklenen uyarı türleri


| **Sinyal türü** | **Kaynağı izle** | **Açıklama** | 
|-------------|----------------|-------------|
| Ölçüm | Azure İzleyicisi | Adlı [ **yakın gerçek zamanlı ölçüm uyarıları**](monitoring-near-real-time-metric-alerts.md), ölçüm Bu uyarılar ölçüm koşulları gibi bir sıklıkla 1 dak değerlendiriliyor destekler ve çok ölçüm kuralları için izin verir. Desteklenen kaynak türleri listesi kullanılabilir [burada](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported). Eski ölçüm uyarıları tanımlanan [burada](monitoring-overview-alerts.md#alerts-in-different-azure-services) yeni desteklenmez uyarıları deneyimi. Uyarıları (Klasik) altında bulabilirsiniz|
| Günlükler  | Log Analytics | Bildirimleri almak veya bir günlük arama sorgusu ölçüm ve/veya olay veriler üzerinde belirli ölçütleri karşıladığında otomatik eylemler çalıştırın.|
| Etkinlik Günlüğü | Etkinlik Günlükleri | Bu kategorideki tüm oluşturma, güncelleştirme, kayıtları içerir ve seçilen hedef (kaynak/kaynak grubu/abonelik) gerçekleştirilen eylemler silin. |
| Günlükler  | Hizmet durumu günlükleri | Uyarıları deneyimi desteklenmiyor.   |
| Günlükler  | Application Insights | Bu kategori, uygulamanızın performansını ayrıntılarla günlükleri içerir. Analytics sorgu kullanarak, geçen-uygulama verilerine dayalı eylemler için koşullar tanımlayabilirsiniz. |
| Ölçüm | Application Insights | Uyarıları deneyimi desteklenmiyor. Uyarıları (Klasik) altında bulabilirsiniz |
| Kullanılabilirlik testleri | Application Insights | Uyarıları deneyimi desteklenmiyor. Uyarıları (Klasik) altında bulabilirsiniz |


## <a name="next-steps"></a>Sonraki adımlar
- [Yeni uyarılar deneyimi oluşturun, görüntüleyin ve Uyarıları yönetmek için nasıl kullanılacağını öğrenin](monitor-alerts-unified-usage.md)
- [Uyarıları deneyiminde günlük uyarılar hakkında bilgi edinin](monitor-alerts-unified-log.md)
- [Uyarıları deneyiminde ölçüm uyarılar hakkında bilgi edinin](monitoring-near-real-time-metric-alerts.md)
- [Etkinlik günlüğü uyarıları uyarılar deneyiminde hakkında bilgi edinin](monitoring-activity-log-alerts-new-experience.md)
