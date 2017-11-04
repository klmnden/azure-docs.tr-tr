---
title: "Çözümünüzü SQL veri ambarına geçirme | Microsoft Docs"
description: "Azure SQL Data Warehouse platformuna çözümünüzü getiren Geçiş Kılavuzu."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 198365eb-7451-4222-b99c-d1d9ef687f1b
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 06/27/2017
ms.author: joeyong;barbkess
ms.openlocfilehash: 771b9456e66b8a1e41f72340b695b19e2adaf793
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
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
