---
title: Azure SQL veri ambarı sürüm notları | Microsoft Docs
description: Azure SQL veri ambarı için sürüm notları.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 11/11/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: a4737f4308f49547178bb216b90589c596f604c9
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2019
ms.locfileid: "54118447"
---
# <a name="azure-sql-data-warehouse-release-notes"></a>Azure SQL veri ambarı sürüm notları

Azure SQL veri ambarı, bir bulut tabanlı kurumsal veri ambarı (yüksek düzeyde paralel işleme (hızlıca petabaytlarca veri üzerinde karmaşık sorgular çalıştırmak için MPP) yararlanan EDW) dağıtılır. SQL Veri Ambarı'nı büyük veri çözümünün temel bileşenlerinden biri olarak kullanabilirsiniz. Basit PolyBase T-SQL sorguları kullanarak büyük verileri SQL Veri Ambarı'na aktarıp MPP gücünü kullanarak yüksek performanslı analizler gerçekleştirebilirsiniz. Tümleştirme ve analiz işlemleri sırasında veri ambarı, işletmenizin öngörüler için güvenebileceği tek veri sürümü haline gelir.

Azure SQL veri ambarı'nın en yeni sürümünde beklediğiniz geliştirmeleri ve yeni özellikleri hakkında daha fazla bilgi için aşağıdaki bağlantıları tıklatın. Tanımlanan bakım zamanlamanızı sırasında bu hizmet güncelleştirmeleri almasını bekleyebilirsiniz.

- [Aralık 2018'e](./release-notes-december-2018.md)
- [Ekim 2018](./release-notes-october-2018.md)
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
