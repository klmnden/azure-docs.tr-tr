---
title: Planlama için Azure bakım olayları - Azure SQL veritabanı | Microsoft Docs
description: Azure SQL veritabanı için planlı bakım olayları için hazırlamayı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aamalvea
ms.author: aamalvea
ms.reviewer: carlrab
manager: craigg
ms.date: 01/30/2019
ms.openlocfilehash: 928338a911efae051df7164239dbd19f9317338a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60584616"
---
# <a name="planning-for-azure-maintenance-events-in-azure-sql-database"></a>Azure SQL veritabanı'nda Azure bakım olayları için planlama

Azure SQL veritabanınızı planlı bakım olayları hazırlamayı öğrenin.

## <a name="what-is-a-planned-maintenance-event"></a>Planlanan bakım olayı nedir

Her veritabanı için Azure SQL DB oluşan tek bir çoğaltmasını birincil olduğu veritabanı çoğaltmalarını tutar. AT her zaman birincil çoğaltma çevrimiçi bakım olmalıdır ve en az bir ikincil çoğaltma iyi durumda olması gerekir. Planlanan bakım sırasında veritabanı çekirdek üyeleri çevrimdışı bir tane birincil çoğaltma ve kapalı kalma süresi olmadan istemci emin olmak için en az bir ikincil çoğaltma çevrimiçi yanıt olduğunu amaç ile bir kerede geçer. Birincil çoğaltması çevrimdışı duruma getirilmesi gerektiğinde, bir yeniden yapılandırma/yük devretme işlemini bir ikincil çoğaltma yeni birincil olacağı meydana gelir.  

## <a name="what-to-expect-during-a-planned-maintenance-event"></a>Planlı bir bakım olayı sırasında beklenmesi gerekenler

Yeniden yapılandırmalar/yük devretme işlemleri genellikle 30 saniye içinde tam – ortalama 8 saniyedir. Zaten bağlıysa, uygulamanızın sağlıklı kopyalama yeni birincil çoğaltmaya veritabanınızın yeniden bağlanmanız gerekir. Yeni birincil çoğaltma çevrimiçi hale gelmeden önce veritabanı bir yeniden yapılandırma aşamasında sırasında yeni bir bağlantı girişiminde bulunulursa, hata 40613 (veritabanı kullanılamıyor) alın: "'{Servername}' sunucusundaki '{databasename}' veritabanı şu anda kullanılabilir değil. Lütfen bağlantıyı daha sonra yeniden deneyin. ". Veritabanınız uzun süre çalışan bir sorgu varsa, bu sorgu, bir yeniden yapılandırma sırasında engellenir ve yeniden başlatılması gerekir.

## <a name="retry-logic"></a>Yeniden deneme mantığı

Bir bulut veritabanı hizmeti için bağlanan her istemci üretim uygulama, sağlam bir bağlantı uygulamalıdır [yeniden deneme mantığı](sql-database-connectivity-issues.md#retry-logic-for-transient-errors). Bu, bu gibi durumlarda azaltılmasına yardımcı olur ve genellikle hatalarını saydam son kullanıcıya olmanız gerekir.

## <a name="frequency"></a>Sıklık

Ortalama olarak her ay 1.7 planlı bakım olayları oluşur.

## <a name="resource-health"></a>Kaynak Durumu

SQL veritabanınızda oturum açma hataları yaşıyor, denetleyin [kaynak durumu](../service-health/resource-health-overview.md#getting-started) penceresinde [Azure portalında](https://portal.azure.com) geçerli durumu. Sistem durumu geçmişi Bu bölüm, her bir olay (varsa) kapalı kalma nedenini içerir.


## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [kaynak durumu](sql-database-resource-health.md) SQL veritabanı için
- Yeniden deneme mantığı hakkında daha fazla bilgi için bkz: [yeniden deneme mantığı geçici hatalar için](sql-database-connectivity-issues.md#retry-logic-for-transient-errors)
