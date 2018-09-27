---
title: Azure bakım zamanlamaları (Önizleme) | Microsoft Docs
description: Bakım zamanlaması, müşterilerin yeni özellikler, yükseltmeleri ve düzeltme eklerini almak için Azure SQL veri ambarı hizmeti kullanan gerekli zamanlanmış bakım olayları geçici planı olanak tanır.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 10/07/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: a6eedc0bac7aab69a9138f4f63d0d9d802e74dfc
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47228452"
---
# <a name="change-a-maintenance-schedule"></a>Bakım zamanlamasını değiştirme 

## <a name="portal"></a>Portal
Bakım zamanlaması güncelleştirilemiyor veya herhangi bir zamanda değiştirilebilir. Ancak, seçilen örneği şu anda ise etkin bakım ayarları döngüsü boyunca kaydedilebilir ve sonraki tanımlanan bakım süresi boyunca etkin olur. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/service-health/resource-health-overview) bir etkin bir bakım olayı sırasında veri ambarınızın izlenmesine dair. 

Önizleme'de, biz 7 günlük süre içinde iki bakım pencerelerini seçmenizi soruyor. Her bir bakım penceresinin 3 ila 8 saat arasında her olabilir 3 saat yüklenmekte olan en kısa kullanılabilir seçenek. Bakım tanımlanan bakım penceresi içinde herhangi bir zamanda gerçekleşebilir ancak olanlar dışındaki zaman pencereleri önceki bildirimi ile tanımlanan gerçekleşmez da olacak deneyimi hizmeti verilerinizi yeni kod dağıtır gibi kısa bağlantı kaybı olabilir ambarı. 

## <a name="identifying-the-primary-and-secondary-windows"></a>Birincil ve ikincil windows tanımlama

Birincil ve ikincil windows (diğer bir deyişle, birincil penceresinde (Salı – Perşembe), ikincil penceresi (Cumartesi – Pazar) içinde ayrı bir gün aralığı tanımlanmalıdır

Veri ambarınıza portalında uygulanan bakım zamanlamasını değiştirmek için aşağıdaki adımları tamamlayın.
1.  [Azure Portal](https://portal.azure.com/) oturum açın.
2.  Güncelleştirmek istediğiniz veri ambarı'nı seçin. Genel Bakış dikey penceresinde sayfasını açar. 
3.  Bakım zamanlamasını ayarlar sayfasına tıklayarak ya da bakım zamanlama (Önizleme) Özet bağlantısına genel bakış dikey penceresinde veya sol taraftaki ve kaynak menüsünde bakım zamanlaması seçeneği aracılığıyla erişilebilir.  

    ![Genel Bakış dikey penceresinde seçenekleri](media/sql-data-warehouse-maintenance-scheduling/maintenance-change-option.png)

4. Sayfanın en üstündeki radyo düğmelerini kullanarak, birincil bir bakım penceresi için tercih edilen bir gün aralığı tanımlayabilirsiniz. Bu seçim, bir hafta içi gün veya hafta sonu üzerinden birincil pencereniz meydana gelir, belirler. Seçiminizi aşağıdaki açılır değerleri uygun şekilde güncelleştirin. Bazı bölgelerde, Önizleme sırasında kullanılabilir günlük seçenekleri kümesinin henüz desteklemiyor olabilir. Bu değerler, önümüzdeki aylarda güncelleştirir.

   ![Bakım ayarlar dikey penceresi](media/sql-data-warehouse-maintenance-scheduling/maintenance-settings-page.png)

5. Gün kullanarak tercih edilen birincil ve ikincil bakım pencereleri başlangıç saatini ve saat penceresinde açılan seçin. Özet dikey pencerenin alt kısmındaki güncelleştirecek zamanlamayı Seçili açılan değerlerine göre.

#### <a name="dropdown-options"></a>Açılır Seçenekler
- Gün: Seçili penceresi sırasında bakım gerçekleştirmek için tercih edilen gün.
- Başlangıç zamanı: bakım penceresi başlangıç zamanı tercih edilir.
- Zaman penceresi: tercih edilen zaman pencerenizin süresini.

  Tercih edilen bakım pencereleri seçtikten sonra Kaydet'e tıklayın. Yeni, zamanlama etkin değil. onaylayan bir onay iletisi görüntülenir. Zamanlama bakım henüz desteklemeyen bir bölgede bir zamanlama kaydediyorsanız, ardından aşağıdaki ileti görünür. Ayarlarınızı kaydedilir ve bu özellik, seçili bölgesinde kullanılabilir olduğunda etkinleşir.    

    ![Bölge bildirim etkin değil](media/sql-data-warehouse-maintenance-scheduling/maintenance-notactive-toast.png)

## <a name="next-steps"></a>Sonraki adımlar
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) günlük uyarısı kuralları için Web kancası eylemleri hakkında.
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/service-health/service-health-overview) Azure hizmet durumu hakkında


