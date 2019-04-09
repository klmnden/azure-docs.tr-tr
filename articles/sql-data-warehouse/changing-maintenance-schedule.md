---
title: Azure bakım zamanlamaları (Önizleme) | Microsoft Docs
description: Bakım zamanlaması, müşterilerin yeni özellikler, yükseltmeleri ve düzeltme eklerini alma için Azure SQL veri ambarı hizmetini kullanan gerekli zamanlanmış bakım olayları geçici planı sağlar.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: design
ms.date: 04/03/2019
ms.author: anvang
ms.reviewer: jrasnick
ms.openlocfilehash: 4bc1867f0dd18a101b2e764970de4e1904b960e0
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59276329"
---
# <a name="change-a-maintenance-schedule"></a>Bakım zamanlamasını değiştirme 

## <a name="portal"></a>Portal
Bakım zamanlaması güncelleştirilemiyor veya herhangi bir zamanda değiştirilebilir. Seçilen örnek bir etkin bakım döngüsü boyunca olacaksa, ayarları kaydedilir. Bunlar sonraki tanımlanan bakım süresi boyunca etkin hale gelirler. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/service-health/resource-health-overview) bir etkin bir bakım olayı sırasında veri ambarınızın izlenmesine dair. 

Bakım zamanlamaları kullanmak için yedi günlük süre içinde iki bakım pencereleri seçmeniz gerekir. Her bir bakım penceresi için üç sekiz saat olabilir. Bakım bir bakım penceresi içinde herhangi bir zamanda gerçekleşebilir ancak önceden bildirimde bulunmadan zaman pencereleri dışında karşılaşılmaz. Hizmet, veri ambarınızı yeni kod dağıtır gibi kısa bir bağlantı kaybı ayrıca deneyeceksiniz. 

## <a name="identifying-the-primary-and-secondary-windows"></a>Birincil ve ikincil windows tanımlama

Birincil ve ikincil windows ayrı gün aralıklarından olması gerekir. Salı – Perşembe birincil bir pencerenin ve penceresinin Cumartesi – Pazar, ikincil bir örnektir.

Veri ambarınız için bakım zamanlamasını değiştirmek için aşağıdaki adımları tamamlayın:
1.  [Azure Portal](https://portal.azure.com/) oturum açın.
2.  Güncelleştirmek istediğiniz veri ambarı'nı seçin. Genel Bakış dikey penceresinde açar. 
3.  Bakım zamanlaması ayarları sayfasını seçerek açın **bakım zamanlaması (Önizleme) Özet** bağlantısına genel bakış dikey penceresinde. Ya da seçin **bakım zamanlaması** sol taraftaki kaynak menüsündeki seçeneği.  

    ![Genel Bakış dikey penceresinde seçenekleri](media/sql-data-warehouse-maintenance-scheduling/maintenance-change-option.png)

4. Sayfanın en üstündeki Seçenekleri'ni kullanarak, birincil bir bakım penceresi için tercih edilen gün aralığı belirleyin. Bu seçim, bir hafta içi gün veya hafta sonu üzerinden birincil pencereniz meydana gelir, belirler. Seçiminizi açılan değerlerini güncelleştirir. Önizleme sırasında bazı bölgelerde kullanılabilen tam kümesini henüz desteklemeyebilir **gün** seçenekleri.

   ![Bakım ayarlar dikey penceresi](media/sql-data-warehouse-maintenance-scheduling/maintenance-settings-page.png)

5. Aşağı açılan liste kutuları kullanarak, birincil ve ikincil tercih edilen bakım pencereleri seçin:
   - **Gün**: Seçili penceresi sırasında bakım gerçekleştirmek için tercih edilen gün.
   - **Başlangıç saati**: Başlangıç zamanı bakım penceresi için tercih edilir.
   - **Zaman penceresi**: Tercih edilen, zaman penceresi süresi.

   **Zamanlama özeti** dikey atındaki seçtiğiniz değerlerine göre güncelleştirilir. 
  
6. **Kaydet**’i seçin. Yeni tablonuza etkin olduğunu onaylayan bir ileti görüntülenir. 

   Zamanlama bakım desteklemeyen bir bölgede bir zamanlama kaydediyorsanız, aşağıdaki ileti görüntülenir. Ayarlarınızı kaydedilir ve bu özellik, seçili bölgesinde kullanılabilir olduğunda etkinleşir.    

   ![Bölge kullanılabilirliği hakkında ileti](media/sql-data-warehouse-maintenance-scheduling/maintenance-notactive-toast.png)

## <a name="next-steps"></a>Sonraki adımlar
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) günlük uyarısı kuralları için Web kancası eylemleri hakkında.
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/service-health/service-health-overview) Azure hizmet durumu hakkında.


