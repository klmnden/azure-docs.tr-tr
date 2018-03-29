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
ms.openlocfilehash: 356988e8ae743d73c8e2cc7cc106cbc5b0d1a423
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="the-new-alerts-experience-in-azure-monitor"></a>Azure İzleyicisi'nde yeni uyarılar deneyimi

## <a name="overview"></a>Genel Bakış
Yeni deneyimi uyarılar var. Deneyimidir şimdi uyarılar (Klasik) sekmesi altında eski uyarılar. Yeni uyarılar deneyimi uyarıları (Klasik) deneyimi aşağıdaki faydaları vardır:

 - **Fired uyarıları ve uyarı kuralları ayrılması** - uyarı kuralları (bir uyarıyı tetikleyen koşulu tanımı) ve tetiklenen uyarıları (uyarı kuralı tetikleme örneğini) Ayrıştırılan, işletimsel ve yapılandırma görünümleri ayrılmış şekilde.
 - **Bir birleşik yazma deneyimi** - tüm oluşturma için ölçümleri, uyarı günlükleri ve etkinlik oturum Azure monitöre günlük analizi ve Application Insights içinde bir yerdir. 
 - **Görünüm harekete Azure Portal'da günlük analizi uyarılarını** -Ayrıca bkz: günlük analizi uyarılar aboneliğinizde harekete artık şunları yapabilirsiniz. Daha önce bu ayrı bir portal yoktu. 
 - **Daha iyi iş akışı** - yeni uyarılar hakkında uyarı için doğru öğeleri bulmak daha basit hale getirir bir uyarı kuralı yapılandırma işlemi boyunca kullanıcı deneyimi kılavuzları yazma.
 

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

 ![alerts-overview](./media/monitoring-overview-unified/alerts-preview-overview.png) 

### <a name="alert-rules-management"></a>Uyarı kuralları Yönetimi
**İzleyici - uyarıları > kuralları** tüm uyarı kuralları, Azure Aboneliklerini yönetmek için tek bir sayfadır. (Etkin veya devre dışı) tüm uyarı kurallarını listeler ve hedef kaynak, kaynak grupları, kural adı veya durum göre sıralanabilir. Uyarı kuralları da devre dışı ve etkin veya bu sayfadan düzenlenemez.  

 ![Uyarı kuralları](./media/monitoring-overview-unified/alerts-preview-rules.png)


## <a name="one-alert-authoring-experience-across-all-monitoring-sources"></a>Bir uyarı yazma deneyimi tüm izleme kaynakları
Yeni uyarılar deneyiminde uyarıları izleme hizmeti bağımsız olarak tutarlı bir şekilde yazılabilir veya türü sinyal. Tüm uyarıları tetiklenir ve ilgili ayrıntıları tek sayfa olarak kullanılabilir.  
 
Bir uyarı yazma olan üç adım görev burada kullanıcı uyarı için bir hedef ilk Çekmeleri sağ sinyal seçerek ve ardından sinyal uyarı kuralının bir parçası olarak uygulanması için mantığı belirterek arkasından. Bu Basitleştirilmiş yazma işlemi artık izleme kaynağı veya bir Azure kaynağı seçmeden önce desteklenen sinyalleri öğrenmek kullanıcının gerektirir. Ortak geliştirme deneyimi otomatik olarak seçilen hedef kaynak üzerindeki temel kullanılabilir sinyalleri listesini filtreler ve uyarı mantığı oluşturulmasını size yol gösterir

Uyarı türleri aşağıdaki oluşturma hakkında daha fazla bilgi edinebilirsiniz [burada](monitor-alerts-unified-usage.md).
- Ölçüm Uyarıları
- Günlük uyarıları (günlük analizi)
- Günlük uyarıları (etkinlik günlükleri)
- Günlük uyarıları (Application Insights)

 

## <a name="alert-types-supported"></a>Desteklenen uyarı türleri


| **Sinyal türü** | **Kaynağı izle** | **Açıklama** | 
|-------------|----------------|-------------|
| Ölçüm | Azure İzleyicisi | Olarak da bilinir [ **yakın gerçek zamanlı ölçüm uyarıları**](monitoring-near-real-time-metric-alerts.md), ölçüm Bu uyarılar ölçüm koşulları gibi bir sıklıkla 1 dak değerlendiriliyor destekler ve çok ölçüm kuralları için izin verir. Desteklenen kaynak türleri listesi kullanılabilir [burada](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported). Eski ölçüm uyarıları tanımlanan [burada](monitoring-overview-alerts.md#alerts-in-different-azure-services) yeni desteklenmez uyarıları deneyimi. Uyarıları (Klasik) altında bulabilirsiniz|
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
