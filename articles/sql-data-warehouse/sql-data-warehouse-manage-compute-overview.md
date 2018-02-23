---
title: "Azure SQL Data Warehouse işlem kaynak yönetme | Microsoft Docs"
description: "Azure SQL veri ambarı özellikleri performans genişletme hakkında bilgi edinin. Dwu ya da daha düşük maliyetleri veri ambarı duraklatarak ayarlayarak ölçeğini."
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
ms.date: 02/20/2018
ms.author: elbutter
ms.openlocfilehash: 7e6ae6e59b53dd79dab5e2504cf7a43a30e55353
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="manage-compute-in-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse işlem yönetme
Azure SQL Data Warehouse işlem kaynakları yönetme hakkında bilgi edinin. Veri ambarı duraklatarak maliyetlerine veya performans taleplerini karşılamak üzere veri ambarına ölçeklendirin. 

## <a name="what-is-compute-management"></a>İşlem yönetimi nedir?
SQL Data Warehouse mimarisi, depolama ve işlem, her bağımsız olarak ölçeklendirebilirsiniz izin vererek ayırır. Sonuç olarak, veri depolamasını bağımsız performans taleplerini karşılamak üzere işlem ölçeklendirebilirsiniz. Ayrıca, duraklatma ve sürdürme işlem kaynakları. Bu mimarinin doğal bir sonuç [fatura](https://azure.microsoft.com/pricing/details/sql-data-warehouse/) işlem ve depolama için ayrıdır. Veri ambarınız biraz kullanmanız gerekmez, duraklatarak işlem maliyetleri kaydedebilirsiniz işlem. 

## <a name="scaling-compute"></a>İşlem ölçeklendirme
Ölçeği genişletme veya geri işlem ayarlayarak ölçeklendirme [veri ambarı birimlerini](what-is-a-data-warehouse-unit-dwu-cdwu.md) veri ambarınız için ayarlama. Daha fazla veri ambarı birimi ekledikçe yükleme ve sorgu performansı doğrusal olarak artırabilirsiniz. SQL veri ambarı sunar [hizmet düzeyleri](performance-tiers.md#service-levels) veri ölçeklendirmek yükleyen performansı belirgin bir değişiklik olun warehouse birimleri out veya yedekleme. 

Genişleme adımlar için bkz: [Azure portal](quickstart-scale-compute-portal.md), [PowerShell](quickstart-scale-compute-powershell.md), veya [T-SQL](quickstart-scale-compute-tsql.md) quickstarts. Genişletme işlemlerini de gerçekleştirebilirsiniz bir [REST API](sql-data-warehouse-manage-compute-rest-api.md#scale-compute).

Bir ölçeklendirme işlemi gerçekleştirmek için SQL Data Warehouse önce tüm gelen sorguları sonlandırır ve tutarlı bir duruma emin olmak için işlemler geri alınır. İşlem geri alma işlemi tamamlandıktan sonra ölçeklendirme yalnızca oluşur. Sistem işlem düğümleri depolama katmanından ayırır bir ölçeklendirme işlemi için işlem düğümleri ekler ve işlem katmana depolama katmanı reattaches. Her veri ambarı, işlem düğümlerine dağılımla 60 dağıtımları olarak depolanır. Daha fazla işlem düğümü ekleme, daha fazla işlem gücüne ekler. İşlem düğümleri sayısı arttıkça daha fazla işlem gücü sorgularınızı sağlama dağıtımları her işlem düğümü sayısını azaltır. Benzer şekilde, azaltma data warehouse birimleri, sorguları işlem kaynaklarını azaltır, işlem düğümleri sayısını azaltır.

Aşağıdaki tabloda işlem düğümü değişiklikleri data warehouse birimleri olarak başına dağıtımların sayısı da nasıl değiştiğini gösterir.  DWU6000 60 işlem düğümleri sağlar ve DWU100 daha yüksek kadar sorgu performansı elde eder. 

| Data warehouse birimleri  | \# İşlem düğümleri | \# Düğüm başına dağıtımları |
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


## <a name="finding-the-right-size-of-data-warehouse-units"></a>Veri ambarı birimi doğru boyutta bulma

Genişletme, özellikle büyük veri ambarı birimleri için performans yararlarını görmek için en az bir 1 TB veri kümesi kullanmak istediğiniz. Veri ambarınıza yönelik en iyi veri ambarı birimi bulmak için yukarı ve aşağı doğru ölçeklendirme deneyin. Verilerinizi yükledikten sonra farklı sayıda veri ambarı birimleri birkaç sorgu çalıştırın. Ölçeklendirme hızla gerçekleştiği bir saat veya daha az çeşitli performans düzeylerini deneyebilirsiniz. 

Veri en iyi sayısını bulmak için öneriler birimleri ambarı:

- Bir veri ambarında için geliştirme, veri ambarı birimi daha küçük bir sayı seçerek başlayın.  İyi bir başlangıç noktası DW400 veya DW200 ' dir.
- Seçilen veri ambarı birim sayısı, gözlemlemek performans karşılaştırıldığında Gözlemleme, uygulama performansını izleyin.
- Doğrusal ölçek varsayar ve ne kadar artırma veya azaltma data warehouse birimleri gerektiğini belirlemek. 
- İş gereksinimleriniz için en iyi performansı düzeyi ulaşana kadar ayarlamaları devam edin.

## <a name="when-to-scale-out"></a>Zaman ölçeği genişletme
Data warehouse birimleri ölçeklendirme bu yönlerinin performansını etkiler:

- Doğrusal olarak taramaları, toplamalar ve CTAS deyimleri için sistem performansını artırır.
- Verileri yüklemek için okuyucuları ve yazıcıları sayısını artırır.
- Eş zamanlı sorgular ve eşzamanlılık yuvaları maksimum sayısı.

Zaman verileri ölçeklemek önerileri birimleri ambarı:

- Çok miktarda verinin yüklendiği veya dönüştürüldüğü işlemi gerçekleştirmeden önce verileri daha hızlı bir şekilde kullanılabilmesi için ölçeği genişletme.
- Yoğun iş saatlerinde sayıda eş zamanlı sorguları uyum sağlayacak şekilde genişletme. 

## <a name="what-if-scaling-out-does-not-improve-performance"></a>Ne ölçeğini performansını iyileştirmez?

Data warehouse birimleri paralellik artırma ekleniyor. İş işlem düğümleri arasında eşit olarak bölünmüş ise, ek paralellik sorgu performansını artırır. Ölçek genişletme performansınızı değiştirme değil, bunun nedeni bazı nedenler vardır. Verilerinizi dağıtımlar arasında eğri veya sorguları büyük miktarda veri taşıma Tanıtımı. Sorgu performans sorunları araştırmak için bkz: [performans sorunlarını giderme](sql-data-warehouse-troubleshoot.md#performance). 

## <a name="pausing-and-resuming-compute"></a>İşlem duraklatma ve sürdürme
Duraklatma işlem işlem düğümleri ayırmak depolama katmanı neden olur. İşlem kaynaklarını hesabınızdan yayınlanır. İşlem duraklatıldığında işlem için ücretlendirilirsiniz değil. İşlem sürdürme işlem düğümlerine depolama reattaches ve işlem için ücret sürdürür. Veri ambarı geldiğinizde:

* İşlem ve bellek kaynakları veri merkezindeki kullanılabilir kaynak havuzu döndürülürsünüz
* Veri ambarı birim maliyetlerini duraklatma süresi için sıfırdır.
* Veri depolama etkilenmez ve verileriniz olduğu gibi kalır. 
* SQL veri ambarı çalışan ya da sıraya alınan tüm işlemleri iptal eder.

Ne zaman bir veri ambarı Sürdür:

* SQL veri ambarı ayarlama, veri ambarı birimleri için işlem ve bellek kaynakları alır.
* İşlem, veri ambarı birimlerini Sürdür ücretlerini.
* Verilerinizi kullanılabilir hale gelir.
* Veri ambarı çevrimiçi olduktan sonra iş yükü sorgularınızı yeniden başlatmanız gerekir.

Her zaman veri ambarınız erişilebilir istiyorsanız, duraklatma yerine aşağıya doğru en küçük boyuta ölçeklendirme göz önünde bulundurun. 

İçin Duraklat ve adımları sürdürmek, bkz: [Azure portal](pause-and-resume-compute-portal.md), veya [PowerShell](pause-and-resume-compute-powershell.md) quickstarts. Aynı zamanda [REST API duraklatmak](sql-data-warehouse-manage-compute-rest-api.md#pause-compute) veya [REST API sürdürmek](sql-data-warehouse-manage-compute-rest-api.md#resume-compute).

## <a name="drain-transactions-before-pausing-or-scaling"></a>Duraklatma veya ölçeklendirme öncesinde işlemleri boşaltın
Bir duraklama veya ölçek işlemini başlatmadan önce tamamlamak var olan işlemler izin vererek öneririz.

SQL Veri Ambarınız için duraklatma veya ölçeklendirme isteğinde bulunduğunuzda, arka planda sorgularınız iptal edilir.  Basit bir SELECT sorgusunu hızlıca ve örnek duraklatma veya ölçeklendirme süresini neredeyse hiç etkilemeden iptal edebilirsiniz.  Ancak, verilerinizi veya verilerinizin yapısını değiştiren işlem sorguları o kadar hızlı durdurulamayabilir.  **Bir işlem sorgusunun tamamlanması veya yaptığı değişiklikleri geri alması gerekir.**  Bir işlem sorgusunun tamamladığı işi geri almak, sorgunun değişiklik yapmak için harcadığı süre kadar, hatta bazen daha fazla zaman alabilir.  Örneğin, bir saattir çalışan ve satır silen bir sorguyu iptal etmeniz halinde sistemin silinmiş olan satırları geri eklemesi bir saat sürebilir.  Duraklatma veya ölçeklendirme isteklerini işlemler devam ederken çalıştırmanız halinde, devam etmek için geri alma işleminin tamamlanmasını bekleyeceğinden ilgili duraklatma veya ölçeklendirme işleminin tamamlanması uzun sürebilir.

Ayrıca bkz. [işlemleri anlama](sql-data-warehouse-develop-transactions.md)ve [işlemleri en iyi duruma getirme][işlemleri en iyi duruma getirme](sql-data-warehouse-develop-best-practices-transactions.md).

## <a name="automating-compute-management"></a>İşlem yönetimi otomatikleştirme
İşlem yönetimi işlemleri otomatikleştirmek için bkz: [Azure işlevleri ile Yönet işlem](manage-compute-with-azure-functions.md).

Her genişleme, duraklatma ve sürdürme işlemleri tamamlanması birkaç dakika sürebilir. Duraklatma, ölçekleme veya otomatik olarak devam ettirme, emin olmak için mantığı uygulamak öneririz, başka bir eylem işlemine devam etmeden önce belirli işlemleri tamamladınız. Çeşitli uç noktaları aracılığıyla veri ambarı durumu denetleniyor doğru Otomasyon böyle işlemlerinin uygulamaya izin verir. 

Veri ambarı durumu denetlemek için bkz: [PowerShell](quickstart-scale-compute-powershell.md#check-database-state) veya [T-SQL](quickstart-scale-compute-tsql.md#check-database-state) hızlı başlangıç. Veri ambarı durumunu kontrol edebilirsiniz bir [REST API](sql-data-warehouse-manage-compute-rest-api.md#check-database-state).


## <a name="permissions"></a>İzinler

Veri ambarı ölçeklendirme açıklanan izinleri gerektirir [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse.md).  Duraklatma ve sürdürme gerektiren [SQL DB Katılımcısı](../active-directory/role-based-access-built-in-roles.md#sql-db-contributor) izni, özellikle Microsoft.Sql/servers/databases/action.


## <a name="next-steps"></a>Sonraki adımlar
İşlem kaynaklarını yönetme bir diğer unsuru farklı işlem kaynaklarını tek tek sorgular için ayırma. Daha fazla bilgi için bkz: [iş yükü yönetimi için kaynak sınıfları](resource-classes-for-workload-management.md).
