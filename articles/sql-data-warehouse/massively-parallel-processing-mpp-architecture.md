---
title: MPP mimarisi - Azure SQL Data Warehouse? | Microsoft Belgeleri
description: "Yüksek düzeyde paralel işleme (MPP) yüksek performans ve ölçeklenebilirlik elde etmek için Azure storage ile Azure SQL Data Warehouse nasıl birleştirir öğrenin."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: architecture
ms.date: 10/23//2017
ms.author: jrj;barbkess
ms.openlocfilehash: 39092028d8317f8881b4e0772dcb87b05c064b6a
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="azure-sql-data-warehouse---massively-parallel-processing-mpp-architecture"></a>Azure SQL Data Warehouse - yüksek düzeyde paralel işleme (MPP) mimarisi
Yüksek düzeyde paralel işleme (MPP) yüksek performans ve ölçeklenebilirlik elde etmek için Azure storage ile Azure SQL Data Warehouse nasıl birleştirir öğrenin. 

## <a name="mpp-architecture-components"></a>MPP mimarisi bileşenleri
SQL veri ambarı veri hesaplama işlenmesini birden çok düğüm arasında dağıtmak için Mimari genişletme yararlanır. Veri ambarı birimi bilinen işlem gücü için bir Özet ölçek birimidir. SQL veri ambarı ayırır, ölçeklendirmek için kullanıcı olarak bağımsız olarak veri sisteminizde işlem hangi etkinleştirir depolama biriminden işlem.

![SQL Data Warehouse Mimarisi](media/massively-parallel-processing-mpp-architecture/massively-parallel-processing-mpp-architecture.png)

SQL veri ambarı göre düğüm mimarisi kullanır. Uygulamaları bağlanın ve tek veri ambarı için giriş noktası bir denetim düğümü için T-SQL komutlarını verin. Denetim düğümü sorguları paralel işleme için en iyi duruma getirir ve ardından çalışmalarını paralel yapmak için işlem düğümlerine operations geçirir MPP altyapısı çalışır. İşlem düğümlerini Azure depolama alanında tüm kullanıcı verilerini depolamak ve paralel sorgular çalıştırın. Veri Taşıma hizmeti (DMS), paralel sorgular çalıştırmak ve doğru sonuçlar döndürmek için gerekli olan düğümler arasında verileri taşır ve sistem düzeyinde çalışan bir iç hizmetidir. 

Ayrılmış depolama ve işlem ile SQL Veri Ambarı şunları yapabilir:

* Bağımsız olarak boyutu, depolama ihtiyaçlarınızı yedeklemiş güç işlem.
* Verileri taşımadan işlem gücünü büyütür veya küçültür.
* İşlem kapasitesini verilerini olduğu gibi bırakarak yalnızca depolama alanı için ödeme şekilde.
* Çalışma saatleri içinde işlem kapasitesini sürdürür.

### <a name="azure-storage"></a>Azure Storage
SQL veri ambarı Azure depolama kullanıcı verilerinizi güvenli tutmak için kullanır.  Verilerinizin depolandığı ve Azure Depolama tarafından yönetilen olduğundan, SQL Data Warehouse depolama tüketim için ayrı ayrı ücretlidir. Veri içine parçalı **dağıtımları** sistemin performansını iyileştirmek için. Tablo tanımlarken veri dağıtırken kullanmak üzere hangi parçalama düzeni seçebilirsiniz. SQL veri ambarı bu parçalama desenleri destekler:

* Karma
* Hepsini Bir Kez Deneme
* Çoğaltma

### <a name="control-node"></a>Denetim düğümü

Veri ambarının Beyin denetim düğümdür. Tüm uygulamalarla ve bağlantılarla etkileşim kuran ön uçtur. MPP altyapısı iyileştirmek ve paralel sorgular koordine etmek için Denetim düğüm üzerinde çalışır. SQL veri ambarı için bir T-SQL sorgusu gönderdiğiniz zaman, kontrol düğümü, her dağıtım paralel karşı çalışan sorguları dönüştürür.

### <a name="compute-nodes"></a>İşlem düğümleri

İşlem düğümleri hesaplama gücüne sağlar. İşleme için işlem düğümlerine dağıtımları eşleyin. Daha fazla işlem kaynaklarını ödeme olarak SQL Data Warehouse kullanılabilir işlem düğümlerine dağıtımları yeniden eşler. Sayısı 1 ile 60 düğümleri aralığından işlem ve veri ambarı için hizmet düzeyine göre belirlenir.

