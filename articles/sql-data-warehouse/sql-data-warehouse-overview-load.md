---
title: "Azure SQL Data Warehouse'a veri yükleme | Microsoft Docs"
description: "Veri SQL Data Warehouse'a veri yükleme için genel senaryolar hakkında bilgi edinin. Bunlar PolyBase, Azure blob depolama, düz dosyalar ve disk sevkiyat kullanarak içerir. Üçüncü taraf araçları da kullanabilirsiniz."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: 2253bf46-cf72-4de7-85ce-f267494d55fa
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: c4199a387f5cdbd477a5e348e48ba8e8b5900075
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="load-data-into-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse'a veri yükleme
Senaryo seçenekleri ve SQL Data Warehouse'a veri yükleme ile ilgili öneriler özeti.

Verileri yükleme en zor parçası genellikle verileri için yük hazırlanıyor. Azure basitleştirir yükleme ortak bir veri deposu olarak birçok Hizmetleri için Azure blob storage kullanarak ve Azure hizmetleri arasında iletişim ve veri taşıma düzenlemek için Azure Data Factory kullanarak. Bu işlemler, verileri paralel verileri Azure blob depolama alanından SQL Data Warehouse'a veri yüklemek için yüksek düzeyde paralel işleme (MPP) kullanan PolyBase teknolojisi ile tümleştirilir. 

Örnek veritabanları yükleme öğreticileri için bkz: [örnek veritabanları yükleme][Load sample databases].

## <a name="load-from-azure-blob-storage"></a>Azure blob depolama alanından yükleme
SQL Data Warehouse'a veri almak için en hızlı yolu, Azure blob depolama alanından verileri yüklemek için PolyBase kullanmaktır. PolyBase, Azure blob depolama alanından verileri paralel yüklemek için SQL Data Warehouse'un yüksek düzeyde paralel işleme (MPP) tasarım kullanır. PolyBase kullanmak için T-SQL komutlarıyla veya bir Azure Data Factory işlem hattı kullanabilirsiniz.

### <a name="1-use-polybase-and-t-sql"></a>1. PolyBase ve T-SQL kullanın
Yükleme işlemi özeti:

1. Verileriniz Azure blob depolama veya Azure Data Lake Store taşıyın ve metin dosyalarında saklayın.
2. Veri biçimi ve konumunu tanımlamak için SQL veri ambarında dış nesneleri yapılandırma
3. Yeni bir veritabanı tablosuna paralel veri yüklemek için bir T-SQL komutunu çalıştırın.

<!-- 5. Schedule and run a loading job. --> 

Bir öğretici için bkz: [veri yükleme Azure blob depolama alanından SQL Data Warehouse'a (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].

### <a name="2-use-azure-data-factory"></a>2. Azure Data Factory'i kullanma
PolyBase kullanmak için daha basit yol için verileri Azure blob depolama alanından SQL Data Warehouse'a veri yüklemek için PolyBase kullanan bir Azure Data Factory işlem hattı oluşturabilirsiniz. T-SQL nesneleri tanımlamak gerekmeyen beri yapılandırmak için hızlı budur. Dış veri içe aktarmadan sorgu gerekiyorsa, T-SQL kullanın. 

Yükleme işlemi özeti:

1. Verilerinizi Azure blob depolama alanına taşıma ve metin dosyalarında saklayın. Azure Data Factory şu anda PolyBase ile ADLS bağlantısı desteklemez).
2. Veri alma için bir Azure Data Factory işlem hattı oluşturun. PolyBase seçeneğini kullanın.
4. Zamanlama ve ardışık düzen çalıştırın.

