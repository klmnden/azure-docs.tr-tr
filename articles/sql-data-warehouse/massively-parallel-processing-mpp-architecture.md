---
title: Azure SQL veri ambarı - MPP mimarisi | Microsoft Docs
description: Azure SQL veri ambarı yüksek performans ve ölçeklenebilirlik elde etmek için Azure depolama ile yüksek düzeyde paralel işleme (MPP) nasıl birleştirir öğrenin.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: design
ms.date: 04/17/2018
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 25dc469c9f50dee7d088fccd214020791ff73def
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66515805"
---
# <a name="azure-sql-data-warehouse---massively-parallel-processing-mpp-architecture"></a>Azure SQL veri ambarı - yüksek düzeyde paralel işleme (MPP) mimarisi
Azure SQL veri ambarı yüksek performans ve ölçeklenebilirlik elde etmek için Azure depolama ile yüksek düzeyde paralel işleme (MPP) nasıl birleştirir öğrenin. 

> [!VIDEO https://www.youtube.com/embed/PlyQ8yOb8kc]

## <a name="mpp-architecture-components"></a>MPP mimarisi bileşenleri
SQL veri ambarı mimarisi, birden fazla düğümde hesaplama verilerinin işlenmesini dağıtmak için bir genişleme yararlanır. Ölçek birimi olarak bilinen işlem gücü bir soyutlamadır bir [veri ambarı birimi](what-is-a-data-warehouse-unit-dwu-cdwu.md). Hangi etkinleştirir, ölçeklendirmek için bağımsız olarak sisteminizdeki işlem verileri depolama alanından SQL veri ambarı ayırır işlem.

![SQL Data Warehouse Mimarisi](media/massively-parallel-processing-mpp-architecture/massively-parallel-processing-mpp-architecture.png)

SQL veri ambarı düğümü tabanlı bir mimari kullanır. Uygulamaları bağlama ve tek bir veri ambarı için giriş noktası olan bir denetim düğümü için T-SQL komutlarını yürütün. Denetim düğümü sorguları paralel işleme için en iyi duruma getirir ve ardından çalışmalarını paralel olarak yürütmeleri için işlem düğümlerine operations geçirir MPP altyapısı çalıştırır. İşlem düğümleri, tüm kullanıcı verilerini Azure Storage'a depoladığınız ve paralel sorgular çalıştırın. Veri Taşıma hizmeti (DMS) doğru sonuçlar döndürebilir ve sorguları paralel olarak çalıştırmak için gerekli olan düğümler arasında veri taşıyan bir sistem düzeyinde iç hizmetidir. 

Ayrılmış depolama ve işlem ile SQL Veri Ambarı şunları yapabilir:

* Bağımsız olarak boyutu, depolama ihtiyaçlarınızı ne olursa olsun power işlem.
* Verileri taşımadan işlem gücünü büyütür veya küçültür.
* İşlem kapasitesini bilgilere dokunmadan yalnızca, böylece yalnızca depolama için ödeme yaparsınız.
* Çalışma saatleri içinde işlem kapasitesini sürdürür.

### <a name="azure-storage"></a>Azure Storage
SQL veri ambarı kullanıcı verilerinizin güvenliğini korumak için Azure depolama kullanır.  Verilerinizin depolandığı ve Azure Depolama tarafından yönetilen olduğundan, SQL veri ambarı depolama tüketiminiz için ayrı ayrı uygular. Verileri içine parçalı **dağıtımları** sistemin performansını iyileştirmek için. Veri tablosu tanımlarsanız dağıtırken kullanmak üzere hangi parçalama düzeni seçebilirsiniz. SQL veri ambarı bu parçalara ayırma örneklerini uygulamanıza destekler:

* Karma
* Hepsini Bir Kez Deneme
* Çoğaltma

### <a name="control-node"></a>Denetim düğümü

Veri ambarının Beyin denetim düğümüdür. Tüm uygulamalarla ve bağlantılarla etkileşim kuran ön uçtur. MPP altyapısı iyileştirmek ve paralel sorgular koordine etmek için denetim düğümü üzerinde çalıştırır. SQL veri ambarı'na bir T-SQL sorgusu gönderdiğiniz zaman, kontrol düğümü, her dağıtım paralel karşı çalışan sorguları dönüştürür.

### <a name="compute-nodes"></a>İşlem düğümleri

İşlem düğümleri, işlem gücünü sağlar. İşleme için işlem düğümlerine dağıtımlarını eşleyin. Daha fazla bilgi işlem kaynakları için ödeme gibi SQL veri ambarı kullanılabilir işlem düğümlerine dağıtımlarını yeniden eşler. Sayısı 1 ile 60 düğümleri aralığından işlem ve veri ambarı hizmet düzeyine göre belirlenir.

Her işlem düğümü sistem görünümlerde görülebilir bir düğüm kimliği vardır. İşlem düğümü kimliği adları sys.pdw_nodes ile başlayan sistem görünümleri $node_id sütununda bakarak görebilirsiniz. Bu sistem görünümleri listesi için bkz. [MPP sistem görünümleri](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views?view=aps-pdw-2016-au7).

### <a name="data-movement-service"></a>Veri Taşıma hizmeti
Veri Taşıma hizmeti (DMS), işlem düğümleri arasında veri taşıma koordinatları veri aktarım teknolojisidir. Bazı sorgular veri taşımayı paralel sorguları doğru sonuçlar döndürebilir emin olmak için gereklidir. Veri taşıma gerekli olduğunda DMS doğru verilere doğru konuma alır sağlar. 

## <a name="distributions"></a>Dağıtımları

Bir dağıtım depolama ve çalıştıran dağıtılmış veriler üzerinde paralel sorgular için işleme temel birimidir. SQL veri ambarı, bir sorgu çalıştırıldığında, işi paralel olarak 60 daha küçük sorgulara ayrılmıştır. Her biri 60 daha küçük sorgulara veri dağıtımları biri üzerinde çalışır. Her işlem düğümünde bir veya daha fazla 60 dağıtım yönetir. En fazla işlem kaynakları ile bir veri ambarı, işlem düğümü başına tek bir dağıtım sahiptir. En düşük bilgi işlem kaynakları ile bir veri ambarı, tüm dağıtımları bir işlem düğümünde sahiptir.  

## <a name="hash-distributed-tables"></a>Karma dağıtılmış tablolar
Bir karma dağıtılmış tablo birleşimler ve Toplamalar için en yüksek sorgu performansı büyük tablolarda teslim edebilirsiniz. 

Karma dağıtılmış bir tabloya parça verileri SQL veri ambarı belirleyici her satır için bir dağıtım atamak için bir karma işlevi kullanır. Tablo tanımında sütunlardan birinin, dağıtım sütunu olarak atanır. Karma işlevi, her satır için bir dağıtım atamak için dağıtım sütundaki değerleri kullanır.

Aşağıdaki diyagram, (tablo dağıtılmış olmayan) tam bir karma dağıtılmış tablo nasıl depolanır gösterir. 

![Dağıtılmış tablo](media/sql-data-warehouse-distributed-data/hash-distributed-table.png "dağıtılmış tablo")  

* Her satır için bir dağıtım ait.  
* Belirlenimci karma algoritma her satır için bir dağıtım atar.  
* Dağıtım başına tablosu satır sayısı, farklı tabloları boyutları tarafından gösterilen şekilde değişir.

Distinctness veri dengesizliği ve sistem üzerinde çalışan sorguları türleri gibi bir dağıtım sütun seçimi performansla ilgili önemli noktalar vardır.

## <a name="round-robin-distributed-tables"></a>Hepsini bir kez deneme dağıtılmış tablolar
Hepsini bir kez deneme tablosu oluşturmak için basit tablo ve bir hazırlama tablosuna yükler için kullanıldığında hızlı performans sunar.

Hepsini bir kez deneme dağıtılmış tablo verilerini tablo arasında ancak başka hiçbir iyileştirme olmadan eşit olarak dağıtır. Bir dağıtımı ilk rastgele seçilir ve ardından arabellek satır dağıtımlar için sırayla atanır. Hepsini bir kez deneme tablosuna veri yüklemek hızlı bir işlemdir ancak sorgu performansı genellikle karma dağıtılmış tablo ile daha iyi olabilir. Hepsini bir kez deneme tabloları birleştirme veri reshuffling gerektirir ve bu ek zaman alır.


## <a name="replicated-tables"></a>Çoğaltılmış tablolar
Çoğaltılmış bir tabloda küçük tablolar için en hızlı sorgu performansı sağlar.

Çoğaltılan bir tablo, tablonun her işlem düğümünde tam bir kopyasını önbelleğe alır. Sonuç olarak, bir tablo çoğaltma JOIN veya toplama önce işlem düğümleri arasında veri aktarımı ihtiyacını ortadan kaldırır. Çoğaltılmış tablolar içeren küçük tablolar en iyi şekilde kullanılır. Ek depolama alanı gereklidir ve büyük tablolar pratik yapan veri yazma sırasında tahakkuk ettirilen ek yükü yoktur.  

Aşağıdaki diyagramda, çoğaltılmış bir tabloda gösterilmiştir. SQL veri ambarı için her işlem düğümünde ilk dağıtım çoğaltılmış tablo önbelleğe alınır.  

![Çoğaltılan tablo](media/sql-data-warehouse-distributed-data/replicated-table.png "çoğaltılan tablo") 

## <a name="next-steps"></a>Sonraki adımlar
SQL Veri Ambarı hakkında biraz bilgi sahibi olduğunuza göre hızlıca [SQL Veri Ambarı oluşturma][create a SQL Data Warehouse] ve [örnek verileri yükleme][load sample data] hakkında bilgi edinin. Azure'da yeniyseniz yeni terimlerle karşılaşabileceğinizi için [Azure sözlüğünü][Azure glossary] yararlı bulabilirsiniz. Alternatif olarak, aşağıdaki diğer SQL Veri Ambarı Kaynakları’na göz atın.  

* [Müşteri başarı hikayeleri]
* [Bloglar]
* [Özellik istekleri]
* [Videolar]
* [Müşteri Danışma Ekibi blogları]
* [Destek bileti oluşturma]
* [MSDN forumu]
* [Stack Overflow forumu]
* [Twitter]

<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Destek bileti oluşturma]: ./sql-data-warehouse-get-started-create-support-ticket.md
[load sample data]: ./sql-data-warehouse-load-sample-databases.md
[create a SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md
[Migration documentation]: ./sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[Integrated tools overview]: ./sql-data-warehouse-overview-integrate.md
[Backup and restore overview]: ./sql-data-warehouse-restore-database-overview.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Müşteri başarı hikayeleri]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Bloglar]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Müşteri Danışma Ekibi blogları]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Özellik istekleri]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN forumu]: https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureSQLDataWarehouse
[Stack Overflow forumu]: https://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videolar]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SLA for SQL Data Warehouse]: https://azure.microsoft.com/support/legal/sla/sql-data-warehouse/v1_0/
[Volume Licensing]: https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Service Level Agreements]: https://azure.microsoft.com/support/legal/sla/
