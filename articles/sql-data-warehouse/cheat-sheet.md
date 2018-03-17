---
title: "Azure SQL Data Warehouse için kopya sayfası | Microsoft Docs"
description: "Bağlantılar ve Azure SQL Data Warehouse çözümlerinizi hızlı bir şekilde oluşturmak için en iyi yöntemleri öğrenin."
services: sql-data-warehouse
documentationcenter: NA
author: acomet
manager: jhubbard
editor: 
ms.assetid: 51f1e444-9ef7-4e30-9a88-598946c45196
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 02/20/2018
ms.author: acomet
ms.openlocfilehash: c67d56ff63f70baa052be17c119d943c558d398f
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="cheat-sheet-for-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse için kopya sayfası
Bu kopya sayfası yararlı ipuçları ve Azure SQL Data Warehouse çözümlerinizi oluşturmaya yönelik en iyi yöntemler sağlar. Başlamadan önce her adım ayrıntılı hakkında daha fazla okuyarak bilgi [Azure SQL veri ambarı iş yükü desenleri ve koruma desenleri](https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/azure-sql-data-warehouse-workload-patterns-and-anti-patterns), SQL Data Warehouse nedir ve ne değil açıklar.

Aşağıdaki grafikte bir veri ambarı tasarlama işlemi gösterilmektedir:

![Taslak]

## <a name="queries-and-operations-across-tables"></a>Sorgular ve tablolar arasında işlemler

Birincil işlemleri ve veri ambarınız çalışacak sorgular önceden biliyorsanız, bu işlemler için veri ambarı mimarisi öncelik sırasına koyabilirsiniz. Bu sorgular ve işlemler şunlar olabilir:
* Bir veya iki olgu tabloları birleşik tabloyu filtreleme ve ardından sonuçları bir veri reyonu ekleme boyut tabloları ile birleştirme.
* Büyük veya küçük güncelleştirmeleri olgu satış yapma.
* Yalnızca veri tablolarınız için ekleme.

Tür işlemler önceden bilerek tablolarınızı tasarım en iyi hale getirmenize yardımcı olur.

## <a name="data-migration"></a>Veri geçişi

İlk olarak, verilerinizi yük [Azure Data Lake Store](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-data-lake-store) veya Azure Blob Depolama. Ardından, verilerinizi hazırlama tablosunda SQL Data Warehouse'a veri yüklemek için Polybase'i kullanın. Aşağıdaki yapılandırmayı kullanın:

| Tasarlayın | Öneri |
|:--- |:--- |
| Dağıtım | Hepsini Bir Kez Deneme |
| Dizinleme | Yığın |
| Bölümleme | Hiçbiri |
| Kaynak Sınıfı | largerc veya xlargerc |

Daha fazla bilgi edinmek [veri geçişi], [veri yükleme]ve [ayıklama, yükleme ve dönüştürme (ELT) işlem](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/design-elt-data-loading). 

## <a name="distributed-or-replicated-tables"></a>Dağıtılmış veya çoğaltılmış tablolar

Tablo özelliklerini bağlı olarak aşağıdaki stratejilerden kullanın:

| Tür | Harika sığdırmak için...| İzleme If...|
|:--- |:--- |:--- |
| Çoğaltılan | Bir yıldız Şeması 2 GB'tan daha az sıkıştırma (~ 5 x sıkıştırma) sonra depolama ile • küçük boyut tabloları |• Birçok işlemleri yazma olan tablosundaki (örneğin, Ekle, upsert, silme, güncelleştirme)<br></br>• Veri ambarı birimi (DWU) sık sağlama değiştirme<br></br>• Tablonuz yalnızca 2-3 sütunları kullanın çok sayıda sütun var.<br></br>• Bir çoğaltılmış tablo dizin |
| Hepsini bir kez (varsayılan) | • Geçici/hazırlama tablosu<br></br> • Birleştirmeyi anahtar veya iyi bir aday belirgin |• Performans nedeniyle veri taşıma yavaş |
| Karma | • Olgu tabloları<br></br>• Büyük boyut tabloları |• Dağıtım anahtar güncelleştirilemez |

**İpuçları:**
* Hepsini bir kez ile başlatılır, ancak yüksek düzeyde paralel bir mimaridir yararlanmak için karma Dağıtım stratejisi aspire.
* Ortak karma anahtarlar aynı veri biçimini olduğundan emin olun.
* Varchar biçimine dağıtın yok.
* Boyut tabloları sık birleştirme işlemleri ile bir olgu tablosu için ortak bir karma anahtar ile karma dağıtılmış olabilir.
* Use *[sys.dm_pdw_nodes_db_partition_stats]* to analyze any skewness in the data.
* Kullanım  *[sys.dm_pdw_request_steps]*  verileri çözümlemek için sorguları, arkasında hareketleri yayın zaman izlemek ve karışık işlemleri sürer. Bu dağıtım stratejinizi gözden geçirmek yararlıdır.

