---
title: Azure SQL veri ambarı işlem kaynağı yönetme | Microsoft Docs
description: Performans ölçeklendirme, Azure SQL veri ambarı özellikleri hakkında bilgi edinin. Dwu'lar ya da daha düşük maliyetler veri ambarını duraklatmak tarafından ayarlayarak ölçeği.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 47be738a4e5dcec144d482c28e39cbe950bba3e7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60748943"
---
# <a name="manage-compute-in-azure-sql-data-warehouse"></a>Azure SQL veri ambarı'nda işlem yönetme
Azure SQL veri ambarı işlem kaynaklarını yönetme hakkında bilgi edinin. Veri ambarını duraklatmak göre daha düşük maliyetler veya veri ambarı performans taleplerine göre ölçeklendirin. 

## <a name="what-is-compute-management"></a>İşlem yönetimi nedir?
SQL veri ambarı mimarisi, depolama ve işlem, her birini ayrı ayrı ölçeklendirilmesine izin vererek ayırır. Sonuç olarak, performans taleplerine göre veri depolamadan bağımsız işlem ölçeklendirebilirsiniz. Ayrıca, duraklatmak ve işlem kaynaklarını sürdürme. Bu mimarinin doğal bir sonuç [fatura](https://azure.microsoft.com/pricing/details/sql-data-warehouse/) işlem ve depolama için ayrıdır. Veri ambarınız bir süredir kullanmanız gerekmez, duraklatarak işlem maliyet tasarrufu yapıp işlem. 

## <a name="scaling-compute"></a>İşlem ölçeklendirme
Ölçeği genişletme veya ayarlayarak arka ölçeklendirileceğini [veri ambarı birimleri](what-is-a-data-warehouse-unit-dwu-cdwu.md) veri ambarınıza yönelik ayarlama. Daha fazla veri ambarı birimleri ekledikçe yükleme ve sorgu performansını doğrusal olarak artırabilirsiniz. 

Genişleme adımlar için bkz: [Azure portalında](quickstart-scale-compute-portal.md), [PowerShell](quickstart-scale-compute-powershell.md), veya [T-SQL](quickstart-scale-compute-tsql.md) hızlı başlangıçları. İle ölçek genişletme işlemleri de yapabilirsiniz bir [REST API](sql-data-warehouse-manage-compute-rest-api.md#scale-compute).

SQL veri ambarı, bir ölçeklendirme işlemi gerçekleştirmek için önce gelen tüm sorgular sonlandırır ve tutarlı bir duruma emin olmak için işlemleri geri alır. İşlemi geri alma tamamlandıktan sonra ölçeklendirme yalnızca gerçekleşir. Sistem işlem düğümlerinin depolama katmanından ayırır bir ölçeklendirme işlemi için işlem düğümlerini ekler ve sonra işlem katmanını depolama katmanına etkilenebilecek. Her veri ambarı, işlem düğümlerine eşit olarak dağıtılmış 60 dağıtım olarak depolanır. Daha fazla işlem düğümleri ekleyerek daha fazla işlem gücü ekler. İşlem düğümleri sayısı arttıkça, sorgularınıza ilişkin daha fazla işlem gücü sağlayan dağıtımları her işlem düğümü sayısını azaltır. Benzer şekilde, veri ambarı birimleri azalan sorgular için işlem kaynaklarını azaltan işlem düğümlerinin sayısını azaltır.

Aşağıdaki tablo, dağıtımları her işlem düğümü değiştikçe veri ambarı birimleri sayısını da nasıl değiştiğini gösterir.  DWU6000 60 işlem düğümleri sağlar ve DWU100 daha çok daha yüksek sorgu performansı elde eder. 

| Veri ambarı birimleri  | \# İşlem düğümleri | \# Düğüm başına dağıtımları |
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


## <a name="finding-the-right-size-of-data-warehouse-units"></a>Veri ambarı birimleri uygun boyutta bulma

Genişletme, özellikle büyük veri ambarı birimleri için performans avantajlarını görmek için en az 1 TB veri kümesi kullanmak istiyorsunuz. Veri ambarınıza yönelik en iyi veri ambarı birimleri sayısını bulmak için ölçeği artırmayı deneyin. Birkaç sorgu yükledikten sonra farklı sayıda veri ambarı birimleri ile çalıştırın. Ölçeklendirme hızla gerçekleştiği için bir saat veya daha az çeşitli performans düzeylerini deneyebilirsiniz. 

En iyi veri sayısını bulmak için öneriler birimleri ambarı:

- Geliştirme, veri ambarı için veri ambarı birimi daha az sayıda seçerek başlayın.  İyi bir başlangıç noktası DW400 ya da DW200 ' dir.
- Seçilen veri ambarı birimleri sayısını, gözlemleyin performans karşılaştırıldığında gözleme, uygulama performansını izleme.
- Doğrusal ölçek varsayar ve ne kadar artırma veya azaltma veri ambarı birimleri gerektiğini belirlemek. 
- Bir en iyi performans düzeyine ilişkin iş gereksinimlerinizin ulaşana kadar ayarlamaları devam edin.

## <a name="when-to-scale-out"></a>Zaman ölçeğini genişletmek için
Veri ambarı birimi ölçeklendirme şu yönlerini performansı etkiler:

- Doğrusal olarak taramaları ve toplamalar CTAS deyimleri sisteminin performansını artırır.
- Verileri yüklemek için okuyucular ve yazıcılar sayısını artırır.
- Eş zamanlı sorguları ve eşzamanlılık yuvaları maksimum sayısı.

Öneriler ne zaman veri çıkışı ölçeklendirme birimleri ambarı:

- Yoğun veri yüklendiği veya dönüştürüldüğü işlemi gerçekleştirmeden önce verileri daha hızlı bir şekilde kullanılabilmesi için ölçeği genişletme.
- En yüksek iş saatlerinde daha fazla sayıda eş zamanlı sorguları uyum sağlamak için ölçeği genişletme. 

## <a name="what-if-scaling-out-does-not-improve-performance"></a>Peki ölçek genişletme performansını iyileştirmez?

Veri ambarı birimleri paralelliğini artırmak ekleniyor. İş işlem düğümleri arasında eşit olarak bölünmüş ise, ek paralellik sorgu performansını artırır. Ölçeği genişletme performansınızı değişmeyen ise bunun olmasının bazı nedenler vardır. Verilerinizi dağıtımlar arasında dengesiz ya da sorguları büyük miktarda veri taşıma Tanıtımı. Sorgu performans sorunlarını araştırmak için bkz: [performans sorunlarını giderme](sql-data-warehouse-troubleshoot.md#performance). 

## <a name="pausing-and-resuming-compute"></a>Duraklatma ve sürdürme
Duraklatma işlem, depolama katmanı, işlem düğümlerinden ayırmak neden olur. İşlem kaynaklarını hesabınızdan serbest bırakılır. İşlem duraklatıldığında işlem için ücretlendirilmez. Sürdürme işlem düğümlerine depolama etkilenebilecek ve için işlem ücretleri devam eder. Veri ambarı geldiğinizde:

* İşlem ve bellek kaynakları, veri merkezinde kullanılabilir kaynakları havuzuna döndürülür
* Veri ambarı birimi duraklatma süresi için sıfır ücretlerdir.
* Veri depolama etkilenmez ve verileriniz olduğu gibi kalır. 
* SQL veri ambarı, çalışan veya sıraya alınan tüm işlemleri iptal eder.

Ne zaman bir veri ambarı Sürdür:

* SQL veri ambarı, işlem ve bellek kaynakları ayarlama, veri ambarı birimleri için alır.
* İşlem, veri ambarı birimleri sürdürme ücretlendirir.
* Verilerinizi kullanılabilir hale gelir.
* Veri ambarı çevrimiçi olduktan sonra iş yükü sorgularınızı yeniden başlatmanız gerekir.

Her zaman veri Ambarınızı erişilebilir istiyorsanız, duraklatmak yerine en küçük boyut ölçeklendirme göz önünde bulundurun. 

İçin duraklatma ve sürdürme adımları için bkz [Azure portalında](pause-and-resume-compute-portal.md), veya [PowerShell](pause-and-resume-compute-powershell.md) hızlı başlangıçları. Ayrıca [REST API duraklatma](sql-data-warehouse-manage-compute-rest-api.md#pause-compute) veya [REST API sürdürmek](sql-data-warehouse-manage-compute-rest-api.md#resume-compute).

## <a name="drain-transactions-before-pausing-or-scaling"></a>Duraklatma veya ölçeklendirme öncesinde işlemleri boşaltın
Duraklatma veya ölçeklendirme işlemi başlatmadan önce bitirmek var olan işlemler izin vererek öneririz.

SQL Veri Ambarınız için duraklatma veya ölçeklendirme isteğinde bulunduğunuzda, arka planda sorgularınız iptal edilir.  Basit bir SELECT sorgusunu hızlıca ve örnek duraklatma veya ölçeklendirme süresini neredeyse hiç etkilemeden iptal edebilirsiniz.  Ancak, verilerinizi veya verilerinizin yapısını değiştiren işlem sorguları o kadar hızlı durdurulamayabilir.  **Bir işlem sorgusunun tamamlanması veya yaptığı değişiklikleri geri alması gerekir.**  Bir işlem sorgusunun tamamladığı işi geri almak, sorgunun değişiklik yapmak için harcadığı süre kadar, hatta bazen daha fazla zaman alabilir.  Örneğin, bir saattir çalışan ve satır silen bir sorguyu iptal etmeniz halinde sistemin silinmiş olan satırları geri eklemesi bir saat sürebilir.  Duraklatma veya ölçeklendirme isteklerini işlemler devam ederken çalıştırmanız halinde, devam etmek için geri alma işleminin tamamlanmasını bekleyeceğinden ilgili duraklatma veya ölçeklendirme işleminin tamamlanması uzun sürebilir.

Ayrıca bkz: [işlemleri anlama](sql-data-warehouse-develop-transactions.md), ve [işlemleri iyileştirme](sql-data-warehouse-develop-best-practices-transactions.md).

## <a name="automating-compute-management"></a>İşlem yönetimi otomatik hale getirme
İşlem yönetimi işlemlerini otomatikleştirme için bkz: [Azure işlevleri ile yönetme işlem](manage-compute-with-azure-functions.md).

Her ölçek genişletme, duraklatma ve sürdürme işlemleri tamamlanması birkaç dakika sürebilir. Duraklatılıyor, ölçeklendirme ya da otomatik olarak yeniden başlatma, emin olmak için mantıksal uygulama öneririz belirli işlemleri başka bir eylem ile devam etmeden önce tamamladınız. Çeşitli uç noktaları aracılığıyla veri ambarı durumu denetimi doğru bu işlemler, Otomasyon uygulamak sağlar. 

Veri ambarı durumunu denetleme için bkz: [PowerShell](quickstart-scale-compute-powershell.md#check-data-warehouse-state) veya [T-SQL](quickstart-scale-compute-tsql.md#check-data-warehouse-state) hızlı başlangıç. Veri ambarı durumu ile de göz atabilirsiniz bir [REST API](sql-data-warehouse-manage-compute-rest-api.md#check-database-state).


## <a name="permissions"></a>İzinler

Veri ambarı ölçeklendirme gerektiren açıklanan izinleri [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse).  Duraklatma ve sürdürme gerektiren [SQL DB Katılımcısı](../role-based-access-control/built-in-roles.md#sql-db-contributor) izni, özellikle Microsoft.Sql/servers/databases/action.


## <a name="next-steps"></a>Sonraki adımlar
İşlem kaynaklarını yönetme bir diğer unsuru dışında her sorgu için farklı işlem kaynaklarını ayırma. Daha fazla bilgi için [iş yükü yönetimi için kaynak sınıfları](resource-classes-for-workload-management.md).
