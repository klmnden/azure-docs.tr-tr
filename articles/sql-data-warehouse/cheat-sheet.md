---
title: "Kopya sayfası Azure SQL Data Warehouse | Microsoft Docs"
description: "Bağlantılar, Azure SQL Data Warehouse çözümlerinizi hızlı bir şekilde oluşturmak için en iyi yöntemleri öğrenin."
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
ms.date: 12/14/2017
ms.author: acomet
ms.openlocfilehash: 2d17385ff255ddf7b85baa81600a2af60d015540
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="cheat-sheet-for-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse için kopya sayfası
Bu sayfa, veri ambarı çözümünüzün ana kullanım örnekleri için tasarlamanıza yardımcı olmalıdır. Bu kopya sayfası Yolculuğunuzun dünya çapındaki veri ambarı oluşturma için harika bir destek olmalıdır, ancak her adım ayrıntıları hakkında daha fazla bilgi öneririz. İlk olarak, hangi SQL veri ambarı hakkında harika bu makaleyi okuduktan öneririz  **[ve değil]**.

Tasarım SQL Data Warehouse başlatırken izlemeniz gereken işlemin bir taslak aşağıdadır.

![Taslak]

## <a name="queries-and-operations-across-tables"></a>Sorgular ve tablolar arasında işlemler

En önemli işlemleri ve veri ambarınız gerçekleşmesini sorguları önceden bilmek gerçekten önemlidir. Veri ambarı mimarisi, bu işlemler için öncelik. Ortak işlemleri örnekleri olabilir:
* Boyuta sahip bir veya iki olgu tabloları birleştirme tabloları, bir süre için bu tabloyu filtreleme ve sonuçları bir veri reyonu ekleme
* Büyük veya küçük güncelleştirmeleri olgu satış yapma
* Yalnızca veri tablolarınız için sonuna ekleme

Operations türünü bilerek tablolarınızı tasarım en iyi hale getirmenize yardımcı olur.

## <a name="data-migration"></a>Veri Taşıma

Verileriniz için ilk yükleme öneririz  **[ADLS içine]**  veya Azure Blob Depolama. Sonra verilerinizi hazırlama tablosunda SQL Data Warehouse'a veri yüklemek için Polybase kullanmanız gerekir. Aşağıdaki yapılandırma öneririz:

| Tasarlayın | Öneri |
|:--- |:--- |
| Dağıtım | Hepsini Bir Kez Deneme |
| Dizinleme | Yığın |
| Bölümleme | Hiçbiri |
| Kaynak Sınıfı | largerc veya xlargerc |

Daha fazla bilgi edinmek  **[veri geçişi], [veri yükleme]**  ile  **[daha ayrıntılı yönergeler] yüklendiği**. 

## <a name="distributed-or-replicated-tables"></a>Dağıtılmış veya çoğaltılmış tablolar

Tablo özelliklerini bağlı olarak aşağıdaki stratejilerden öneririz:

| Tür | Harika uygun | İzleme If...|
|:--- |:--- |:--- |
| Çoğaltılan | Bir yıldız Şeması 2 GB'tan daha az sıkıştırma (~ 5 x sıkıştırma) sonra depolama ile • küçük boyut tabloları |• Tablosunda birçok yazma işlemleri (örn: Ekle, upsert, silme, güncelleştirme)<br></br>• Değiştirmek veri ambarı birimi (DWU) sık sağlama<br></br>• Yalnızca 2-3 sütunları ve tablonuzu kullanın çok sayıda sütun var.<br></br>• Bir çoğaltılmış tablo dizin |
| Hepsini bir kez (varsayılan) | • Geçici/hazırlama tablosu<br></br> • Birleştirmeyi anahtar veya iyi bir aday belirgin |• Yavaş performans veri hareketi nedeniyle |
| Karma | • Olgu tabloları<br></br>• Büyük boyut tabloları |• Dağıtım anahtar güncelleştirilemez |

**İpuçları:**
* Hepsini bir kez ile başlatın, ancak bir karma Dağıtım stratejisi yoğun paralel bir mimaridir yararlanmak aspire
* Ortak karma anahtarlar aynı veri biçimini olduğundan emin olun
* Varchar biçimine dağıtmak yok
* Boyut tabloları sık birleştirme işlemleri ile bir olgu tablosu için ortak hash anahtarı ile dağıtılmış karma olabilir
* Kullanım  *[sys.dm_pdw_nodes_db_partition_stats]*  herhangi ÇARPIKLIK veri çözümlemek için
* Kullanım  *[sys.dm_pdw_request_steps]*  veri hareketleri sorguları arkasında çözümlemek için zaman yayın izlemek ve karışık işlemleri sürer. Dağıtım stratejinizi gözden geçirmek yararlıdır.

