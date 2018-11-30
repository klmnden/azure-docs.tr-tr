---
title: Azure bakım zamanlamaları (Önizleme) | Microsoft Docs
description: Bakım zamanlaması, müşterilerin yeni özellikler, yükseltmeleri ve düzeltme eklerini alma için Azure SQL veri ambarı hizmeti kullanan gerekli zamanlanmış bakım olayları geçici planı sağlar.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 11/27/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: 3c683d5819ded4eb3d286969b8c3fc3f0bd42865
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52497506"
---
# <a name="view-a-maintenance-schedule"></a>Bakım zamanlaması görüntüleyin 

## <a name="portal"></a>Portal

Varsayılan olarak, dağıtım sırasında uygulanan bir sekiz saatlik birincil ve ikincil bakım penceresi yeni oluşturulan tüm Azure SQL veri ambarı örnekleri vardır. Dağıtım tamamlandıktan kısa süre içinde windows değiştirebilirsiniz. Herhangi bir bakım önceden bildirimde bulunmadan belirtilen bakım penceresi dışında yer alacak.

Veri ambarınıza uygulanan bakım zamanlaması görüntülemek için aşağıdaki adımları tamamlayın:

1.  [Azure Portal](https://portal.azure.com/) oturum açın.
2.  Görüntülemek istediğiniz veri ambarı'nı seçin. 
3.  Seçili veri ambarı genel bakış dikey penceresinde açılır. Veri ambarı'na uygulanan bakım zamanlaması altında görünen **bakım zamanlaması (Önizleme)**.

![Genel Bakış dikey penceresi](media/sql-data-warehouse-maintenance-scheduling/clear-overview-blade.PNG)

## <a name="next-steps"></a>Sonraki adımlar
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-usage) oluşturma, görüntüleme ve Azure İzleyici'yi kullanarak Uyarıları yönetme hakkında.
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) günlük uyarısı kuralları için Web kancası eylemleri hakkında.
- [Daha fazla bilgi edinin](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-action-groups) oluşturma ve Eylem grupları yönetme.
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/service-health/service-health-overview) Azure hizmet durumu hakkında.