Daha fazla bilgi edinmek [çoğaltılmış tablolar] ve [dağıtılmış tabloları].

## <a name="index-your-table"></a>Tablonuz dizin

Dizin oluşturma, tablolar hızla okumak için yararlıdır. Gereksinimlerinize göre kullanabileceğiniz teknolojileri benzersiz bir kümesi vardır:

| Tür | Harika sığdırmak için... | İzleme If...|
|:--- |:--- |:--- |
| Yığın | • Hazırlama/geçici tablo<br></br>• Küçük küçük aramaları tablolarla |• Tam tablonun tüm arama tarar |
| Kümelenmiş dizini | • En fazla 100 milyon satır tablolarla<br></br>• Büyük tabloları (100 milyondan fazla satır) sütunlarla yoğun olarak kullanılan yalnızca 1-2 |• Bir çoğaltılmış tablo üzerinde kullanılan<br></br>Birden fazla birleştirme ve Group By işlemleri içeren karmaşık sorgular olan •<br></br>Yaptığınız güncelleştirmeler dizinli sütunlarda •: bellek alır |
| Kümelenmiş columnstore dizini (CCI) (varsayılan) | • Büyük tabloları (100 milyondan fazla satırlar) | • Bir çoğaltılmış tablo üzerinde kullanılan<br></br>• Yoğun yaptığınız güncelleştirmek, tablosu üzerinde işlem<br></br>• Tablonuz overpartition: satır grupları farklı dağıtım düğümleri ve bölümleri arasında span değil |

**İpuçları:**
* Kümelenmiş bir dizin üzerinde kümelenmemiş bir dizin filtreleme için yoğun olarak kullanılan bir sütuna eklemek isteyebilirsiniz. 
* Bellek CCI olan bir tabloda yönetme dikkatli olun. Veri yüklediğinizde, bir büyük kaynak sınıftan yararlanmak için kullanıcı (veya sorgu) istiyor. Kırpma ve çok sayıda küçük sıkıştırılmış satır grupları oluşturma önlemek emin olun.
* İşlem katmanı rocks CCI ile en iyi duruma getirilmiş.
* CCI için yavaş performans satır gruplarınızı zayıf sıkıştırma nedeniyle oluşabilir. Bu gerçekleşirse, yeniden oluşturmanız veya, CCI yeniden düzenleyin. Sıkıştırılmış satır grupları başına en az 100.000 satır istiyor. Bir satır grubu 1 milyon satır idealdir.
* Artımlı yük sıklığı ve boyutuna bağlı olarak, yeniden düzenleyin veya dizinleri yeniden oluşturduğunuzda otomatikleştirmek istediğiniz. Yay temizleme her zaman yararlıdır.
* Bir satır grubunun kırpma istediğinizde stratejik olabilir. Açık satır grupları ne kadar büyük misiniz? Ne kadar veri önümüzdeki günlerde yük bekliyorsunuz?

Daha fazla bilgi edinmek [dizinleri].

## <a name="partitioning"></a>Bölümleme
(1 milyon satırdan fazla büyük) büyük olgu tablosu varsa, tablosunda bölüm. Yüzde 99 örneklerinin bölüm anahtarı tarihte bağlı olmalıdır. Değil overpartition, özellikle kümelenmiş columnstore dizini olduğunda dikkat edin.

ELT gerektiren tabloları hazırlama ile bölümleme dışında yararlı olabilir. Veri yaşam döngüsü yönetimi kolaylaştırır.
Özellikle kümelenmiş columnstore dizini bulunan verilerinizi overpartition değil dikkat edin.

Daha fazla bilgi edinmek [bölümleri].

## <a name="incremental-load"></a>Artımlı yükleme

İlk artımlı olarak verilerinizi yük kullanacaksanız, verilerinizi yükleme için daha büyük kaynak sınıfları ayırdığınızdan emin olun. SQL Data Warehouse'a ELT hatlarınızı otomatikleştirmek için PolyBase ve ADF V2 kullanmanızı öneririz.

Geçmiş verilerinizi güncelleştirmelerinde büyük toplu için önce ilgili verileri silin. Ardından yapma yeni veri toplu ekleme. Bu iki aşamalı yaklaşımı daha verimli olur.

## <a name="maintain-statistics"></a>İstatistiklerin bakımını yapın
 Otomatik istatistikleri genel olarak kullanılabilir oluncaya kadar SQL Data Warehouse istatistik el ile bakım gerektirmez. İstatistikleri olarak güncelleştirmek önemlidir *önemli* değişiklikleri verilerinizi gerçekleşir. Bu sorgu planlarınızı en iyi duruma getirme yardımcı olur. Tüm, istatistik korumak için çok uzun sürdüğünü fark ederseniz, hangi sütunların istatistiklerine sahip hakkında fazla seçici olun. 

