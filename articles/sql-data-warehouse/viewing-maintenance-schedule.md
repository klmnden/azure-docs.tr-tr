---
title: Azure bakım zamanlamaları (Önizleme) | Microsoft Docs
description: Bakım zamanlaması, müşterilerin yeni özellikler, yükseltmeleri ve düzeltme eklerini almak için Azure SQL veri ambarı hizmeti kullanan gerekli zamanlanmış bakım olayları geçici planı olanak tanır.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 10/06/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: 6fbf914c8035344d33e8d2739fb9d92d5757c3d1
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47228428"
---
# <a name="viewing-a-maintenance-schedule"></a>Bakım zamanlaması görüntüleme 

## <a name="portal"></a>Portal

Dağıtım tamamlandıktan kısa süre içinde tüm yeni Azure SQL veri ambarı örneği, dağıtım sırasında uygulanan bir 8 ik sistem tarafından tanımlanan birincil ve ikincil bakım penceresi olacaktır oluşturulan varsayılan olarak bu düzenlenebilir. Herhangi bir bakım önceden bildirimde bulunmadan belirtilen bakım süreleri dışında gerçekleşir.
Veri ambarınıza portalında uygulanan bakım zamanlaması görüntülemek için aşağıdaki adımları tamamlayın.

Veri ambarınıza portalında uygulanan bakım zamanlaması görüntülemek için aşağıdaki adımları tamamlayın.
1.  [Azure Portal](https://portal.azure.com/) oturum açın.
2.  Görüntülemek istediğiniz veri ambarı'nı seçin. 
3.  Seçili Azure SQL veri ambarı genel bakış dikey penceresinde açılır. Seçili veri ambarı'na uygulanan bakım zamanlaması, bakım zamanlaması (Önizleme) gösterilir.

![Genel Bakış dikey penceresi](media/sql-data-warehouse-maintenance-scheduling/clear-overview-blade.PNG)

## <a name="next-steps"></a>Sonraki adımlar
[Daha fazla bilgi edinin](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-usage) oluşturma, görüntüleme ve Azure İzleyicisi'ni kullanarak Uyarıları yönetme hakkında.
[Daha fazla bilgi edinin](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) günlük uyarısı kuralları için Web kancası eylemleri hakkında.
[Daha fazla bilgi edinin](https://docs.microsoft.com/azure/service-health/service-health-overview) Azure hizmet durumu hakkında


