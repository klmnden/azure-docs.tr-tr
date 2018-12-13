---
title: Planlı bakım - Azure SQL veri ambarı | Microsoft Docs
description: Azure SQL veri ambarı için planlı bakım olayları için hazırlamayı öğrenin.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/19/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: c1e8f94a0131ace6354d070e932e414a1897260e
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53166311"
---
# <a name="planning-for-maintenance-on-your-azure-sql-data-warehouse"></a>Azure SQL veri ambarı bakım planlaması

Azure SQL veri ambarınızın planlı bakım olayları hazırlamayı öğrenin.

## <a name="what-is-a-planned-maintenance-event"></a>Planlanan bakım olayı nedir?
Planlanan bakım olayı, süre veri ambarınız için bir bakım işlemi çevrimdışı olması gerektiğinde bir penceredir. Bu hizmet yükseltme, yeni özellikler ve düzeltme ekleri uygulama fırsatlarını olaylardır. 

## <a name="frequency"></a>Sıklık
Ortalama olarak her ay en az bir planlı bir bakım olayı oluşur. 

## <a name="notifications-and-downtime"></a>Bildirimler ve kapalı kalma süresi
Her bir planlanan bakım olayı önce bir bildirim alırsınız. Bir bakım olayı, çalışan tüm sorguları iptal eder ve veri Ambarınızı çevrimdışı duruma getirir. Her veri ambarı için beklenen kapalı kalma süresi yaklaşık 30 dakika olmalıdır. Bakım işlemi tamamlandıktan sonra bildirim bekleyebilirsiniz. 

## <a name="setting-up-alerts"></a>Uyarıları Ayarlama

Kullanmanızı öneririz [Azure İzleyici](../azure-monitor/platform/alerts-activity-log-service-notifications.md) bakım günlüğü uyarıları ayarlamak için planlanan. Uyarılar, işletmenizi etkisini en aza indirmek gerekli bakım için planlamanıza yardımcı olur. 

Bildirimlerini ayarlamak için bu kullanın [günlük uyarı yönergeleri](../azure-monitor/platform/alerts-activity-log-service-notifications.md). 

## <a name="next-steps"></a>Sonraki adımlar
İzleme hakkında daha fazla bilgi için bkz. [iş yükünüzü izleme](sql-data-warehouse-manage-monitor.md).
