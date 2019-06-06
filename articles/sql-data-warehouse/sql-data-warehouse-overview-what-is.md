---
title: Azure SQL Data Warehouse Nedir? | Microsoft Docs
description: Kurumsal sınıf, ilişkisel ve ilişkisel olmayan petabaytlarca işleyebilen veritabanı dağıtılmış. Buna sektörün ilk bulut veri ambarı ile büyütme, küçültme ve saniyeler içinde büyütmenizi özelliği var.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: overview
ms.subservice: design
ms.date: 05/30/2019
ms.author: martinle
ms.reviewer: igorstan
mscustom: sqlfreshmay19
ms.openlocfilehash: a9126e9023091dd8c3df71f2aa2558a01227a8be
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66428021"
---
# <a name="what-is-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse Nedir?

SQL veri ambarı, bir bulut tabanlı kurumsal veri ambarı (hızlıca petabaytlarca veri üzerinde karmaşık sorgular çalıştırmak için yüksek düzeyde paralel işleme (MPP) kullanan EDW) dağıtılır. SQL Veri Ambarı'nı büyük veri çözümünün temel bileşenlerinden biri olarak kullanabilirsiniz. Büyük veri aktarma SQL veri ambarı'na ile basit [PolyBase](/sql/relational-databases/polybase/polybase-guide?view=sql-server-2017&viewFallbackFrom=azure-sqldw-latest) T-SQL sorguları ve yüksek performanslı bir analiz çalıştırılacak MPP gücünü kullanın. Tümleştirme ve analiz işlemleri sırasında veri ambarı, işletmenizin öngörüler için güvenebileceği tek veri sürümü haline gelir.  

## <a name="key-component-of-big-data-solution"></a>Büyük veri çözümünün önemli bileşeni

SQL Veri Ambarı, buluttaki uçtan uca veri çözümünün önemli bileşenlerinden biridir.

![Veri ambarı çözümü](media/sql-data-warehouse-overview-what-is/data-warehouse-solution.png) 

Bulut veri çözümünde veriler farklı veri kaynaklarından büyük veri depolarına alınır. Büyük veri depolarına giren veriler Hadoop, Spark ve makine öğrenimi algoritmaları tarafından hazırlanır ve eğitilir. Veriler karmaşık analiz işlemlerine hazır olduğunda SQL Veri Ambarı, PolyBase kullanarak büyük veri depolarını sorgular. PolyBase, verileri SQL Veri Ambarı'na getirmek için standart T-SQL sorgularını kullanır.
 
SQL Veri Ambarı, verileri sütunlu depolama alanındaki ilişkisel tablolara kaydeder. Bu biçim veri depolama maliyetlerini önemli ölçüde düşürürken sorgu performansını artırır. Veriler SQL Veri Ambarı'nda depolandıktan sonra büyük ölçekli analiz çalışmaları gerçekleştirebilirsiniz. Analizler geleneksel veri sistemlerine kıyasla dakikalar yerine saniyeler, günler yerine saatler içinde tamamlanır. 

Analiz sonuçları dünya çapındaki raporlama veritabanlarına veya uygulamalarına iletilebilir. Ardından iş analistleri işlerle ilgili destekli kararlar almak üzere öngörü sahibi olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- Keşfedin [Azure SQL veri ambarı mimarisi](/azure/sql-data-warehouse/massively-parallel-processing-mpp-architecture)
- Hızlı bir şekilde [SQL Data Warehouse oluşturma][create a SQL Data Warehouse]
- [Örnek verileri yükleme][load sample data].
- Keşfedin [videoları](/azure/sql-data-warehouse/sql-data-warehouse-videos)

Alternatif olarak, aşağıdaki diğer SQL Veri Ambarı Kaynakları’na göz atın.  
* Arama [Bloglar]
* Gönderme bir [özellik istekleri]
* Arama [Müşteri danışma ekibi blogları]
* [Destek bileti oluşturma]
* Arama [MSDN Forumu]
* Arama [Stack Overflow Forumu]


<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Destek bileti oluşturma]: ./sql-data-warehouse-get-started-create-support-ticket.md
[load sample data]: ./sql-data-warehouse-load-sample-databases.md
[create a SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md
[Migration documentation]: ./sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[Integrated tools overview]: ./sql-data-warehouse-overview-integrate.md
[Backup and restore overview]: ./sql-data-warehouse-restore-database-overview.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Bloglar]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Müşteri Danışma Ekibi blogları]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Özellik istekleri]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN forumu]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Stack Overflow forumu]: https://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videos]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SLA for SQL Data Warehouse]: https://azure.microsoft.com/support/legal/sla/sql-data-warehouse/v1_0/
[Volume Licensing]: https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Service Level Agreements]: https://azure.microsoft.com/support/legal/sla/
