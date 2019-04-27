---
title: Çözümünüzü SQL veri ambarı'na geçirme | Microsoft Docs
description: Geçiş Kılavuzu, çözümü için Azure SQL veri ambarı platformu sunmaktadır.
services: sql-data-warehouse
author: WenJason
manager: digimobile
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
origin.date: 04/17/2018
ms.date: 03/25/2019
ms.author: v-jay
ms.reviewer: igorstan
ms.openlocfilehash: 04c921282d3591e7326d326c230bf72e7f5c1812
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60776229"
---
# <a name="migrate-your-solution-to-azure-sql-data-warehouse"></a>Çözümünüzü Azure SQL veri ambarı'na taşıyın
Ne var olan veritabanı çözümünü Azure SQL veri ambarı'na geçiş ile ilgili bakın. 

## <a name="profile-your-workload"></a>İş yükünüz profil
Geçiş yapmadan önce belirli SQL veri ambarı iş yükünüz için doğru çözümdür olmasını istediğiniz. SQL veri ambarı, büyük veriler üzerinde analiz gerçekleştirmek için tasarlanmış bir dağıtılmış sistemidir.  SQL veri ambarı'na geçirme uygulamak için biraz zaman alabilir ancak anlamak çok sabit olmayan bazı tasarım değişiklikleri gerektirir. Bir kurumsal sınıf veri ambarı işletmenizin ihtiyaç duyduğu çabalara avantajları vardır. Ancak, SQL veri ambarı'nın gücünü gerekmiyorsa, SQL Server veya Azure SQL veritabanı'nı kullanmak için daha uygun maliyetli olur.

SQL veri ambarı'nı kullanmayı göz önünde bulundurun,:
- Bir veya daha fazla terabaytlarca veriyi sahip
- Analytics büyük miktarlarda verileri çalıştırmayı planlayın
- İşlem ve depolama ölçeklendirme özelliği gerekir 
- İhtiyacınız olduğunda duraklatma işlem kaynakları tarafından maliyet tasarrufu sağlamak istersiniz.

SQL veri ambarı sahip işlem (gerçekleştirme OLTP) iş yükleri için kullanmayın:
- Yüksek sıklık düzeyi okuma ve yazma işlemleri
- Singleton çok sayıda seçer
- Yüksek hacimli tek bir satır ekler
- Satır satır işleme
- Uyumsuz biçimleri (JSON, XML)

## <a name="plan-the-migration"></a>Geçişi planlama

Varolan bir çözümü SQL Data Warehouse'a geçirmeye karar verdikten sonra başlama önce planlamanız önemlidir. 

Planlama tek amacı, verilerinizi, tablo şemalarını ve kodunuzu SQL veri ambarı ile uyumlu sağlamaktır. Bazı uyumluluk fark, geçerli sistem ile SQL veri ambarı arasında geçici çözüm yoktur. Ayrıca, büyük miktarlarda veri için Azure tarafından gerçekleştirilen işlemlerin zaman geçiriliyor. Dikkatli planlama verilerinizi azure'a alma hızlandırır. 

Planlama, başka bir amaç, çözümünüzü SQL veri ambarı sağlamak için tasarlanmış yüksek sorgu performansı yararlanır emin olmak için tasarım ayarlamalar sağlamaktır. Veri ambarları için ölçek tasarlama farklı tasarım desenleri ve bu nedenle geleneksel yaklaşımlarını her zaman en iyi olmayan tanıtır. Geçişten sonra bazı tasarım ayarlamalar yapabilirsiniz ancak daha erken bir işlemde değişiklik daha sonra zaman kaydedin.

Başarılı bir geçiş gerçekleştirmek için tablo şemalarını, kodunuz ve verileriniz geçirmek gerekir. Bu geçiş konuları hakkında yönergeler için bkz:

-  [Şema geçişi](sql-data-warehouse-migrate-schema.md)
-  [Kodunuzu geçirme](sql-data-warehouse-migrate-code.md)
-  [Verilerinizi geçirme](sql-data-warehouse-migrate-data.md). 

## <a name="next-steps"></a>Sonraki adımlar
CAT (Müşteri danışma ekibi), Web günlükleri yayımlama bazı harika SQL veri ambarı yönergeler de vardır.  Kendi makaleye göz atın [uygulamada Azure SQL veri ambarı Storsimple'a veri] [ Migrating data to Azure SQL Data Warehouse in practice] geçiş hakkında ek yönergeler için.

<!--Image references-->

<!--Article references-->

<!--MSDN references-->

<!--Other Web references-->
[Migrating data to Azure SQL Data Warehouse in practice]: https://blogs.msdn.microsoft.com/sqlcat/20../../migrating-data-to-azure-sql-data-warehouse-in-practice/

<!--Update_Description: update meta properties, wording update-->