Daha fazla bilgi edinmek  **[çoğaltılmış tablolar] ve [dağıtılmış tablolar]**.

## <a name="indexing-your-table"></a>Tablonuz dizin oluşturma

Dizin oluşturma olan **harika** hızla tabloları okumak için. Kullanabileceğiniz teknolojileri gereksinimlerinize göre benzersiz bir kümesi vardır:

| Tür | Harika uygun | İzleme If...|
|:--- |:--- |:--- |
| Yığın | • Hazırlama/geçici tablo<br></br>• Küçük küçük aramaları tablolarla |• Tam tablonun tüm arama tarar |
| Kümelenmiş dizini | • En fazla 100-m satır tablosu<br></br>• Büyük tabloları (100-m'den fazla satır) yalnızca 1-2 sütunlarla yoğun olarak kullanılır |• Bir çoğaltılmış tablo üzerinde kullanılan<br></br>Birden fazla birleşim, Group By işlemleri içeren • karmaşık sorgular<br></br>• Yapma güncelleştirmeleri dizinli sütunlar üzerinde bellek sürer |
| Kümelenmiş Columnstore dizini (CCI) (varsayılan) | • Büyük tabloları (birden fazla 100-m satırlar) | • Bir çoğaltılmış tablo üzerinde kullanılan<br></br>• Yoğun yaptığınız güncelleştirmek, tablosu üzerinde işlem<br></br>• Aşırı tablonuz bölüm: satır grupları farklı dağıtım düğümleri ve bölümleri arasında span değil |

**İpuçları:**
* Kümelenmiş bir dizin üstünde filtre için yoğun olarak kullanılan bir sütuna kümelenmemiş dizin eklemek isteyebilirsiniz. 
* Bellek CCI olan bir tabloda yönetme dikkatli olun. Veri yüklediğinizde, bir büyük kaynak sınıftan yararlanmak için kullanıcı (veya sorgu) istiyor. Kırpma ve çok sayıda küçük sıkıştırılmış satır grupları oluşturma önlemek emin olun
* İşlem katmanı rocks CCI ile en iyi duruma getirilmiş
* CCI için yavaş performans satır gruplarınızı zayıf sıkıştırma nedeniyle gerçekleşebilir, yeniden oluşturmanız veya yeniden düzenlemek, CCI isteyebilirsiniz. Sıkıştırılmış satır grupları başına en az 100 k satır istiyor. Bir satır gruptaki 1-m satırlar idealdir.
* Artımlı yük sıklığı ve boyutuna bağlı olarak, yeniden düzenleyin veya dizinleri yeniden oluşturduğunuzda otomatikleştirmek istediğiniz. Yay temizleme her zaman yararlıdır.
* Bir satır grubunun kırpma istediğinizde stratejik olabilir: açık satır grupları ne kadar büyük? Ne kadar veri önümüzdeki günlerde yük bekliyorsunuz?

Daha fazla bilgi edinmek  **[dizinler]**.

## <a name="partitioning"></a>Bölümleme
Büyük olgu tabloları sahip olduğunuzda, tablo bölümleme (> 1B satırlı bir tablo). % 99 örneklerini bölüm anahtarı tarihte bağlı olmalıdır. Özellikle, bir kümelenmiş Columnstore dizini olduğunda değil atlayarak bölüm için dikkatli olun.
ETL gerektiren tabloları hazırlama ile bölümleme dışında yararlı olabilir. Veri yaşam döngüsü yönetimi kolaylaştırır.
Özellikle bir kümelenmiş Columnstore dizini bulunan verilerinizi overpartition değil dikkat edin.

Daha fazla bilgi edinmek  **[bölümleri]**.

## <a name="incremental-load"></a>Artımlı yükleme

İlk olarak, verilerinizi yükleme için daha büyük kaynak sınıfları ayırdığınızdan emin olmalısınız. SQL DW ETL işlem hatlarınızı otomatikleştirmek için Polybase ve ADF V2 kullanmanızı öneririz.

Geçmiş verilerinizi güncelleştirmelerinde büyük toplu için önce ilgili verileri silme öneririz. Bir toplu yapabileceğiniz sonra yeni veri Ekle. Bu bir 2 adımlı yaklaşım daha verimli olur.

## <a name="maintain-statistics"></a>İstatistiklerin bakımını yapın
Otomatik istatistikleri genellikle yakında kullanılabilir olacak. O zamana kadar SQL Data Warehouse istatistik el ile bakım gerektirmez. İstatistikleri olarak güncelleştirmek önemlidir **önemli** değişiklikleri verilerinizi gerçekleşir. Bu sorgu planlarınızı en iyi duruma getirme yardımcı olur. Tüm, istatistik korumak için çok uzun sürdüğünü fark ederseniz, hangi sütunların istatistiklerine sahip hakkında fazla seçici olun isteyebilirsiniz. Ayrıca, güncelleştirmelerinin sıklığını tanımlayabilirsiniz. Örneğin, yeni değer eklenme ihtimali olan tarih sütunlarını her gün güncelleştirmeyi tercih edebilirsiniz. Çoğu avantajı, birleşimler, WHERE yan tümcesinde kullanılan sütun ve GROUP BY içinde bulunan sütunlar dahil edilen sütunlar üzerinde istatistikleri sağlayarak sahip olurlar.

Daha fazla bilgi edinmek  **[istatistikleri]**.

## <a name="resource-class"></a>Kaynak sınıfı
SQL Veri Ambarı, kaynak gruplarını sorgulara bellek ayırma yöntemi olarak kullanır. Sorgu veya hızlı yükleme artırmak için daha fazla bellek ihtiyacınız varsa, daha yüksek kaynak sınıfları ayırmanız gerekir. Çevir tarafında büyük kaynak sınıflarını kullanarak eşzamanlılık etkiler. Tüm kullanıcılarınızın bir büyük kaynak sınıfına geçmeden önce dikkate almak istediğiniz.

Sorguları uzun sürmesine fark ederseniz, kullanıcılarınızın büyük kaynak sınıflarda çalıştırmayın denetleyin. Büyük kaynak sınıfları birçok eşzamanlılık yuvaları kullanabilir. Bunlar kuyruğuna diğer sorgular neden olabilir.

Son olarak, en iyi duruma getirilmiş işlem katmanı kullanırsanız, her kaynak sınıfı daha fazla bellek x 2.5 esnek en iyi duruma getirilmiş katmanında alır.

Çalışmak nasıl daha fazla bilgi  **[kaynak sınıfları ve eşzamanlılık]**.

## <a name="lower-your-cost"></a>Maliyetlerinizi alt
SQL Veri Ambarı’nın önemli özelliklerinden biri, kullanmadığınız zaman duraklatarak işlem kaynakların maliyetlerini durdurma şansına sahip olmanızdır. Bir diğer önemli özellik ise kaynakları ölçeklendirmektir. Duraklatma ve Ölçeklendirme işlemlerini Azure portal üzerinden veya PowerShell komutları aracılığıyla yapabilirsiniz.

Otomatik ölçeklendirme artık zamanında Azure işlevleriyle istiyor:

<a href="https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwTimerScaler%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>

## <a name="optimize-you-architecture-for-performance"></a>Mimarisi performansı en iyi duruma getirme

SQL database ve Azure Analysis Services içinde bir Hub ve bağlı bileşen mimarisi dikkate öneririz. Bu çözüm ayrıca bazı yararlanarak SQL DB güvenlik özelliklerinden Gelişmiş sırada farklı kullanıcı grupları ile Azure Analysis Services arasında iş yükünü yalıtım sağlar. Ayrıca, kullanıcılarınıza sınırsız eşzamanlılık sağlamanın bir yolu budur.

Daha fazla bilgi edinmek  **[SQL DW yararlanarak tipik mimarileri]**.

Bir tıklatın, bağlı bileşen SQL DW SQL DB veritabanlarından içinde dağıtın:

<a href="https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwSpokeDbTemplate%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>


<!--Image references-->
[Taslak]:media/sql-data-warehouse-cheat-sheet/picture-flow.png

<!--Article references-->
[veri yükleme]:./design-elt-data-loading.md
[daha ayrıntılı yönergeler]: ./guidance-for-loading-data.md
[dizinler]:./sql-data-warehouse-tables-index.md
[bölümleri]:./sql-data-warehouse-tables-partition.md
[istatistikleri]:./sql-data-warehouse-tables-statistics.md
[kaynak sınıfları ve eşzamanlılık]:./sql-data-warehouse-develop-concurrency.md

<!--MSDN references-->


<!--Other Web references-->
[SQL DW yararlanarak tipik mimarileri]: https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/common-isv-application-patterns-using-azure-sql-data-warehouse/
[ve değil]:https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/azure-sql-data-warehouse-workload-patterns-and-anti-patterns/
[veri geçişi]:https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
[çoğaltılmış tablolar]:https://docs.microsoft.com/azure/sql-data-warehouse/design-guidance-for-replicated-tables
[dağıtılmış tablolar]:https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute
[ADLS içine]: https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-store
[sys.dm_pdw_nodes_db_partition_stats]: https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql
[sys.dm_pdw_request_steps]:https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql
