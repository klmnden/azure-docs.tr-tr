---
title: Azure bakım zamanlamaları (Önizleme) | Microsoft Docs
description: Bakım zamanlaması, müşterilerin yeni özellikler, yükseltmeleri ve düzeltme eklerini almak için Azure SQL veri ambarı hizmeti kullanan gerekli zamanlanmış bakım olayları geçici planı olanak tanır.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 09/20/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: 85e8327d1cac17059b2e91b1ea21becc23002c8e
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47228469"
---
# <a name="using-maintenance-schedules-to-manage-service-updates-and-maintenance"></a>Hizmet güncelleştirmeleri ve Bakım yönetmek için bakım zamanlamaları kullanma

Azure SQL veri ambarı bakım zamanlama artık Önizleme aşamasındadır. Bu yeni özellik, hizmet durumu planlı bakım bildirimlerini, kaynak sistem durumu İzleyicisi'ni kontrol edin ve Azure SQL veri ambarı bakım zamanlama hizmeti sorunsuzca tümleştirilir.

Bakım zamanlaması, yeni özellikleri, yükseltmeleri almak uygun olduğunda, bir zaman penceresi zamanlamanıza olanak sağlar ve düzeltme ekleri. Müşteriler seçer birincil ve ikincil bir bakım penceresi 7 günlük süre içinde yani Cumartesi 22:00 – Pazar 01:00 (birincil) ve Çarşamba 19:00 – 22:00 (ikincil). Biz, birincil bir bakım penceresi sırasında bakım gerçekleştirmeye bulamıyorsanız, biz, ikincil bir bakım penceresi sırasında yeniden deneyecek.

Tüm yeni Azure SQL veri ambarı oluşturulan dağıtım sırasında uygulanan bir sistem tarafından tanımlanan bakım zamanlaması örnekleri sahip olur. Dağıtım tamamlandıktan hemen sonra zamanlama düzenlenebilir.

Her bir bakım penceresinin 3 ila 8 saat arasında her biri, 3hrs ile kısa kullanılabilir seçenek yüklenmekte. Bakım penceresi içinde herhangi bir zamanda meydana gelebilir ve hizmet, veri ambarınızı yeni kod dağıtır gibi kısa bir bağlantı kaybı beklemelisiniz. 

Özellik önizlemesi sırasında sizden ayrı gün aralıklarından içinde birincil ve ikincil pencerelerini tanımlamak için müşterilerin seçmenizi istiyoruz.   
Zamanlanan bakım pencereleri içinde tamamlanması gereken tüm bakım işlemleri ve herhangi bir bakım önceden bildirimde bulunmadan belirtilen bakım süreleri dışında yer alacak. Planlanan bakım sırasında veri ambarınız duraklatıldığı, sürdürme işlemi sırasında güncelleştirilir.  


## <a name="alerts-and-monitoring"></a>Uyarı ve izleme

İzleyici müşterilerin yaklaşan bakım etkinliğini bilgilendirilmenizi sağlar. Hizmet durumu bildirimlerine ile sorunsuz tümleştirme ve kaynak durumunu denetleyin. Yeni Otomasyon, Azure İzleyici yararlanır ve yaklaşan Bakımı olayları almak istedikleri ve hangi akışları otomatik kapalı kalma süresi yönetmek ve işlemlerini etkisini en aza indirmek için tetiklenmesinin gerekip nasıl uyacaklarını belirlemelerini sağlar.

Tüm bakım olayları, 24 saat sağladığımız ön bildirimi tarafından öncesinde olur. Örnek kapalı kalma süresini en aza indirmek için seçilen bakım döneminizin başlangıç önce veri ambarı'nızı üzerinde hiçbir uzun süre çalışan işlem emin olmalısınız. Bakım başlatıldığında tüm etkin oturumlar iptal edilecek, kaydedilmemiş işlemler geri alınacak ve veri ambarınız bir kısa bağlantı kaybı yaşar. Bakım veri ambarınıza tamamlandıktan hemen sonra size bildirilir. 

Bakım gerçekleşir, ancak bu işlem sırasında bakım gerçekleştirmeye belirleyemiyoruz sağladığımız ön bildirimi aldıysanız, iptal bildirimi alırsınız. Bakım, ardından bir sonraki zamanlanmış bakım süresi boyunca devam edecek.
 
Tüm etkin bakım olayları 'hizmet Durumu'nda yer - planlı Bakım' görüntülenecek bölümü. Tam geçmiş olaylar kaydını hizmet durumu geçmişi bir parçası olarak korunur. Bakım sırasında etkin bir olay aracılığıyla Azure hizmet durumu onay portal panosuna izlenebilir.

### <a name="maintenance-schedule-availability"></a>Bakım zamanlaması kullanılabilirlik

Bakım zamanlaması henüz seçili Bölgenizde kullanılabilir olmasa bile, yine de görüntüleyebilir ve Bakım zamanlamanızı herhangi bir zamanda düzenleyin. Bakım zamanlaması Bölgenizde kullanılabilir hale geldiğinde, tanımlanan zamanlamaya hemen veri ambarınıza etkin hale gelir.


## <a name="next-steps"></a>Sonraki adımlar

- [Daha fazla bilgi edinin](viewing-maintenance-schedule.md) bakım zamanlaması görüntüleme hakkında 
- [Daha fazla bilgi edinin](changing-maintenance-schedule.md) bakım zamanlamasının değiştirilmesi hakkında
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-usage) oluşturma, görüntüleme ve Azure İzleyicisi'ni kullanarak Uyarıları yönetme hakkında
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) günlük uyarısı kuralları için Web kancası eylemleri hakkında
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/service-health/service-health-overview) Azure hizmet durumu hakkında







