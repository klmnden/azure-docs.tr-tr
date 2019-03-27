---
title: Azure SQL veri ambarı sürüm notları | Microsoft Docs
description: Azure SQL veri ambarı için sürüm notları.
services: sql-data-warehouse
author: mlee3gsd
ms.author: anumjs
ms.reviewer: jrasnick
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 02/09/2019
ms.openlocfilehash: e77556ac0d6f64797906c0f3b4181f147b1668e2
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58448417"
---
# <a name="azure-sql-data-warehouse-release-notes-and-documentation-updates"></a>Azure SQL veri ambarı sürüm notları ve belge güncelleştirmeleri

Azure SQL veri ambarı (SQL DW) bulut tabanlı kurumsal veri yüksek düzeyde paralel işleme (hızlıca petabaytlarca veri üzerinde karmaşık sorgular çalıştırmak için MPP) yararlanan ambarıdır. SQL Veri Ambarı'nı büyük veri çözümünün temel bileşenlerinden biri olarak kullanabilirsiniz. Basit PolyBase T-SQL sorguları kullanarak büyük verileri SQL Veri Ambarı'na aktarıp MPP gücünü kullanarak yüksek performanslı analizler gerçekleştirebilirsiniz. Tümleştirme ve analiz işlemleri sırasında veri ambarı, işletmenizin öngörüler için güvenebileceği tek veri sürümü haline gelir.

Azure SQL veri ambarı'nın en yeni sürümünde beklediğiniz geliştirmeleri ve yeni özellikleri hakkında daha fazla bilgi için aşağıdaki bağlantıları tıklatın. Tanımlanan bakım zamanlamanızı sırasında bu hizmet güncelleştirmeleri almasını bekleyebilirsiniz.

- [Mart 2019](./release-notes-10-0-10106-0.md#march-2019)
- [Ocak 2019](./release-notes-10-0-10106-0.md#january-2019)
- [Aralık 2018'e](./release-notes-10-0-10106-0.md#december-2018)
- [Ekim 2018](./release-notes-10-0-10106-0.md#october-2018)
- [Eylül 2018](./release-notes-september-2018.md)
- [Ağustos 2018](./release-notes-august-2018.md)
- [Temmuz 2018](./release-notes-july-2018.md)
- [Haziran 2018](./release-notes-june-2018.md)
- [Mayıs 2018](./release-notes-may-2018.md)

## <a name="checking-the-code-version-that-has-been-applied-to-your-data-warehouse"></a>Veri ambarınıza uygulanan kod sürümü denetleniyor

Hangi sürüm veri ambarınıza uygulanmış olarak onaylanan için. SSMS ile veri ambarınıza bağlanmak ve SQL veri ambarı'nın geçerli sürümü döndürmek için aşağıdaki söz dizimini çalıştırın.

```sql
SELECT @@VERSION AS 'SQL Data Warehouse';
```

Örnek çıktı: ![SQL veri ambarı sürümü](./media/release-notes/sql_data_warehouse_version.png)

Lütfen hangi sürüm, Azure SQL veri ambarı'na uygulanmış onaylamak için belirlenen tarih kullanın. 


## <a name="next-steps"></a>Sonraki adımlar
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-data-warehouse/viewing-maintenance-schedule) bakım zamanlaması görüntüleme hakkında. 
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-data-warehouse/changing-maintenance-schedule) bakım zamanlamasını değiştirme hakkında.
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/monitoring-and-diagnostics/alert-metric) oluşturma, görüntüleme ve Azure İzleyici'yi kullanarak Uyarıları yönetme hakkında.
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) günlük uyarısı kuralları için Web kancası eylemleri hakkında.
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/service-health/service-health-overview) Azure hizmet durumu hakkında