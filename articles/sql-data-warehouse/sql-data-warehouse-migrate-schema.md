---
title: SQL veri ambarı'na şemanızın geçişini yapın | Microsoft Docs
description: Şemanızı çözümleri geliştirmek için Azure SQL veri ambarı'na geçirmek için ipuçları.
services: sql-data-warehouse
author: WenJason
manager: digimobile
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
origin.date: 04/17/2018
ms.date: 10/15/2018
ms.author: v-jay
ms.reviewer: igorstan
ms.openlocfilehash: 4139ea776f6947eeacf4620c3676606d6535dd2b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60748161"
---
# <a name="migrate-your-schemas-to-sql-data-warehouse"></a>Şemaları SQL veri ambarı'na geçirme
SQL veri ambarı, SQL şemaları geçişine ilişkin yönergeler. 

## <a name="plan-your-schema-migration"></a>Şema geçişinizi planlayın

Bir geçiş planı görmeniz [tabloya genel bakış] [ table overview] tablo tasarımları gibi istatistikleri, dağıtım, bölümlendirme ve dizin oluşturma ile ilgili bilgi sahibi olma.  Ayrıca bazı listeler [desteklenmeyen Tablo Özellikler] [ unsupported table features] ve bunların geçici çözümleri bulabilirsiniz.

## <a name="use-user-defined-schemas-to-consolidate-databases"></a>Kullanıcı tanımlı şemalar veritabanları birleştirmek için kullanın

Mevcut iş yükünüz, büyük olasılıkla birden fazla veritabanı vardır. Örneğin, bir SQL Server veri ambarı Hazırlama veritabanı, veri ambarı veritabanını ve bazı veri reyonu veritabanı içerebilir. Bu topolojide, her veritabanını ayrı güvenlik ilkeleriyle ayrı bir iş yükü olarak çalışır.

Bunun aksine, SQL veri ambarı, tüm veri ambarı iş yükünün bir veritabanı içinde çalışır. Veritabanı birleştirmeler izin verilmez. Bu nedenle, bir veritabanı içinde depolanacak veri ambarı tarafından kullanılan tüm tabloları SQL veri ambarı bekliyor.

Mevcut iş yükünüz bir veritabanına birleştirmek için kullanıcı tanımlı şemalar kullanmanızı öneririz. Örnekler için bkz [kullanıcı tanımlı şemalar](sql-data-warehouse-develop-user-defined-schemas.md)

## <a name="use-compatible-data-types"></a>Uyumlu veri türleri kullanın
Veri türleri, SQL veri ambarı ile uyumlu olacak şekilde değiştirin. Desteklenen ve desteklenmeyen veri türleri listesi için bkz. [veri türleri][data types]. Bu konu, desteklenmeyen türler için geçici çözümler sağlar. Ayrıca SQL veri ambarı'nda desteklenmez mevcut türlerini tanımlamak üzere bir sorgu sağlar.

## <a name="minimize-row-size"></a>Satır boyutu en aza indir
En iyi performans için tablolarınızı satır uzunluğu en aza indirin. Daha iyi performans için daha kısa satır uzunlukları neden olduğundan, verileriniz için çalışan en küçük veri türlerini kullanın. 

Tablo için satır genişlik, PolyBase 1 MB sınırı vardır.  PolyBase ile SQL veri ambarı'na veri yükleme planlıyorsanız, en büyük satır genişliğini 1 MB'tan az olması tablolarınızı güncelleştirin. 

## <a name="specify-the-distribution-option"></a>Dağıtım seçeneğini belirtin
SQL veri ambarı dağıtılan bir veritabanı sistemidir. Her tablo dağıtılmış veya işlem düğümleri arasında çoğaltılır. Verilerin nasıl dağıtılacağını belirtmenize olanak sağlar. bir tablo seçenek mevcuttur. Seçimleri çoğaltılan, hepsini bir kez deneme, veya karma dağıtılmış. Her avantajları ve dezavantajları vardır. SQL veri ambarı dağıtım seçeneği belirtmezseniz hepsini bir kez deneme varsayılan olarak kullanır.

- Hepsini bir kez deneme varsayılandır. Kullanmak için en basit olduğundan ve veri taşıma, bir sorgu performansı yavaşlattığını olası, ancak birleştirmeler hızlı veri yüklerini gerektirir.
- Bir tablonun her işlem düğümünde kopyasını çoğaltılmış depolar. Bunlar veri taşıma birleşimler ve Toplamalar için gerektirmediğinden çoğaltılmış tablolar performansa sahiptir. Bunlar ek depolama alanı gerektirir ve bu nedenle daha küçük tablolar için en iyi çalışır.
- Karma dağıtılmış satırları bir karma işlevi ile tüm düğümler arasında dağıtır. Karma dağıtılmış tablo SQL veri ambarı'nın temelini olduğundan yüksek sorgu performansı büyük tablolarda sağlamak için tasarlanmıştır. Bu seçenek, bazı verileri dağıtmak en iyi sütunu seçmek planlama gerektirir. İlk kez en iyi sütun seçmezseniz, ancak, kolayca veriler üzerinde farklı bir sütun yeniden dağıtabilirsiniz. 

Her tablo için en iyi dağıtım seçeneği için bkz: [dağıtılmış tablolar](sql-data-warehouse-tables-distribute.md).

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelerden birine, veritabanı şemasını SQL veri ambarı'na başarıyla geçirdikten sonra devam edin:

* [Verilerinizi geçirme][Migrate your data]
* [Kodunuzu geçirme][Migrate your code]

SQL veri ambarı en iyi uygulamalar hakkında daha fazla bilgi için bkz. [en iyi uygulamalar] [ best practices] makalesi.

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

<!--Update_Description: update meta properties, add new content about Migrate schemas to SQL Data Warehouse -->