Her işlem düğümü sistem görünümlerde görülebilir bir düğüm kimliği vardır. Sistem görünümleri adları sys.pdw_nodes ile başlayan node_id sütununda arayarak işlem düğüm kimliği görebilirsiniz. Bu sistem görünümleri listesi için bkz: [MPP sistem görünümleri](sql-data-warehouse-reference-tsql-statements.md).

### <a name="data-movement-service"></a>Veri Taşıma hizmeti
Veri Taşıma hizmeti (DMS), veri taşıma işlem düğümleri arasında koordinatları veri aktarım teknolojisidir. Bazı sorguları paralel sorgular doğru sonuçlar döndürebilir emin olmak için veri taşıma gerektirir. Veri taşıma gerekli olduğunda, doğru konuma doğru verileri alır DMS sağlar. 

## <a name="distributions"></a>Dağıtımları

Bir dağıtım depolama ve Dağıtılmış veri üzerinde çalıştıran paralel sorgular için işleme temel birimidir. SQL veri ambarı bir sorgu çalıştığında, paralel olarak çalışan 60 küçük sorgulara iş ayrılmıştır. Her 60 küçük sorgulara veri dağıtımları biri üzerinde çalışır. Her işlem düğümünde bir veya daha fazla 60 dağıtımları yönetir. Veri ambarı en fazla işlem kaynağı işlem düğümü başına tek bir dağıtım sahiptir. Veri ambarı minimum işlem kaynağı tüm dağıtımları bir işlem düğümünde sahiptir.  

## <a name="hash-distributed-tables"></a>Karma dağıtılmış tabloları
Karma dağıtılmış tablo birleşimler ve Toplamalar için en yüksek sorgu performansı büyük tablolarda sunabilir. 

Karma dağıtılmış bir tabloya verileri için SQL Data Warehouse belirleyici biçimde her satır için bir dağıtım atamak için bir karma işlevini kullanır. Tablo tanımı sütunlardan biri dağıtım sütunu olarak atanmış. Karma işlevi, her satır için bir dağıtım atamak için dağıtım sütundaki değerleri kullanır.

Aşağıdaki diyagram, tam (dağıtılmış olmayan tablo) karma dağıtılmış bir tablo olarak nasıl depolanır gösterir. 

![Dağıtılmış tablo](media/sql-data-warehouse-distributed-data/hash-distributed-table.png "dağıtılmış tablo")  

* Her satır için bir dağıtım aittir.  
* Belirleyici karma algoritma her satır için bir dağıtım atar.  
* Dağıtım başına tablo satır sayısı tablolar farklı boyutlarda tarafından gösterildiği gibi değişir.

Distinctness, veri eğme ve sistem üzerinde çalışan sorguları türleri gibi bir dağıtım sütun seçimi için başarım düşünceleri vardır.

## <a name="round-robin-distributed-tables"></a>Hepsini dağıtılmış tabloları
Hepsini tablo oluşturmak için basit bir tablodur ve hazırlama tablosu olarak yükleri kullanıldığında hızlı performans sunar.

Hepsini dağıtılmış tablo veri tablosu arasında ancak hiçbir daha fazla iyileştirme olmadan dağıtır. Bir dağıtımı ilk rastgele seçilir ve ardından arabellek satır dağıtımları için ardışık olarak atanır. Hepsini tabloya veri yüklemek hızlı, ancak sorgu performansı çoğunlukla Dağıtılmış karma tabloları ile daha iyi olabilir. Veri reshuffling hepsini tablolarda birleştirmeler gerektirir ve bu ek zaman alır.


## <a name="replicated-tables"></a>Çoğaltılmış tablolar
Yinelenen Tablo küçük tablolar için hızlı sorgu performansı sağlar.

Çoğaltılan bir tablo tablo her işlem düğümü üzerinde tam bir kopyasını saklar. Sonuç olarak, bir tablo çoğaltma JOIN veya toplama önce işlem düğümleri arasında veri aktarmak için gereksinimini ortadan kaldırır. Çoğaltılmış tablolarda en küçük tablolarla yararlanılmıştır. Ek depolama alanı gereklidir ve büyümesine neden olan veri yazmada pratik tabloları yükleyen ücrete ek ek yüklerini vardır.  

Aşağıdaki diyagramda, çoğaltılmış bir tablo gösterir. SQL Data Warehouse için her işlem düğümü üzerinde ilk dağıtım çoğaltılmış tablo önbelleğe alınır.  

![Yinelenmiş tablo](media/sql-data-warehouse-distributed-data/replicated-table.png "yinelenmiş tablosu") 

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
[Stack Overflow forumu]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videolar]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SLA for SQL Data Warehouse]: https://azure.microsoft.com/support/legal/sla/sql-data-warehouse/v1_0/
[Volume Licensing]: http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Service Level Agreements]: https://azure.microsoft.com/support/legal/sla/
