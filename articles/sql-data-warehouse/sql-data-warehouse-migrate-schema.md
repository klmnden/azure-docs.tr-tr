---
title: "SQL veri ambarına şemanızı geçirme | Microsoft Docs"
description: "Çözümleri geliştirme için Azure SQL veri ambarı'na şemanızı geçirmek için ipuçları."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 538b60c9-a07f-49bf-9ea3-1082ed6699fb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 10/31/2016
ms.author: joeyong;barbkess
ms.openlocfilehash: 07ca2321852e276502187e768177e7e82bdfd080
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="migrate-your-schemas-to-sql-data-warehouse"></a>Şemaları SQL veri ambarına geçirme
SQL Data Warehouse, SQL şemaları geçirmek için yönergeler. 

## <a name="plan-your-schema-migration"></a>Şema geçişinizi planlayın

Geçiş planlıyorsanız görür [tablo genel bakışı] [ table overview] tablo tasarım konuları istatistikleri, dağıtım, bölümlendirme ve dizin oluşturma gibi öğrenmeniz için.  Ayrıca bazı listeler [desteklenmeyen tablo özellikleri] [ unsupported table features] ve bunların geçici çözümleri.

## <a name="use-user-defined-schemas-to-consolidate-databases"></a>Kullanıcı tanımlı şemaları veritabanları birleştirmek için kullanın

Var olan İş yükünüzün büyük olasılıkla birden fazla veritabanı var. Örneğin, bir SQL Server veri ambarı Hazırlama veritabanı, veri ambarı veritabanı ve bazı veri reyonu veritabanı içerebilir. Bu topolojide, her veritabanı ayrı bir iş yükü ayrı güvenlik ilkeleriyle birlikte çalışır.

Bunun aksine, SQL Data Warehouse bir veritabanına içinde tüm veri ambarı iş yükü çalışır. Veritabanı birleştirmeler izin verilmez. Bu nedenle, SQL Data Warehouse bir veritabanı içinde depolanan veri ambarı tarafından kullanılan bütün tablolar bekliyor.

Var olan İş yükünüzün bir veritabanına birleştirmek için kullanıcı tanımlı şemaları kullanmanızı öneririz. Örnekler için bkz: [kullanıcı tanımlı şemaları](sql-data-warehouse-develop-user-defined-schemas.md)

## <a name="use-compatible-data-types"></a>Uyumlu veri türleri kullanın
Veri türlerinizi SQL Data Warehouse ile uyumlu olacak şekilde değiştirin. Desteklenen ve desteklenmeyen veri türlerinin listesi için bkz: [veri türleri][data types]. Bu konu desteklenmeyen türleri için geçici çözümler sunar. Ayrıca, SQL veri ambarı'nda desteklenmez mevcut türleri tanımlayacak bir sorgu sağlar.

## <a name="minimize-row-size"></a>Satır boyutu en aza indir
En iyi performans için tablolar satır uzunluğu en aza indirin. Daha iyi performans için daha kısa satır uzunlukları neden olduğundan, iş en küçük veri türleri, verileriniz için kullanın. 

Tablo satır genişliğini için PolyBase 1 MB sınırı vardır.  PolyBase ile SQL veri ambarına veri yükleme planlıyorsanız, en fazla satır genişliğini 1 MB'tan az olması tablolarınızı güncelleştirin. 

<!--
- For example, this table uses variable length data but the largest possible size of the row is still less than 1 MB. PolyBase will load data into this table.

- This table uses variable length data and the defined row width is less than one MB. When loading rows, PolyBase allocates the full length of the variable-length data. The full length of this row is greater than one MB.  PolyBase will not load data into this table.  

-->

## <a name="specify-the-distribution-option"></a>Dağıtım seçeneğini belirtin
SQL veri ambarı dağıtılmış bir veritabanı sistemidir. Her tablo dağıtılmış veya işlem düğümleri arasında çoğaltılan. Nasıl veri dağıtılacağını belirtmenize olanak sağlar. bir tablo seçeneği yoktur. Çoğaltılan, hepsini, seçimlerdir veya karma dağıtılmış. Her Artıları ve eksileri vardır. Dağıtım seçeneği belirtmezseniz, SQL Data Warehouse hepsini varsayılan olarak kullanın.

- Hepsini varsayılandır. Kullanmak için en basit olan ve sorgu performansı yavaşlatır veri taşıma yükleri mümkün, ancak birleştirmeler hızlı veri ister.
- Bir tablonun her işlem düğümünde kopyasını çoğaltılmış depolar. Çoğaltılmış tablolar kullanıcı olduklarından, veri taşıma birleşimler ve Toplamalar için gerektirmez. Bunlar ek depolama alanı gerektirir ve bu nedenle daha küçük tablolar için en iyi çalışır.
- Dağıtılmış karma satırları karma işlevi ile tüm düğümler arasında dağıtır. Büyük tablolarda yüksek sorgu performansını sağlamak için tasarlanmış beri dağıtılmış karma tablolar SQL Data Warehouse Kalp değildir. Bu seçenek bazı verileri dağıtmak en iyi sütunda seçmek planlama gerektirir. İlk kez en iyi sütun seçmezseniz, ancak, kolayca farklı bir sütun üzerindeki verileri yeniden dağıtabilirsiniz. 

Her tablo için en iyi dağıtım seçeneği için bkz: [dağıtılmış tabloları](sql-data-warehouse-tables-distribute.md).


## <a name="next-steps"></a>Sonraki adımlar
SQL veri ambarı veritabanı şemanızı başarıyla geçirdikten sonra aşağıdaki makaleler birine devam edin:

* [Verilerinizi geçirme][Migrate your data]
* [Kodunuzu geçirme][Migrate your code]

SQL veri ambarı en iyi uygulamalar hakkında daha fazla bilgi için bkz: [en iyi uygulamalar] [ best practices] makalesi.

<!--Image references-->

<!--Article references-->
[Migrate your code]: ./sql-data-warehouse-migrate-code.md
[Migrate your data]: ./sql-data-warehouse-migrate-data.md
[best practices]: ./sql-data-warehouse-best-practices.md
[table overview]: ./sql-data-warehouse-tables-overview.md
[unsupported table features]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[data types]: ./sql-data-warehouse-tables-data-types.md
[unsupported data types]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types

<!--MSDN references-->


<!--Other Web references-->
