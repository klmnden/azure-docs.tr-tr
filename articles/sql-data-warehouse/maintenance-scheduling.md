---
title: Azure bakım zamanlamaları (Önizleme) | Microsoft Docs
description: Bakım zamanlaması, müşterilerin yeni özellikler, yükseltmeleri ve düzeltme eklerini alma için Azure SQL veri ambarı hizmetini kullanan gerekli zamanlanmış bakım olayları geçici planı sağlar.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 09/20/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: c2ed79673af3563ae62f516057a174770cda99e9
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49427871"
---
# <a name="use-maintenance-schedules-to-manage-service-updates-and-maintenance"></a>Hizmet güncelleştirmeleri ve Bakım yönetmek için bakım zamanlamaları kullanın

Azure SQL veri ambarı bakım zamanlama artık Önizleme aşamasındadır. Bu özellik, hizmet durumu planlı bakım bildirimlerini, kaynak sistem durumu İzleyicisi'ni kontrol edin ve Azure SQL veri ambarı bakım zamanlama hizmetini tümleştirir.

Yeni özellikler, yükseltmeleri ve düzeltme eki almaya uygun olduğunda, bir zaman penceresi seçmek için zamanlama bakım kullanırsınız. Yedi günlük süre içinde bir birincil ve ikincil bir bakım penceresi seçin. Örnek bir Cumartesi birincil penceredir 22:00 ile Pazar 01:00 ve ikincil bir pencere, Çarşamba 19:00 için 22:00. SQL veri ambarı bakım, birincil bir bakım penceresi sırasında gerçekleştiremiyorsanız bakım, ikincil bir bakım penceresi sırasında yeniden deneyecek.

Tüm yeni Azure SQL veri ambarı oluşturulan dağıtım sırasında uygulanan bir sistem tarafından tanımlanan bakım zamanlaması örnekleri sahip olur. Dağıtım tamamlandıktan hemen sonra zamanlama düzenlenebilir.

Her bir bakım penceresi için üç sekiz saat olabilir. Bakım penceresi içinde herhangi bir zamanda meydana gelebilir. Hizmet, veri ambarınızı yeni kod dağıtır gibi kısa bir bağlantı kaybı beklemelisiniz. 

Özellik önizlemesi sırasında birincil ve ikincil pencereleri içinde ayrı bir gün aralığı belirleyin. Tüm bakım işlemleri zamanlanmış bakım pencereleri içinde tamamlanmalıdır. Herhangi bir bakım önceden bildirimde bulunmadan belirtilen bakım penceresi dışında yer alacak. Planlanan bakım sırasında veri ambarınız duraklatıldığı, sürdürme işlemi sırasında güncelleştirilir.  


## <a name="alerts-and-monitoring"></a>Uyarı ve izleme

Hizmet durumu bildirimi ve kaynak sistem durumu İzleyicisi denetleyin ile tümleştirme, müşterilerin yaklaşan bakım etkinliğini bilgilendirilmenizi sağlar. Yeni Otomasyon Azure İzleyici yararlanır. Yaklaşan bakım olayları almak istediğiniz karar verebilirsiniz. Ayrıca hangi otomatikleştirilmiş akışlar kapalı kalma süresi yönetmenize ve işlemlerinizi etkisini en aza yardımcı olabilir karar verin.

Bir 24 saatlik sağladığımız ön bildirimi tüm bakım olayları önce gelir. Örnek kapalı kalma süresini en aza indirmek için veri Ambarınızı seçilen bakım süreniz önce hiçbir uzun süre çalışan işlemler olduğundan emin olun. Bakım başladığında, tüm etkin oturumlar iptal edilir. Noncommitted işlemler geri alınacak ve veri ambarınız bir kısa bağlantı kaybı yaşar. Hemen bakım veri ambarınıza tamamlandığında size bildirilir. 

Bakım gerçekleşir, ancak SQL veri ambarı, bu süre boyunca bakım gerçekleştiremiyor sağladığımız ön bildirimi aldıysanız, iptal bildirimi alırsınız. Bakım, ardından bir sonraki zamanlanmış bakım süresi boyunca devam edecek.
 
Tüm etkin bakım olayları görünür **hizmet durumu - planlı Bakım** bölümü. Hizmet durumu geçmişi geçmiş olayların tam bir kaydını içerir. Etkin bir olayı sırasında bakım aracılığıyla Azure hizmet durumu onay portal panosunda izleyebilirsiniz.

### <a name="maintenance-schedule-availability"></a>Bakım zamanlaması kullanılabilirlik

Bakım zamanlaması, seçili bölgesinde kullanılabilir durumda değilse, görüntüleyebilir ve Bakım zamanlamanızı herhangi bir zamanda düzenleyin. Bakım zamanlaması Bölgenizde kullanılabilir hale geldiğinde, tanımlanan zamanlamaya hemen veri ambarınıza etkin hale gelir.


## <a name="next-steps"></a>Sonraki adımlar

- [Daha fazla bilgi edinin](viewing-maintenance-schedule.md) bakım zamanlaması görüntüleme hakkında. 
- [Daha fazla bilgi edinin](changing-maintenance-schedule.md) bakım zamanlamasını değiştirme hakkında.
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-usage) oluşturma, görüntüleme ve Azure İzleyici'yi kullanarak Uyarıları yönetme hakkında.
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) günlük uyarısı kuralları için Web kancası eylemleri hakkında.
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/service-health/service-health-overview) Azure hizmet durumu hakkında.







