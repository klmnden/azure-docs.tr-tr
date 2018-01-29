---
title: "Azure SQL Data warehouse'da (genel bakış) işlem güç yönetimi | Microsoft Docs"
description: "Azure SQL veri ambarı özellikleri genişletme performans. Dwu ayarlayarak ölçeğini veya duraklatıp işlem kaynakları maliyet tasarrufu sağlamak."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: 
ms.assetid: e13a82b0-abfe-429f-ac3c-f2b6789a70c6
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 3/23/2017
ms.author: elbutter
ms.openlocfilehash: d795abe5254d47a72a468b0989e46829a5c5142a
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-overview"></a>Azure SQL Data warehouse'da (genel bakış) işlem güç yönetimi
> [!div class="op_single_selector"]
> * [Genel Bakış](sql-data-warehouse-manage-compute-overview.md)
> * [Portal](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>

SQL Data Warehouse mimarisi, depolama ve işlem, her bağımsız olarak ölçeklendirebilirsiniz izin vererek ayırır. Sonuç olarak, işlem veri miktarını bağımsız performans taleplerini karşılamak üzere genişletilebilir. Bu mimarinin doğal bir sonuç [fatura] [ billed] işlem ve depolama için ayrıdır. 

Bu genel bakış açıklanmaktadır SQL veri ambarı ve Duraklat kullanmaya nasıl çalışır nasıl ölçeğini sürdürme ve ölçeklendirme SQL veri ambarı özelliklerini. 

## <a name="how-compute-management-operations-work-in-sql-data-warehouse"></a>SQL veri ambarı'nda yönetim işlemlerini iş nasıl işlem
Bir denetim düğümü, işlem düğümlerini ve 60 dağıtımları yayılan depolama katmanı, SQL Data Warehouse mimarisi oluşur. 

Normal etkin oturum SQL Data warehouse'da sırasında sisteminizin baş düğüm meta verileri yönetir ve dağıtılmış sorgu iyileştiricisi içerir. Bu baş düğümü altında işlem düğümlerini ve depolama katmanı olan. DWU 400 için sisteminizi bir baş düğüm, dört işlem düğümlerini ve 60 dağıtımlarını oluşan depolama katmanı vardır. 

Bir ölçek uygulanabilecek veya işlemi duraklatmak, sistem önce tüm gelen sorguları sonlandırır ve tutarlı bir duruma emin olmak için işlemler geri alınır. Bu işlem geri alma işlemi tamamlandıktan sonra ölçek işlemleri için ölçeklendirme yalnızca meydana gelir. Bir ölçek büyütme işleminin ek sayısı istenen sistem hükümleri işlem düğümleri ve depolama katmanı için işlem düğümleri yeniden takmanız başlar. Bir ölçek azaltma işlemi için gereksiz düğümleri yayımlanan ve geri kalan işlem düğümleri kendilerini uygun sayıda dağıtımlar için yeniden ekleyin. Duraklatma işlemi için tüm düğümler yayımlanan ve sisteminizin meta veri işlemleri, son sistem kararlı durumda bırakın, çeşitli yapılacaktır işlem.

| DWU  | \#işlem düğümleri | \#Düğüm başına dağıtımları |
| ---- | ------------------ | ---------------------------- |
| 100  | 1                  | 60                           |
| 200  | 2                  | 30                           |
| 300  | 3                  | 20                           |
| 400  | 4                  | 15                           |
| 500  | 5                  | 12                           |
| 600  | 6                  | 10                           |
| 1000 | 10                 | 6                            |
| 1200 | 12                 | 5                            |
| 1500 | 15                 | 4                            |
| 2000 | 20                 | 3                            |
| 3000 | 30                 | 2                            |
| 6000 | 60                 | 1                            |

İşlem yönetmek için üç birincil işlev şunlardır:

1. Duraklat
2. Sürdür
3. Ölçek

Bu işlemlerin her biri tamamlanması birkaç dakika sürebilir. Otomatik olarak ölçeklendirme/duraklatma/sürdürme varsa, belirli işlemleri başka bir eylem işlemine devam etmeden önce tamamlandığından emin olmak için mantığı uygulamak isteyebilirsiniz. 

Çeşitli uç noktaları aracılığıyla veritabanı durumunu denetleme doğru Otomasyon böyle işlemlerinin uygulamaya izin verir. Portal bir işlem ve veritabanlarını tamamlanmasından sonra bildirimi geçerli durumu sağlar ancak programlı durumunu denetlemek için izin vermiyor. 

>  [!NOTE]
>
>  İşlem yönetimi işlevselliği tüm uç noktalar arasında yok.
>
>  

|              | Duraklat/Sürdür | Ölçek | Veritabanı durumunu kontrol edin |
| ------------ | ------------ | ----- | -------------------- |
| Azure portalına | Evet          | Evet   | **Hayır**               |
| PowerShell   | Evet          | Evet   | Evet                  |
| REST API     | Evet          | Evet   | Evet                  |
| T-SQL        | **Hayır**       | Evet   | Evet                  |



<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a>Bilgi işlem

SQL veri ambarı performans [data warehouse birimlerinde (Dwu)] ölçülen [veri ambarı birimlerini (Dwu'lar)], bir işlem kaynakları CPU, bellek ve g/ç bant genişliği gibi abstracted ölçüsüdür. Kendi sistem performansını ölçeklendirme istediği bir kullanıcı çeşitli yollarla gibi portalı, T-SQL ve REST API'leri bunu yapabilirsiniz. 

### <a name="how-do-i-scale-compute"></a>İşlem nasıl ölçeklendirme?
Güç sizin için SQL Data Warehouse DWU ayarını değiştirerek yönetilen işlem. Belirli işlemleri için daha fazla DWU eklemek performansı doğrusal olarak artar.  Yukarı veya aşağı sisteminizi ne zaman ölçeklendirme, performansı belirgin şekilde değişir olun DWU teklifleri sunuyoruz. 

Dwu ayarlamak için tek tek bu yöntemlerden birini kullanabilirsiniz.

* [Azure portal ile güç işlem ölçeklendirme][Scale compute power with Azure portal]
* [İşlem PowerShell ile güç ölçeklendirme][Scale compute power with PowerShell]
* [REST API'leri ile ölçek işlem gücü][Scale compute power with REST APIs]
* [TSQL ile güç işlem ölçeklendirme][Scale compute power with TSQL]

### <a name="how-many-dwus-should-i-use"></a>Kaç tane Dwu kullanmalıyım?

İdeal DWU değerinizin ne olduğunu anlamak için verilerinizi yükledikten sonra ölçeği artırmayı veya azaltmayı ve birkaç sorgu çalıştırmayı deneyin. Ölçeklendirme hızla gerçekleştiği bir saat veya daha az çeşitli performans düzeylerini deneyebilirsiniz. 

> [!Note] 
> SQL veri ambarı büyük miktarda veriyi işlemek için tasarlanmıştır. Özellikle büyük Dwu ölçeklendirmeye yönelik gerçek kapasitesini görmek için 1 TB yaklaşıyor veya büyük bir veri kümesini kullanmak istediğiniz.

İş yükü için en iyi DWU bulmak için öneriler:

1. Geliştirme bir veri ambarı için daha küçük bir DWU performans düzeyi seçerek başlayın.  İyi bir başlangıç noktası DW400 veya DW200 ' dir.
2. Uygulama performansı izleme, karşılaştırıldığında, gözlemlemek performans Dwu sayısı seçili gözlemleyebilirsiniz.
3. Doğrusal ölçek üstlenerek gereksinimleriniz için en iyi performans düzeyine ulaşmak size ne kadar hızlı veya daha yavaş performans olacağını belirleyebilirsiniz.
4. Artırın veya azaltın nasıl çok daha hızlı veya daha yavaş gerçekleştirmek için İş yükünüzün istediğiniz orantılı olarak Dwu sayısı. 
5. İş gereksinimleriniz için en iyi performansı düzeyi ulaşana kadar ayarlamaları devam edin.

> [!NOTE]
>
> İş işlem düğümleri arasında bölünebilir, sorgu performansı ile daha fazla paralelleştirme yalnızca artırır. Ölçeklendirme performansınızı değişmeyen olduğunu fark ederseniz, lütfen bizim performans verilerinizi düz olmayan şekilde dağıtılmış olup olmadığını veya çok miktarda veri taşıma giriş varsa denetlemek için makaleleri ayarlama denetleyin. 

### <a name="when-should-i-scale-dwus"></a>Dwu zaman ölçeklendirmeniz gerekir?
Dwu ölçeklendirme, aşağıdaki önemli senaryolar değiştirir:

1. Doğrusal olarak taramaları, toplamalar ve CTAS deyimleri için sistem performansını değiştirme
2. PolyBase ile birlikte yüklenirken okuyucuları ve yazıcıları sayısını artırmayı
3. Maksimum eş zamanlı sorgular ve eşzamanlılık yuva sayısı

Zaman Dwu ölçeklemek önerileri:

1. Çok miktarda verinin yüklendiği veya dönüştürüldüğü işlemi gerçekleştirmeden önce verilerinizin daha hızlı kullanılabilir olmasını sağlamak Dwu ölçeklendirin.
2. Yoğun iş saatlerinde sayıda eş zamanlı sorguları uyacak şekilde ölçeklendirilir. 

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Duraklatma işlem
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Bir veritabanı duraklatmak için tek tek bu yöntemlerden birini kullanın.

* [Azure portal ile Duraklat işlem][Pause compute with Azure portal]
* [PowerShell ile Duraklat işlem][Pause compute with PowerShell]
* [REST API'leri ile Duraklat işlem][Pause compute with REST APIs]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Resume işlem
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Bir veritabanı sürdürmek için tek tek bu yöntemlerden birini kullanın.

* [Azure portal ile Sürdür işlem][Resume compute with Azure portal]
* [PowerShell ile Sürdür işlem][Resume compute with PowerShell]
* [REST API'leri ile Sürdür işlem][Resume compute with REST APIs]

<a name="check-compute-bk"></a>

## <a name="check-database-state"></a>Veritabanı durumunu kontrol edin 

Bir veritabanı sürdürmek için tek tek bu yöntemlerden birini kullanın.

- [T-SQL ile veritabanı durumunu kontrol edin][Check database state with T-SQL]
- [PowerShell ile veritabanı durumunu kontrol edin][Check database state with PowerShell]
- [REST API'leri ile veritabanı durumunu kontrol edin][Check database state with REST APIs]

## <a name="permissions"></a>İzinler

Veritabanı ölçeklendirme açıklanan izinleri gerektirir [ALTER DATABASE][ALTER DATABASE].  Duraklatma ve sürdürme gerektiren [SQL DB Katılımcısı] [ SQL DB Contributor] izni, özellikle Microsoft.Sql/servers/databases/action.

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Sonraki adımlar
Bazı ek anahtar performans kavramlarını anlamanıza yardımcı olması için aşağıdaki makalelere bakın:

* [İş yükü ve eşzamanlılık Yönetimi][Workload and concurrency management]
* [Tablo Tasarım genel bakış][Table design overview]
* [Tablo dağıtım][Table distribution]
* [Tablo dizin oluşturma][Table indexing]
* [Tablo bölümleme][Table partitioning]
* [Tablo istatistikleri][Table statistics]
* [En iyi uygulamalar][Best practices]

<!--Image reference-->

<!--Article references-->
[billed]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Scale compute power with Azure portal]: ./sql-data-warehouse-manage-compute-portal.md#scale-compute-power
[Scale compute power with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#scale-compute-bk
[Scale compute power with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#scale-compute-bk
[Scale compute power with TSQL]: ./sql-data-warehouse-manage-compute-tsql.md#scale-compute-bk

[capacity limits]: ./sql-data-warehouse-service-capacity-limits.md

[Pause compute with Azure portal]:  ./sql-data-warehouse-manage-compute-portal.md
[Pause compute with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#pause-compute-bk
[Pause compute with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#pause-compute-bk

[Resume compute with Azure portal]:  ./sql-data-warehouse-manage-compute-portal.md
[Resume compute with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#resume-compute-bk
[Resume compute with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#resume-compute-bk

[Check database state with T-SQL]: ./sql-data-warehouse-manage-compute-tsql.md#check-database-state-and-operation-progress
[Check database state with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#check-database-state
[Check database state with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#check-database-state

[Workload and concurrency management]: ./sql-data-warehouse-develop-concurrency.md
[Table design overview]: ./sql-data-warehouse-tables-overview.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Table indexing]: ./sql-data-warehouse-tables-index.md
[Table partitioning]: ./sql-data-warehouse-tables-partition.md
[Table statistics]: ./sql-data-warehouse-tables-statistics.md
[Best practices]: ./sql-data-warehouse-best-practices.md
[development overview]: ./sql-data-warehouse-overview-develop.md

[SQL DB Contributor]: ../active-directory/role-based-access-built-in-roles.md#sql-db-contributor

<!--MSDN references-->
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Azure portal]: http://portal.azure.com/
