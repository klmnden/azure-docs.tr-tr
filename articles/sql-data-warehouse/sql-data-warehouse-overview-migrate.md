---
title: Çözümünüzü SQL veri ambarına geçirme | Microsoft Docs
description: Azure SQL Data Warehouse platformuna çözümünüzü getiren Geçiş Kılavuzu.
services: sql-data-warehouse
author: jrowlandjones
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: jrj
ms.reviewer: igorstan
ms.openlocfilehash: 5a609fb2da1f9dba1247358f64b284fc3e3ef5bc
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="migrate-your-solution-to-azure-sql-data-warehouse"></a>Çözümünüzü Azure SQL veri ambarına geçirme
Azure SQL Data Warehouse için varolan bir veritabanı çözümü geçirme içinde ne katılan bakın. 

## <a name="profile-your-workload"></a>İş yükünüzün profil
Bazı SQL Data Warehouse, iş yükü için doğru çözümdür olmasını istediğiniz geçirmeden önce. SQL veri ambarı üzerinde büyük veri analizi gerçekleştirmek için tasarlanmış dağıtılan bir sistemdir.  SQL veri ambarı'na geçirme çok sabit anlamak için değildir ancak uygulamak için biraz zaman alabilir bazı tasarım değişiklikleri gerektirir. Bir kurumsal sınıf veri ambarı, İşletmenizde avantajları çabalara demektir. Ancak, SQL Data Warehouse gücünü gerekmiyorsa, düşük maliyetli SQL Server veya Azure SQL Database kullanın.

SQL veri ambarı kullanmayı olduğunda:
- Bir veya daha fazla terabayt veri sahip
- Büyük miktarlarda veri analizi çalıştırmayı planladığınız
- İşlem ve depolama ölçeklendirebilmeniz gerekir 
- Bunları gerekmediğinde tarafından duraklatma işlem kaynakları maliyet tasarrufu sağlamak istiyorsunuz.

SQL veri ambarı sahip işlem (gerçekleştirme OLTP) iş yükleri için kullanmayın:
- Yüksek yoğunlukta okuma ve yazma işlemleri
- Singleton çok sayıda seçer
- Yüksek hacimli tek bir satır ekler
- Satır satır işleme tarafından gerekiyor
- Uyumsuz biçimleri (JSON, XML)


## <a name="plan-the-migration"></a>Geçişi planlama

Varolan bir çözümü SQL Data Warehouse'a geçirmeye karar verdikten sonra başlarken önce geçiş planlama önemlidir. 

Planlama bir hedef verilerinizi, tablo şemalarını ve kodunuzu SQL Data Warehouse ile uyumlu olduğundan emin olmaktır. Geçerli sistem ve SQL Data Warehouse arasında çözmek için bazı uyumluluk farklılıklar vardır. Ayrıca, büyük miktarlarda verinin Azure alır zaman geçirme. Dikkatli planlama verilerinizi Azure alma hızlandırır. 

Planlama, başka bir çözümünüzü SQL veri ambarı sağlamak üzere tasarlanmış yüksek sorgu performansı yararlanır emin olmak için tasarım ayarlamalar yapmak için hedeftir. Veri ambarları ölçek tasarlama farklı tasarım desenleri ve bu nedenle geleneksel yaklaşım her zaman en iyi olmayan tanıtır. Geçişten sonra bazı tasarım ayarlamalar yapabilmenize rağmen daha erken bir işlemde değişiklik daha sonra zaman kazandırır.

Başarılı bir geçiş yapmak için tablo şemalarını, kodunuz ve verileriniz geçirmeniz gerekir. Bu geçiş konuları hakkında yönergeler için bkz:

-  [Şemalar geçirme](sql-data-warehouse-migrate-schema.md)
-  [Kodunuzu geçirme](sql-data-warehouse-migrate-code.md)
-  [Verilerinizi geçirme](sql-data-warehouse-migrate-data.md). 

<!--
## Perform the migration


## Deploy the solution


## Validate the migration

-->

## <a name="next-steps"></a>Sonraki adımlar
CAT (Müşteri danışma ekibi) blog yayımlama bazı harika SQL Data Warehouse yönergeler de vardır.  Kendi makale bakalım [uygulamada Azure SQL Data Warehouse için geçiş veri] [ Migrating data to Azure SQL Data Warehouse in practice] geçiş hakkında ek yönergeler için.

<!--Image references-->

<!--Article references-->

<!--MSDN references-->

<!--Other Web references-->
[Migrating data to Azure SQL Data Warehouse in practice]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
