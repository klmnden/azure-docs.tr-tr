---
title: Gen2 önbelleğinizi en iyi duruma getirme | Microsoft Docs
description: Azure portalını kullanarak Gen2 önbelleğinizi izlemeyi öğrenin.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.subservice: performance
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 26791aecb2ca57b31358d3385d07230c73c84904
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61474428"
---
# <a name="how-to-monitor-the-gen2-cache"></a>Gen2 önbelleğini izleme
Gen2 depolama mimarisi, en sık Sorgulanmış columnstore segmentleri NVMe SSD tabanlı Gen2 veri ambarları için tasarlanmış bulunan bir önbellekte otomatik olarak katmanlandırır. Sorgularınızın önbelleğinde bulunan segmentleri aldığınızda daha yüksek performans alırlar. Bu makalede, izleme ve iş yükünüz Gen2 önbellek en iyi şekilde yararlanma olup olmadığını belirleyerek yavaş sorgu performansı sorunlarını giderme açıklar.  
## <a name="troubleshoot-using-the-azure-portal"></a>Azure portalını kullanarak sorun giderme
Azure İzleyici, sorgu performansı sorunlarını gidermek için 2. nesil önbellek ölçümlerini görüntülemek için kullanabilirsiniz. İlk Azure portalına gidin ve üzerinde İzleyicisi'ni tıklatın:

![Azure İzleyici](./media/sql-data-warehouse-cache-portal/cache_0.png)

Ölçümleri düğmesini seçip doldurun **abonelik**, **kaynak** **grubu**, **kaynak türü**, ve **kaynak adı** veri ambarınızın.

Gen2 önbelleği sorunlarını giderme için ana ölçümler **önbellek isabet yüzdesi** ve **önbelleği kullanılan yüzde**. Bu iki ölçümleri görüntülemek için Azure ölçüm grafiğini yapılandırın.

![Önbellek ölçümleri](./media/sql-data-warehouse-cache-portal/cache_1.png)


## <a name="cache-hit-and-used-percentage"></a>Önbellek isabet ve kullanılan yüzdesi
Aşağıdaki matris önbellek ölçümlerini değerlerine göre senaryosu açıklanmaktadır:

|                                | **Yüksek Önbellek isabet yüzdesi** | **Düşük önbellek isabet yüzdesi** |
| :----------------------------: | :---------------------------: | :--------------------------: |
| **Yüksek kullanılan önbellek yüzdesi** |          Senaryo 1           |          Senaryo 2          |
| **Düşük kullanılan önbellek yüzdesi**  |          Senaryo 3           |          Senaryo 4          |

**Senaryo 1:** Önbelleğinizi en verimli şekilde kullanıyor. [Sorun giderme](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-manage-monitor) sorgularınızı yavaşlatmasını diğer alanları.

**Senaryo 2:** Geçerli Çalışma Veri kümenizi düşük neden önbelleğe sığamıyorsa önbellek isabet yüzdesi fiziksel okuma nedeniyle. Performans düzeyinizi ölçeklendirmeyi düşünün ve önbelleğini doldurmak için iş yükünüzü yeniden çalıştırın.

**Senaryo 3:** Sorgunuzu önbelleğe ilgisi olmayan nedenlerden dolayı yavaş çalıştığından emin olma olasılığı yüksektir. [Sorun giderme](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-manage-monitor) sorgularınızı yavaşlatmasını diğer alanları. Ayrıca düşünebilirsiniz [örneğinizin ölçeklendirme](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-manage-monitor) maliyet tasarrufu için önbellek boyutunu azaltmak için. 

**Senaryo 4:** Sorgunuzu yavaş neden neden olabilecek bir soğuk önbellek var. Çalışma kümenizin artık, önbelleğe alınması gerektiği gibi sorgunuzu yeniden deneyin. 

**Önemli: Önbellek isabet yüzdesi veya kullanılan önbellek yüzdesini güncelleştirilmiyor iş yükünüzü yeniden çalıştırdıktan sonra çalışma kümenizin zaten bellekte bulunan bağlantısız. Yalnızca kümelenmiş columnstore tablolarını önbelleğe alındığını unutmayın.**

## <a name="next-steps"></a>Sonraki adımlar
Genel sorgu performansını ayarlama ile ilgili daha fazla bilgi için bkz: [izleme sorgu yürütme](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-manage-monitor#monitor-query-execution).


<!--Image references-->

<!--Article references-->
[SQL Data Warehouse best practices]: ./sql-data-warehouse-best-practices.md
[System views]: ./sql-data-warehouse-reference-tsql-system-views.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Investigating queries waiting for resources]: ./sql-data-warehouse-manage-monitor.md#waiting

<!--MSDN references-->
[sys.dm_pdw_dms_workers]: https://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_exec_sessions]: https://msdn.microsoft.com/library/mt203883.aspx
[sys.dm_pdw_request_steps]: https://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: https://msdn.microsoft.com/library/mt203889.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: https://msdn.microsoft.com/library/mt204017.aspx
[DBCC PDW_SHOWSPACEUSED]: https://msdn.microsoft.com/library/mt204028.aspx
[LABEL]: https://msdn.microsoft.com/library/ms190322.aspx
