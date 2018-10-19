---
title: Azure bakım zamanlamaları (Önizleme) | Microsoft Docs
description: Bakım zamanlaması, müşterilerin yeni özellikler, yükseltmeleri ve düzeltme eklerini alma için Azure SQL veri ambarı hizmeti kullanan gerekli zamanlanmış bakım olayları geçici planı sağlar.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 10/06/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: db5502b796fc345b2e2bf083f864d80bd59e7293
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49427136"
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
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/service-health/service-health-overview) Azure hizmet durumu hakkında.