Bir öğretici için bkz: [veri yükleme Azure blob depolama alanından SQL Data Warehouse'a (Azure Data Factory)][Load data from Azure blob storage to SQL Data Warehouse (Azure Data Factory)].

## <a name="load-from-sql-server"></a>SQL Server'dan yükleme
SQL Server'dan SQL Data Warehouse'a veri yüklemek için Integration Services (SSIS), aktarım düz dosyalar veya Microsoft sevk disk kullanabilirsiniz. Bkz: farklı özetini açın okuma işlemleri ve öğreticiler bağlantılar yükleniyor.

SQL veri ambarı SQL Server'dan tam veri geçiş planlamak için bkz: [geçişine genel bakış][Migration overview]. 

### <a name="use-integration-services-ssis"></a>Integration Services (SSIS) kullanın
SQL Server'a yüklemek için zaten Integration Services (SSIS) paketleri kullanıyorsanız, hedef olarak SQL Server SQL veri ambarı ve kaynak kullanmak için paketlerinizi güncelleştirebilirsiniz. Bunu yapmak, kolay ve hızlı ve verileri bulutta zaten kullanmak için yükleme işlemi geçirmek çalışmıyorsanız iyi bir seçimdir. Kolaylığını yük bu SSIS paralel olarak yük gerçekleştirmez çünkü PolyBase kullanmaktan daha yavaş olacaktır ' dir.

Yükleme işlemi özeti:

1. SQL Server örneği için kaynak ve hedef için SQL Data Warehouse veritabanına işaret edecek şekilde Integration Services paketinizi gözden geçirin.
2. Bunu zaten yoksa şemanızı SQL veri ambarı'na geçirin.
3. Değişiklik paketlerinizi eşlemesinde yalnızca SQL Data Warehouse tarafından desteklenen veri türlerini kullanın.
4. Zamanlama ve paket çalıştırın.

Bir öğretici için bkz: [veri yükleme SQL Server'dan Azure SQL veri ambarı (SSIS)][Load data from SQL Server to Azure SQL Data Warehouse (SSIS)].

### <a name="use-azcopy-recommended-for--10-tb-data"></a>AZCopy (< 10 TB veri için önerilir) kullanın
Veri boyutu < 10 TB ise, verileri SQL Server'dan düz dosyalara dışarı aktarabilir, Azure blob depolama alanına kopyalayın ve verileri SQL Data Warehouse'a veri yüklemek için Polybase'i kullanın

Yükleme işlemi özeti:

1. Verileri SQL Server'dan düz dosyalara dışarı aktarmak için bcp komut satırı yardımcı programını kullanın.
2. Azure blob depolama alanına düz dosyalardan veri kopyalamak için AZCopy komut satırı yardımcı programını kullanın.
3. SQL Data Warehouse'a veri yüklemek için Polybase'i kullanın.

Bir öğretici için bkz: [veri yükleme Azure blob depolama alanından SQL Data Warehouse'a (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].

### <a name="use-bcp"></a>BCP kullanın
Az miktarda veriniz varsa, doğrudan Azure SQL Data Warehouse'a veri yüklemek için bcp kullanabilirsiniz.

Yükleme işlemi özeti:

1. Verileri SQL Server'dan düz dosyalara dışarı aktarmak için bcp komut satırı yardımcı programını kullanın.
2. Düz dosyalardan doğrudan SQL Data Warehouse'a veri yüklemek için BCP kullanın.

Bir öğretici için bkz: [veri yükleme SQL Server'dan Azure SQL veri ambarı'na (bcp)][Load data from SQL Server to Azure SQL Data Warehouse (bcp)].

### <a name="use-importexport-recommended-for--10-tb-data"></a>İçeri/dışarı aktarma (> 10 TB veri için önerilir) kullanın
Veri boyutu > 10 TB ise ve Azure'a taşımak istediğiniz hizmet sevkiyat bizim disk kullanmanızı öneririz [içeri/dışarı aktarma][Import/Export]. 

Yükleme işlemi özeti

1. Aktarılabilir disklerde düz dosyalar için SQL Server'dan veri aktarmak için bcp komut satırı yardımcı programını kullanın.
2. Microsoft disklere birlikte.
3. Microsoft verileri SQL Data Warehouse'a veri yükler.

## <a name="load-from-hdinsight"></a>Hdınsight'ta yükleme
SQL veri ambarı veri PolyBase aracılığıyla hdınsight'tan destekler. Azure Blob depolama alanından verileri yükleniyor - Hdınsight'a verileri yüklemek için bağlanmak için Polybase'i kullanarak aynı işlemidir. 

### <a name="1-use-polybase-and-t-sql"></a>1. PolyBase ve T-SQL kullanın
Yükleme işlemi özeti:

1. Hdınsight'a verilerinizi taşıma ve metin dosyalarını saklamak ORC veya Parquet biçimi.
2. Dış nesne veri biçimi ve konumunu tanımlamak için SQL veri ambarında yapılandırın.
3. Yeni bir veritabanı tablosuna paralel veri yüklemek için bir T-SQL komutunu çalıştırın.

Bir öğretici için bkz: [veri yükleme Azure blob depolama alanından SQL Data Warehouse'a (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].

## <a name="recommendations"></a>Öneriler
Ortaklarımızın çoğunun çözümleri yüklenirken vardır. Daha fazla bilgi bulmak için bir listesini görmek bizim [çözüm ortakları][solution partners]. 

Verilerinizi ilişkisel olmayan bir kaynaktan geldiğinden ve SQL Data Warehouse'a veri yüklemek istiyorsanız, yükleme önce satırlar ve sütunlar halinde dönüştürmek gerekecektir. Dönüştürülmüş verileri bir veritabanına depolanmasını gerektirmez, metin dosyalarından depolanabilir.

Yeni yüklenen verilere ilişkin istatistikler oluşturun. Azure SQL Data Warehouse henüz istatistiklerin otomatik olarak oluşturulup güncelleştirilmesini desteklemiyor.  Sorgularınızdan en iyi performansı alabilmek için ilk yükleme işleminden sonra tüm tabloların tüm sütunlarda istatistikler oluşturmak önemli olduğunu veya verilerdeki önemli değişikliklerden.  Ayrıntılar için bkz [istatistikleri][Statistics].

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış][development overview].

<!--Image references-->

<!--Article references-->
[Load data from Azure blob storage to SQL Data Warehouse (PolyBase)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Load data from Azure blob storage to SQL Data Warehouse (Azure Data Factory)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md
[Load data from SQL Server to Azure SQL Data Warehouse (SSIS)]: ./sql-data-warehouse-load-from-sql-server-with-integration-services.md
[Load data from SQL Server to Azure SQL Data Warehouse (bcp)]: ./sql-data-warehouse-load-from-sql-server-with-bcp.md
[Load data from SQL Server to Azure SQL Data Warehouse (AZCopy)]: ./sql-data-warehouse-load-from-sql-server-with-azcopy.md

[Load sample databases]: ./sql-data-warehouse-load-sample-databases.md
[Migration overview]: ./sql-data-warehouse-overview-migrate.md
[solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[development overview]: ./sql-data-warehouse-overview-develop.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->

<!--Other Web references-->
[Import/Export]: https://azure.microsoft.com/documentation/articles/storage-import-export-service/