Ayrıca, güncelleştirmelerinin sıklığını tanımlayabilirsiniz. Örneğin, burada yeni değerleri, günlük olarak eklenebilir tarih sütunlarının güncelleştirmek isteyebilirsiniz. Çoğu avantajı, birleşimler, WHERE yan tümcesinde kullanılan sütun ve GROUP BY içinde bulunan sütunlar dahil edilen sütunlar üzerinde istatistikleri sağlayarak sahip olurlar.

Daha fazla bilgi edinmek [istatistikleri].

## <a name="resource-class"></a>Kaynak sınıfı
SQL Veri Ambarı, kaynak gruplarını sorgulara bellek ayırma yöntemi olarak kullanır. Sorgu veya hızlı yükleme artırmak için daha fazla bellek ihtiyacınız varsa, daha yüksek kaynak sınıfları ayırmanız gerekir. Çevir tarafında büyük kaynak sınıflarını kullanarak eşzamanlılık etkiler. Tüm kullanıcılarınızın bir büyük kaynak sınıfına geçmeden önce dikkate alın istiyor.

Sorguları uzun sürmesine fark ederseniz, kullanıcılarınızın büyük kaynak sınıflarda çalıştırmayın denetleyin. Büyük kaynak sınıfları birçok eşzamanlılık yuvaları kullanabilir. Bunlar kuyruğuna diğer sorgular neden olabilir.

Son olarak, en iyi duruma getirilmiş işlem katmanı kullanarak, her kaynak sınıfı esnek en iyi duruma getirilmiş Katman 2,5 katı daha fazla bellek alır.

Çalışmak nasıl daha fazla bilgi [kaynak sınıfları ve eşzamanlılık].

## <a name="lower-your-cost"></a>Maliyetlerinizi alt
Anahtar, bir SQL Data Warehouse yeteneği özelliğidir [işlem kaynaklarının yönetilmesine](sql-data-warehouse-manage-compute-overview.md). Kullanmadığınızda, işlem kaynaklarını ödeme durdurur veri ambarı duraklatabilirsiniz. Performans taleplerini karşılamak için kaynakları ölçeklendirebilirsiniz. Duraklatmak için kullanmak [Azure portal](pause-and-resume-compute-portal.md) veya [PowerShell](pause-and-resume-compute-powershell.md). Ölçeklendirmek için kullanmak [Azure portal](quickstart-scale-compute-portal.md), [Powershell](quickstart-scale-compute-powershell.md), [T-SQL](quickstart-scale-compute-tsql.md), veya bir [REST API](sql-data-warehouse-manage-compute-rest-api.md#scale-compute).

Otomatik ölçeklendirme artık zamanında Azure işlevleriyle istiyor:

<a href="https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwTimerScaler%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>

## <a name="optimize-your-architecture-for-performance"></a>Mimarinizi performansı en iyi duruma getirme

SQL Database ve Azure Analysis Services içinde bir hub ve bağlı bileşen mimarisi düşünüldüğünde öneririz. Bu çözüm ayrıca SQL Database ve Azure Analysis Services'ın gelişmiş güvenlik özelliklerinden kullanırken farklı kullanıcı grupları arasında iş yükünü yalıtım sağlar. Ayrıca, kullanıcılarınıza sınırsız eşzamanlılık sağlamanın bir yolu budur.

Daha fazla bilgi edinmek [SQL Data Warehouse yararlanmak tipik mimarileri](https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/common-isv-application-patterns-using-azure-sql-data-warehouse/).

SQL veri ambarından SQL veritabanlarında, bağlı bileşen tek bir tıklatmayla dağıtın:

<a href="https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwSpokeDbTemplate%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>


<!--Image references-->
[Taslak]:media/sql-data-warehouse-cheat-sheet/picture-flow.png

<!--Article references-->
[veri yükleme]:design-elt-data-loading.md
[deeper guidance]:guidance-for-loading-data.md
[dizinleri]:sql-data-warehouse-tables-index.md
[bölümleri]:sql-data-warehouse-tables-partition.md
[istatistikleri]:sql-data-warehouse-tables-statistics.md
[kaynak sınıfları ve eşzamanlılık]:resource-classes-for-workload-management.md

<!--MSDN references-->


<!--Other Web references-->
[typical architectures that take advantage of SQL Data Warehouse]: https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/common-isv-application-patterns-using-azure-sql-data-warehouse/
[is and is not]:https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/azure-sql-data-warehouse-workload-patterns-and-anti-patterns/
[veri geçişi]:https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
[çoğaltılmış tablolar]:https://docs.microsoft.com/en-us/azure/sql-data-warehouse/design-guidance-for-replicated-tables
[dağıtılmış tabloları]:https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute
[Azure Data Lake Store]: https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-data-lake-store
[sys.dm_pdw_nodes_db_partition_stats]: https://docs.microsoft.com/en-us/sql/relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql
[sys.dm_pdw_request_steps]:https://docs.microsoft.com/en-us/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql
