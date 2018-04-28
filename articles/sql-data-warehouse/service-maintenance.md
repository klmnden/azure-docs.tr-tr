---
title: Planlı bakım - Azure SQL Data Warehouse | Microsoft Docs
description: Azure SQL veri ambarı için planlı bakım etkinliği hazırlama hakkında bilgi edinin.
services: sql-data-warehouse
author: antvgski
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/19/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: a273ad4c256267865d3af324f0ef755a6cb75c5c
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="planning-for-maintenance-on-your-azure-sql-data-warehouse"></a>Azure SQL veri ambarı Bakım planlama

Azure SQL veri ambarı planlı bakım olaylarına hazırlama hakkında bilgi edinin.

## <a name="what-is-a-planned-maintenance-event"></a>Planlanan bakım olayı nedir?
Planlanan bakım olayı veri ambarınız için bir bakım işlemi çevrimdışı olması gerektiği zaman zaman bir penceredir. Bu olaylar, hizmet yükseltme, yeni özellikler veya düzeltme ekleri uygulanmasının fırsatlarını verilmiştir. 

## <a name="frequency"></a>Sıklık
Ortalama, en az bir planlı bakım olayı her ay oluşur. 

## <a name="notifications-and-downtime"></a>Bildirimler ve kapalı kalma süresi
Her planlanan bakım olayı önce bir bildirim alırsınız. Bir bakım olayı çalışan tüm sorguları iptal eder ve veri ambarınız çevrimdışı duruma getirir. Her veri ambarı için beklenen kapalı kalma süresi yaklaşık olarak 30 dakikadır. Bakım tamamlandıktan sonra bir bildirim bekleyebilirsiniz. 

## <a name="setting-up-alerts"></a>Uyarılarını ayarlama

Kullanmanızı öneririz [Azure İzleyici](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md) planlı bakım günlüğü uyarıları ayarlamak için. Uyarılar, iş üzerindeki etkileri en aza indirmek gerekli bir bakım planlamanıza yardımcı olabilir. 

Bildirimleri ayarlamak için bu kullanın [oturum uyarı yönergeleri](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md). 

## <a name="next-steps"></a>Sonraki adımlar
İzleme hakkında daha fazla bilgi için bkz: [, iş yükünü izlemek](sql-data-warehouse-manage-monitor.md).
